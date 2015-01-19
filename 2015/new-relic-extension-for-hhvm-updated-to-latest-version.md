Since I [first published my New Relic extension for HHVM](/archive/2014/03/27/hhvm-and-new-relic.htm), I didn't do much and we stayed on that HHVM version (3.1). A lot changed in [HHVM](http://hhvm.com/), which was one of the reasons I didn't do much: Too many changes in there, but it was for the good. 

# (Soon) no HHVM recompling needed anymoer
Since HHVM 3.4 it's theoretically possible to have your own external profiler for function level profiling (like xhprof or xdebug) without having to recompile HHVM itself. Unfortunately it wasn't perfect (or I couldn't make it running), but there's a patch in the master branch now (the upcoming 3.6), which seems to solve that problem. So I worked a little bit [on my extension](https://github.com/chregu/hhvm-newrelic-ext/) in the last few days and I adjusted a lot of things and improved some other stuff.

It's no secret, that [New Relic](https://newrelic.com/) is also working on its own extension (they even contribute to HHVM and most of the stuff making the new version possible was thanks to them), but I still wanted to have something new now and not wait for New Relic. I learn a lot doing such things and it maybe helps other people to do similar stuff for other profilers.

# The hotprofiler is slow (in general)

Just one thing in advance, if you want to get function level profiling with the so called hotprofiler, your requests are getting slow. In my tests, they take about twice the time as without the profiler. But that is also the case with the profilers built into HHVM, so this seems to ge a general problem. So don't turn it on by default, only when you need it. We don't really use it in production and for local performance tests, other tools are better suited (like xhprof). But if you only use it in certain part of the code, it may not slow you down a lot.

The good thing: If you don't want to use the hotprofiler, but just report the total time your request took, errors/warnings and maybe some additional data or "segments", you don't have to recompile (or use experimental branches) HHVM anymore. But can just download the hhvm and hhvm-dev packages and compile the extension.

One problem is also that New Relic only shows 2000 segments, which you hit very soon, when you record every function/method call in a complex framework like Symfony.

# But the rest of the extension isn't

We mainly just report the total time our requests take, to see a general trend. And to get reported about errors and where they happened. We may also add our own segments (with `newrelic_segment_generic_begin($name)`) for known slow parts in the near future, but don't do that yet. With only this, we didn't notice any slowdown of the requests when using the extension.

I also merged the ["Automatic Segements" feature by Jared Kipe](https://jaredkipe.com/blog/website-development/newrelic-hhvm-automatic-segments/), which gives you some more info about MySQL and externel http calls, without slowing your requests down much.

You could also use the hotprofiler just on some parts of the code, eg. in a command and only enable it there with `newrelic_profiling_enable($level)` to save some time.

# Caution using unpatched HHVM 3.5 and the hotprofiler

If you use standard HHVM 3.5 and my extension, then it will fallback to "userland level profiling", which make your request even more slow. But this is recorded as Custom Attribute in New Relic, so you see, if something is wrong. If you want to use HHVM 3.5 with the fix for the hotprofiler, you can use [my patched HHVM 3.5 version](https://github.com/chregu/hhvm/tree/HHVM-3.5), but then you have to recompile HHVM and this can take ages and has a lot of dependencies

# To sum up
If you got used to New Relic and want to use HHVM (or are using it already), give my extension a try until New Relic releases their own extension. They find hopefully a way to not slow down HHVM that much when using function level profiling. But if you don't need that immediately, my extension is a good way to get data into New Relic. It's very useful for us.

If you have question about the extension or feature requests or want to know more, just ask in the comment section or [open an issue](https://github.com/chregu/hhvm-newrelic-ext/issues). I'm always happy about feedback.


