As discussed in [one of my last blog posts](/archive/2014/02/03/collecting-performance-data-with-varnish-and-statsd.html), we really like [New Relic](http://newrelic.com/) for performance metrics and use it a lot. Unfortunetaly there isn't an extension for [HHVM](http://hhvm.com/) (yet) and HHVM is becoming an important part in our setup.

But - a big great coincidence - New Relic released an [Agent SDK](https://docs.newrelic.com/docs/features/agent-sdk) and with that, an [example extension for HHVM](http://blog.newrelic.com/2014/02/10/agentsdk-blog-post/) and WordPress. That was a great start for me to get behind the whole thing.

I had mainly to goals for this. Have an API compatible extension to [the official New Relic PHP extension](https://docs.newrelic.com/docs/php/the-php-api), so that we can use the same code for the Zend PHP Engine and HHVM. In our Symfony2 projects, we use the [Ekino New Relic Bundle](https://github.com/ekino/EkinoNewRelicBundle) and we didn't want to have to change that. And as a bonus, make profiling as informative as possible so that we can see which part of the call takes how long.

[Writing hhvm extension](https://github.com/facebook/hhvm/wiki/Extension-API) is surprisingly easy (at least if you have a template ;)), you can even write the easy stuff in PHP (or [Hack](http://hacklang.org/)) and only for the more advanced stuff switch to C++, see [newrelic.php](https://github.com/chregu/hhvm-newrelic-ext/blob/master/newrelic.php) as an example of that.

New Relic is continously adding new features to their Agent SDK and today, we're almost API complete (the only important thing missing is being able to change the license key or the appname from within HHVM).

If you want to use New Relic on your HHVM server, just compile and install the extension from [github.com/chregu/hhvm-newrelic-ext/](https://github.com/chregu/hhvm-newrelic-ext/) and you're good to go. You can send attributes, errors and it will also send the time your requests needed. 

## Profiling data

Of course, one of the main features of New Relic is to see, where your requests spend their time. That needed a little bit more work. And I came up with two solutions.

Solution 1 needs a patched and recompiled HHVM and hooks into the HOTPROFILING of HHVM. This is disabled by default, so you have to enable this with `cmake -D HOTPROFILER:BOOL=ON .` before compiling HHVM (you also have to do that, if you want to use xhprof). And of course [my patched HHVM](https://github.com/chregu/hhvm/tree/newrelic-profiling), which adds the possibility to send profiling data to New Relic. You can then enable it with `newrelic_profiling_enable($level)` in your code, where level is the amount of levels (depth) you want to collect. If you set that too high, you won't see everything.

If you enable profiling with `newrelic_profiling_enable()`, it will make your requests approx. 20-50% slower (but still much faster than with Zend PHP), so it's advised not to use that all the time. If you don't use that call, I couldn't see any performance difference to an HHVM without HOTPROFILER compiled in (I'm sure there is, but it must be really low).

Solution 2 doesn't need a patched HHVM (just the HHVM New Relic extension), but will make it much slower (2-3 times as slow, depending on how many function calls you have). It uses the HHVM function `fb_setprofile`, which will get called on every function request and collects data this way. It's also enabled with `newrelic_profiling_enable($level)`, the extension automatically chooses the better option.

## Summary

With this new relic extension and the patched HHVM, we can now collect almost the same data into New Relic as with the official PHP extension, which helps a lot in finding performance problems and bottlenecks. It's not perfect yet, but we will get there (unless New Relic publishes an official HHVM extension before that, which would be great of course as well).

I also want to thank New Relic for their quick support and fixing when I had questions or found something dubious. Really appreciated (but of course it would sometimes have been easier, if the source code to the Agent SDK was available ;))

Feel free to use and extend the extension and patches of course are always welcome.