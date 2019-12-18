---
title: "A Poor Guy's Local PHP Apm"
date: 2019-12-18T07:58:36-08:00
draft: false
tags:
  - instrumentation
  - opencensus
  - jaeger
  - php
---

I've seen the benefits of instrumenting code for a very long time: with PHP, Go, Frontend Javascript, and more recently, NodeJS.

When you have access to a full APM solution like I do at work (Newrelic), or on the side (with [Elastic APM](https://www.elastic.co/products/apm)), it becomes easy to see at a glance not only what transactions (API calls, queued jobs, asynchronous processes, etc) are taking a long time, but also _why_.

I ran into a situation where one of [my first projects](https://www.congregateonline.com/) experienced a bottleneck
with slow page loads. In Newrelic Lite (the free forever plan), I could tell that on average, we were making
select queries on a table over a 100 times per page load on average. Most of this code was written in 2010
(my excuse, although not a very good one), and at a glance it wasn't easy to tell _where_ exactly the problem was, although I knew what it was: [ORM lazy loading in a loop](https://www.mehdi-khalili.com/orm-anti-patterns-part-3-lazy-loading).

Upgrading to the a pricier version of Newrelic was not an option. I had a local version of the application working with docker-compose: was there perhaps a solution I could run locally, while pinpointing the exact problem? There had to be.

I tried:

- **XDebug**. The holy grail. Grind viewers were less than helpful getting an exact call sequence. Not fun.
- **PHP APM (https://pecl.php.net/package/APM)**. This hasn't been updated since 2017, fails to build on [PHP 7.3](https://github.com/patrickallaert/php-apm/issues/73), and I experienced random application crashes on 7.2.
- **Pinpoint APM (https://naver.github.io/pinpoint/)**. Let's just say there was a lot of headscratching, especially around the install. I sort of got it working, but didn't get any useful data. Also, more random crashes.

I tried other APM solutions, most of them Saas, but the hurdle was either installing them, the signup/activation process, or the knowledge that I wouldn't be able to reliable diagnose another problem after the trials ran out.

But wait. I'd done some work with OpenTracing and OpenCensus before. Could I...?

[YES!](https://github.com/census-instrumentation/opencensus-php)

I modified my Dockerfile very simply:

{{< highlight docker >}}
FROM php:7.3-apache

# Everything else
RUN apt-get update && apt-get install -y zlib1g-dev libzip-dev zip unzip && \
    docker-php-ext-install iconv bcmath zip mysqli opcache pdo pdo_mysql && \
    echo "done!"

# OpenCensus
RUN curl -L https://github.com/census-instrumentation/opencensus-php/archive/master.zip -o opencensus.zip && unzip opencensus.zip
RUN (cd opencensus-php-*/ext && phpize && ./configure --enable-opencensus && make && make test && make install)
RUN docker-php-ext-enable opencensus
{{< /highlight >}}

I added both `opencensus/opencensus` and `opencensus/opencensus-exporter-zipkin` (for [Jaeger](https://www.jaegertracing.io/)) to composer. I didn't use `opencensus/opencensus-exporter-jaeger` at the time due to [this issue](https://github.com/census-ecosystem/opencensus-php-exporter-jaeger/issues/7).

```bash
composer require --dev 'opencensus/opencensus:^0.5.2' 'opencensus/opencensus-exporter-zipkin:^0.1.0'
```

Followed by changing my docker-compose.yml:

```yaml
version: '3.6'
services:
  app:
    build: .
    links:
      - db
    ports:
      - '7999:80'
    expose:
      - '80'
    volumes:
      - .:/var/www/html
    environment:
      - DB_HOST=db
      - DB_NAME=appdb
      - DB_USER=root
      - DB_PASS=phpapptest
      - DB_PORT=3306
      - KOHANA_ENV=DEVELOPMENT
  db:
    image: 'mariadb:10'
    ports:
      - '3360:3306'
    volumes:
      - ./build/phpunit_database.sql:/docker-entrypoint-initdb.d/db.sql
      - ./docker/mysql:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: phpapptest
      MYSQL_DATABASE: appdb
  opencensus:
    image: jaegertracing/all-in-one:latest
    environment:
      COLLECTOR_ZIPKIN_HTTP_PORT: '9411'
    ports:
      - 9400:16686
      - 9414:9411
    expose:
      - 5775/udp
      - 6831/udp
      - 6832/udp
      - 5778
      - 16686
      - 14268
      - 9411
```

With that, I had the Jaeger endpoint running at `http://localhost:9400`

In my application entry point, right after including `vendor/autoload.php`, I added the below:

There's some gore in there because we're in the middle of replacing [~~Kohana~~ Koseven](https://koseven.ga) with [Laravel](https://laravel.io).

```php
<?php
use OpenCensus\Trace\Span;
use OpenCensus\Trace\Tracer;
use OpenCensus\Trace\Exporter\ZipkinExporter;

if (extension_loaded('opencensus')) {
    // Inbuilt instrumentation
    OpenCensus\Trace\Integrations\Mysql::load();
    OpenCensus\Trace\Integrations\PDO::load();
    OpenCensus\Trace\Integrations\Eloquent::load();
    OpenCensus\Trace\Integrations\Curl::load();
    // Other
    opencensus_trace_method(trim(PageManager::class, '\\'), '__construct', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), '__construct'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(PageManager::class, 'get', function($scope, $id){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'get'),
            'attributes' => compact('id'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(PageManager::class, 'init', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'init'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(PageManager::class, 'top', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'top'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(PageManager::class, 'topMenu', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'topMenu'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    // Views
    opencensus_trace_method(Kohana_View::class, 'factory', function($scope, $file){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'find_all'),
            'attributes' => array_map('strval', compact('file')),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_View::class, 'render', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'render'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    // Database and ORM
    opencensus_trace_method(Kohana_ORM::class, 'find_all', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'find_all'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_ORM::class, 'find', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'find'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    $databaseClasses = [Kohana_Database::class, Kohana_Database_MySQLi::class, Database_CMySQLi::class];
    foreach ($databaseClasses as $databaseClass) {
        opencensus_trace_method($databaseClass, 'query', function($scope, $type, $sql){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'query'),
                'attributes' => ['query' => $sql],
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($databaseClass, 'begin', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'begin'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($databaseClass, 'commit', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'commit'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($databaseClass, 'rollback', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'rollback'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
    }
    // Controllers
    $controllerClasses = [Kohana_Controller::class, Controller::class, Base_Controller::class];
    foreach ($controllerClasses as $controllerClass) {
        opencensus_trace_method($controllerClass, '__construct', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), '__construct'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($controllerClass, 'before', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'before'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($controllerClass, 'after', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'after'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
        opencensus_trace_method($controllerClass, 'execute', function($scope){
            return [
                'name' => sprintf('%s::%s', get_class($scope), 'execute'),
                'kind' => Span::KIND_CLIENT,
            ];
        });
    }
    opencensus_trace_method(Controller_Page::class, 'setup', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'setup'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    // Request execution and responses
    opencensus_trace_method(Kohana_Request::class, 'execute', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'execute'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Request_Client::class, 'execute', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'execute'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Request_Client::class, 'execute_request', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'execute_request'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Request::class, 'process', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'process'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Response::class, 'body', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'body'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Response::class, 'send_headers', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'send_headers'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    // Routing
    opencensus_trace_method(Kohana_Route::class, 'set', function($scope, $name){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'name'),
            'attributes' => array_map('strval', compact('name')),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(Kohana_Route::class, 'cache', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'cache'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    // Laravel
    opencensus_trace_method(\CongreGATEv7\Models\User::class, 'findForPassport', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'findForPassport'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\CongreGATEv7\Models\User::class, 'validateForPassportPasswordGrant', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'validateForPassportPasswordGrant'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\CongreGATEv7\Models\User::class, 'getAuthIdentifierName', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'getAuthIdentifierName'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\Illuminate\Auth\EloquentUserProvider::class, 'retrieveById', function($scope, $identifier){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'retrieveById'),
            'attributes' => array_map('strval', compact('identifier')),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\Illuminate\Auth\EloquentUserProvider::class, 'retrieveByToken', function($scope, $identifier, $token){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'retrieveByToken'),
            'attributes' => array_map('strval', compact('identifier', 'token')),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\Illuminate\Auth\EloquentUserProvider::class, 'updateRememberToken', function($scope, $user, $token){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'updateRememberToken'),
            'attributes' => array_map('strval', compact( 'token')),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\Illuminate\Auth\EloquentUserProvider::class, 'retrieveByCredentials', function($scope, $user, $token){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'retrieveByCredentials'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    opencensus_trace_method(\Illuminate\Auth\EloquentUserProvider::class, 'validateCredentials', function($scope){
        return [
            'name' => sprintf('%s::%s', get_class($scope), 'validateCredentials'),
            'kind' => Span::KIND_CLIENT,
        ];
    });
    Tracer::start(new ZipkinExporter('congregate', 'http://opencensus:9411/api/v2/spans'));
}
```

For a case where I needed a stacktrace (calling `debug_backtrace()` or `debug_print_backtrace()` inside one of the scoped lambda functions above WILL crash the application), I had to do something this ugly:

```php
<?php

    public function find_all()
    {
        if (extension_loaded('opencensus')) {
            ob_start();
            debug_print_backtrace(DEBUG_BACKTRACE_IGNORE_ARGS);
            $trace = ob_get_clean();
            return Tracer::inSpan([
                'name' => 'Model_Page::find_all.instrumented',
                'attributes' => [
                    'trace' => $trace,
                ]
            ], function() {
                return parent::find_all();
            });
        }

        return parent::find_all();
    }
```

A couple days ago, I was told calendar feeds were loading very slowly. All I had to do was look in Newrelic:

![Newrelic APM Stats](/posts/post1/newrelic-breakdown.png)

Not good. Given these relationships:

```
Event
(belongsTo) EventCategory
(hasMany) EventUnavailabilities
```

on average we were querying for events 20 times, categories 57 times, unavailabilities
(time ranges to exclude events) **359** times, and members and families for birthdays and anniversaries on the member calendar
roughly 15-20 times on average. NOT GOOD.

Looking locally, I saw this which has a whopping 678 spans:

![Jaeger](/posts/post1/jaeger-take-1.png)

A quick rewrite later, by prefetching, I got this:

![Jaeger](/posts/post1/jaeger-take-2.png)

322 spans, which looks much saner.

Even Newrelic agrees:

![Newrelic APM Stats](/posts/post1/newrelic-breakdown-2.png)

And that, friends, is one way to run a local APM for PHP.
