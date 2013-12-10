---
layout: post
title: Use Monolog to send your logs to Papertrail
date: '2013-07-10T21:44:00+08:00'
tags: [php, monolog, papertrailapp, laravel]
image:
  feature: monolog.png
---

<a href="https://github.com/Seldaek/monolog" rel="nofollow">Monolog</a> provides many handlers and formatters. We'll use the `SocketHandler` and `LineFormatter` to forward ours logs to <a href="https://papertrailapp.com/?thank=93d0b9" rel="nofollow">Papertrail</a>.


{% highlight php startinline %}
$appName = 'MyPHPApp';
$port = 1234;
$connection = 'udp://logs.papertrailapp.com:' . $port;


$msgFormat = '<22>%datetime% '
        . $appName
        . ' %channel%: Level[%level_name%]: %message% %context% %extra%';
$dateFormat = 'M d H:i:s';

$handler = new Monolog\Handler\SocketHandler($connection);
$handler->setFormatter(new Monolog\Formatter\LineFormatter($msgFormat, $dateFormat));

$writer = new Monolog\Logger($appName);
$writer->pushHandler($handler);
{% endhighlight %}

Laravel
-------

If you're using the awesome Laravel 4 framework, replace these line from your `app/start/global.php` file:

{% highlight php startinline %}
//$logFile = 'log-'.php_sapi_name().'.txt';
//Log::useDailyFiles(storage_path().'/logs/'.$logFile);

$appName = 'MyPHPApp';
$port = 1234;
$connection = 'udp://logs.papertrailapp.com:' . $port;

$format = '<22>%datetime% '
        . $appName
        . ' %channel%: Level[%level_name%]: %message% %context% %extra%';
$dateFormat = 'M d H:i:s';

$handler = new Monolog\Handler\SocketHandler($connection);
$handler->setFormatter(new Monolog\Formatter\LineFormatter($format, $dateFormat));

Log::getMonolog()->pushHandler($handler);
{% endhighlight %}

**OR** If you prefer the elegant way:

{% highlight php startinline %}
// app/config/papertrail.php

return array(
    'url' => 'udp://logs.papertrailapp.com:1234',
    'format' => array(
        'message' => '<22>%datetime% MyLaravelApp %channel%: Level[%level_name%]: %message% %context% %extra%',
        'date' => 'M d H:i:s'
    )
);

// app/start/global.php

$papertrail = Config::get('papertrail');

$handler = new Monolog\Handler\SocketHandler($papertrail['url']);
$handler->setFormatter(
    new Monolog\Formatter\LineFormatter(
        $papertrail['format']['message'],
        $papertrail['format']['date']
    )
);

Log::getMonolog()->pushHandler($handler);
{% endhighlight %}

Links
-----

- <a href="https://papertrailapp.com/?thank=93d0b9" rel="nofollow">Don't have a Papertrail account yet? Register here</a>
- <a href="http://help.papertrailapp.com/kb/configuration/configuring-centralized-logging-from-php-apps" rel="nofolllow" target="_blank">Configuring centralized logging from PHP apps</a>
