---
layout: post
title: Debugging PHP Exceptions using Xdebug
date: '2013-01-24T23:28:00+08:00'
tags: [xdebug,PHP,Laravel,PHP Debugging]
---

The only reason I used [Xdebug](http://xdebug.org/index.php){:rel='nofollow'} before was because of the cool colors when I `var_dump` things. Then I discovered Xdebug could be way cooler than that.



When you encounter an exception in PHP the only error that appears is:

{% highlight text %}
{% raw %}
PHP ErrorException:  Warning: Invalid argument supplied for foreach() in E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Gramm ars\Grammar.php line 194 in E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Grammars\Grammar.php on line 194
{% endraw %}
{% endhighlight %}

I didn't know what the hell made that was going on for that to be thrown. So Xdebug comes in! Add this to your `php.ini` configuration (download the xdebug extension if you haven't):

{% highlight text %}
{% raw %}
xdebug.show_exception_trace=1
{% endraw %}
{% endhighlight %}

After restarting PHP-CGI I got a stack trace of how I got the exception:

{% highlight text %}
{% raw %}
PHP ErrorException:  Warning: Invalid argument supplied for foreach() in E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Grammars\Grammar.php line 194 in E:\dev\vendor\laravel\framework\
src\Illuminate\Database\Query\Grammars\Grammar.php on line 194
PHP Stack trace:
PHP   1. {main}() E:\dev\artisan:0
PHP   2. Symfony\Component\Console\Application->run() E:\dev\artisan:73
PHP   3. Symfony\Component\Console\Application->doRun() E:\dev\vendor\symfony\console\Symfony\Component\Console\Application.php:106
PHP   4. Illuminate\Console\Command->run() E:\dev\vendor\symfony\console\Symfony\Component\Console\Application.php:193
PHP   5. Symfony\Component\Console\Command\Command->run() E:\dev\vendor\laravel\framework\src\Illuminate\Console\Command.php:95
PHP   6. Illuminate\Console\Command->execute() E:\dev\vendor\symfony\console\Symfony\Component\Console\Command\Command.php:240
PHP   7. ScrapeResults->fire() E:\dev\vendor\laravel\framework\src\Illuminate\Console\Command.php:107
PHP   8. Results\Category->update() E:\dev\app\commands\ScrapeResults.php:120
PHP   9. Illuminate\Database\Eloquent\Model->__call() E:\dev\app\commands\ScrapeResults.php:120
PHP  10. call_user_func_array() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Eloquent\Model.php:1174
PHP  11. Illuminate\Database\Eloquent\Builder->__call() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Eloquent\Model.php:0
PHP  12. call_user_func_array() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Eloquent\Builder.php:443
PHP  13. Illuminate\Database\Query\Builder->update() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Eloquent\Builder.php:443
PHP  14. Illuminate\Database\Query\Grammars\Grammar->compileUpdate() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Builder.p hp:1045
PHP  15. Illuminate\Database\Query\Grammars\Grammar->compileWheres() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Grammars\ Grammar.php:532
PHP  16. Symfony\Component\HttpKernel\Debug\ErrorHandler->handle() E:\dev\vendor\laravel\framework\src\Illuminate\Database\Query\Grammars\Grammar.php:194
{% endraw %}
{% endhighlight %}

Now I know which specific lines were called which lead to that exception. Yey!
