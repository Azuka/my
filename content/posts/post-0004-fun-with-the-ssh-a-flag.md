---
date: "2022-12-19T23:37:38-08:00"
draft: false
tags:
- ssh
- deployerphp
title: Fun with php deployer and ssh
---


A long, long time ago when my daily driver was a Windows machine,
I used to deploy websites over ftp, then sftp. I remember SmartFTP being one of the first
software tools I ever purchased.

In time (around 2011), I got introduced to [Capistrano](https://capistranorb.com/) when I started doing continuous integration/delivery and setting up multiple environments for my projects.

Getting a consistent ruby ecosystem working for mostly php projects was annoying as I moved between Jenkins, Wercker (pre-Oracle), Shippable (before they got acquired by JFrog) and finally CircleCI. I did have Ruby dependencies because [Compass](https://github.com/Compass/compass) was all the rage, but eventually, it became annoying as browsers stopped needing prefixes, and Sass got ported to other languages.

I started doing deploys somewhat like this:

```shell
- if [ "$BRANCH" != "master" ]; then ssh -t $DEPLOY_USER@$DEPLOY_HOST "cd ~/app_staging && git fetch --all && git checkout --force $COMMIT && php composer.phar install --no-dev --optimize-autoloader" && grunt; curl -H "x-api-key:xxxx"  -d "deployment[app_name]=App-other" -d "deployment[description]=This deployment can be viewed at ${BUILD_URL}" -d "deployment[revision]=${COMMIT}" -d "deployment[changelog]=${SHIPPABLE_BUILD_ID}" -d "deployment[user]=${USER}" https://api.newrelic.com/deployments.xml; fi
- if [ "$BRANCH" == "master" ]; then ssh -t $DEPLOY_USER@$DEPLOY_HOST "cd ~/app_system && git fetch --all && git checkout --force $COMMIT && php composer.phar install --no-dev --optimize-autoloader" && grunt; curl -H "x-api-key:xxxx"  -d "deployment[app_name]=App-production" -d "deployment[description]=This deployment can be viewed at ${BUILD_URL}" -d "deployment[revision]=${COMMIT}" -d "deployment[changelog]=${SHIPPABLE_BUILD_ID}" -d "deployment[user]=${USER}" https://api.newrelic.com/deployments.xml; fi

```

I missed capistrano multistage though, because there was no easy way to roll back deployments other than manually, and on projects where I had to deploy to multiple servers, this became unwieldy very quickly.

Enter [deployer](https://deployer.org).

It had an API that reminded me of Capistrano: multiple hosts and stages, with support for custom tasks and symlinked releases that didn't get swapped until deployment completion.

I started at v3 and have gone through 4 major version upgrades with [some](https://github.com/deployphp/deployer/pull/1160) [battle](https://github.com/deployphp/deployer/issues/1156) [scars](https://github.com/deployphp/deployer/issues/3337) to show.

The latest one was a simple task definition which I used to keep secrets pulled from a separate repo (admittedly not a very good idea, but one that works for now):

```php
task('env:secrets', function () {

    run('(cd ~/.env-secrets && git pull)');

})->desc('Pull secret files.');
```

I kept getting a fatal pull error even though I could ssh into all the affected servers and literally run `(cd ~/.env-secrets && git pull)` without any problems. I could also run the deploy command locally without any issues, ie:

```shell
./vendor/bin/dep env:secrets stage=staging --revision=$(git rev-parse HEAD) -vvv
```

Using CircleCI's ssh mode to troubleshoot got me nothing. Cue the dreaded list of CI git commits a la `try deploy x16`.

I fix CI systems at work quite a bit, and in almost every case, the problem is right there in the terminal output, so it was quite ironic when I took a closer look and saw:

```shell
ssh '-F' '/home/circleci/.ssh/config' '-A' '-o' 'ControlMaster=auto' '-o' 'ControlPersist=60' '-o' 'ControlPath=/dev/shm/***@****' '***@****' ': 34bf7dc0417ecfc06846; bash -ls'
```

Using those ssh args, and running a git pull, I got the expected fatal error: bingo!

After reading the _fine_ manual and narrowing the culprit to [the -A flag](https://superuser.com/questions/1376111/with-ssh-what-does-the-a-flag-do), it was as simple as updating the task to:

```php
task('env:secrets', function () {
    run('SSH_AUTH_SOCK="" (cd ~/.env-secrets && git pull)');
})->desc('Pull secret files.');
```

Deployer does have a `->setForwardAgent(false)` host flag, but 4 major upgrades have taught me not to mess too much with the internals when they _just_ work.

As always, it's the little things that cost you hours of debugging, especially when your internet is spotty.
