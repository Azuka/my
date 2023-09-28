---
date: "2020-02-28T09:42:14-08:00"
draft: false
tags:
- go
title: That Minor SNAFU With Go Types
---

Today's specimen:

```go
type expectedError struct {
	error
}

func (e *expectedError) Cause() error {
	return e.error
}

func IsExpectedError(err error) bool {
	_, ok := err.(*expectedError)
	return ok
}
```

In one of my projects, I'm using [consul for distributed locking](https://link.medium.com/rzNnWtY6r4), to prevent
processing an item twice from a queue worker.

My implementation looked something like the below:

```go
import (
	"context"
	"github.com/hashicorp/consul/api"
	"github.com/pkg/errors"
	"os"
	"time"
)

type lock struct {
	db     datasource.LockStore
	consul *api.Client
}

func (l *lock) AcquireLock(ctx context.Context, action string, f func() error) error {

	lck, err := l.db.GetLock(ctx, action)

	if err != nil {
		return errors.Wrap(err, "unable to query database for lock")
	}

	if lck != nil {
		return &expectedError{
			errors.Errorf("item %s already processed at %s", action, lck.CreatedAt.Format(time.RFC3339)),
		}
	}

	lck = &models.Lock{
		Action: action,
	}

	// nolint
	hn, _ := os.Hostname()

	opts := &api.LockOptions{
		Key:          lck.Action,
		Value:        []byte("set by " + hn),
		SessionTTL:   "120s",
		LockWaitTime: time.Second * 2,
	}

	cLck, err := l.consul.LockOpts(opts)

	if err != nil {
		return errors.WithStack(err)
	}

	_, err = cLck.Lock(nil)

	if err != nil {
		return errors.WithStack(err)
	}

	defer func() {
		_ = cLck.Unlock()
	}()

	if err = f(); err != nil {
		return errors.WithStack(err)
	}

	return &expectedError{
		errors.Wrapf(l.db.CreateLock(ctx, lck),
			"unable to save lock to database for %s at %s",
			lck.Action, lck.CreatedAt.Format(time.RFC3339))
	}
}
```

What could go wrong? Everything.

Something like

```go
// Process message in a lock
err = w.lock.AcquireLock(ctx, action, func() error {
	return w.sendMessage(ctx, m, accountName)
})

if err != nil {

	// This detects a lock-only error and uses that
	if locking.IsExpectedError(err) {
		return nil
	}

	return err
}

doSomething()
```

would never execute `doSomething()`, even if `sendMessage()` succeeds, because I decided to be lazy and wrap the result of `CreateLock()` without checking for a non-nil error.

Tiny bug, annoying to troubleshoot, until I wrote tests, which makes me wonder why I didn't write any in the first place.
