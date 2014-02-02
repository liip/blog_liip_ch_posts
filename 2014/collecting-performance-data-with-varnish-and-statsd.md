One of our currently bigger projects is soon going online and one part of it is an API, which only delivers JSON and XML to a lot of possible clients. To speed things up, we use varnish in front of the application (a lot of requests are easily cacheable, since they only change once a day at most). We also use HHVM for some of the requests, for now just the ones which will have many misses (from livesearch requests for example). We don’t dare yet to use HHVM for all requests, we’d like to gather some experience first with it.

One important task for such a project is to monitor the performance of the application. We usually use [New Relic](https://newrelic.com/) for that, but there’s no New Relic for HHVM (yet), so we had to look into other solutions.

Since varnish is the entry point for all requests, be it PHP or HHVM or another backend, we searched for possibilities there.  We found 2 vmods, [libvmod-timers](https://github.com/jib/libvmod-timers) and [libvmod-statsd](https://github.com/jib/libvmod-statsd), which make that possible.

## Collecting response time and number of requests

It needs a [statsd](https://github.com/etsy/statsd/) and a backend for collecting the data. We decided to test [librato](https://metrics.librato.com) as the backend as we have heard good things about it (btw, if you have to configure many metrics in librato with the same attributes all over, use the API of librato, it's more powerful than their webfrontend and you can script it for later new metrics)

This setup allows us to collect detailed data about the performance of the different backends (and also monitor for example if there are too many 500 answers)

Compared to the examples in [the blog posts about the vmods](http://jiboumans.wordpress.com/2013/02/27/realtime-stats-from-varnish/), I made some small changes, to get the correct data for us.

To be sure our API is "always" up, we use the [fallback director of varnish](https://www.varnish-cache.org/docs/trunk/reference/vcl.html#the-fallback-director), especially for the HHVM part (we have much more experience in running a PHP server than a HHVM, that's why). It looks like the following

````
director hhvm_api_fallback fallback {
  { .backend = hhvm_api; }
  { .backend = php_api; } // will only be used if hhvm_api is unhealthy.
}
````
This means, that varnish will normally use the hhvm_api backend, but if that is down, it will use the php_api backend. They run on the same server, but listen on different ports (of course, the whole thing is loadbalanced in front of it to have less single point of failures)

As we only route some requests to the hhvm_api, we have the following in `vcl_recv`:

````
if (req.url ~ "^/things.json?.*search=" ) {
        set req.backend = hhvm_api_fallback;
}   
````

We can then later extend that URL matcher to include more requests.

The example of the author of the vmods uses `req.http.X-Request-Backend` for reporting which backend was used with statsd. Since with the fallback director defined above that is always _hhvm_api_fallback_, no matter which backend was used and we wanted to know when the fallback uses php_api instead of hhvm_api, we added the following to `vcl_fetch`:

````
set req.http.X-Stats-Backend = beresp.backend.name;
````

Now the backend that is actually used is reported later (at least for misses, for hits it's still hhvm_api_fallback, but that is ok, since it didn't use any backend at all, wthough I'm sure this could be changed as well somehow with some header trickery in the vcl).

The librato backend seems also to interpret some stats differently than for example the default graphite backend in statsd. Therefore we used the the following lines in `vcl_deliver` for sending the values to statsd:

````
### Key to use for statsd. Something like: config.hit.200
set req.http.X-Stats-Key =
req.http.X-Stats-Backend + "." +
req.http.X-Stats-HitMiss + "." +
req.http.X-Stats-Status;

### Increment the amount of requests here, and how long this 
### particular backend took.
statsd.incr(   req.http.X-Stats-Key + ".count");
statsd.timing( req.http.X-Stats-Key + ".time", timers.req_response_time() );
````

We basically added `.count` and `.time` to the key as librato gets confuesd if you use the same key for counters and timers.

With this data we can now plot graphs for each backend and each request type in librato. We immediatly see, if there are problems (for example too many non-200 responses or if one backend is slower than expected). We also can compare hits vs misses and collect the load of the servers with [collectd](http://collectd.org/).

## Logging slow requests

Having an overview about how fast your setup is, is one thing, but being able to know exactly which requests are slow is another one. For the PHP backend we have New Relic for that. It keeps track of slow requests with full call traces and eg. SQL queries and their performance. It's very useful, also for error reporting. For HHVM nothing similar exists yet (to our knowledge at least) and we didn't want to clutter the code with too many home-grown analysis calls. We decided that for now it's enough to just know which calls were slow, to be able to debug them, if it happens too often. And this is possible with varnish and the timers vmod. All we added for this in `vcl_deliver` was

````
std.log("Backend:" + req.http.X-Stats-Backend);
## Tag SlowQueries in the Log
if (timers.req_response_time() > 500) {
    std.log("SlowQuery:" + timers.req_response_time());
}
````
This always logs the used backend to the varnish logs. And if the response time was slower than 500ms, we also log it as SlowQuery to the logs.

Then we can use [varnishncsa](https://www.varnish-cache.org/docs/3.0/reference/varnishncsa.html) to get readable logs and write them to a file. We use the following command for that:

````
varnishncsa -F '%t "%r" %s %b "%{Referer}i" "%{User-agent}i" %{Varnish:hitmiss}x %{VCL_Log:Backend}x %D ' -m "VCL_Log:SlowQuery" -m "VCL_Log:Backend:hhvm_api$" -D -a -w logs/hhvm-api-slow.log
````

This is the usual logfile format of web servers with some info added at the end. If it was a hit or a miss, which backend was used (logged via the std.log command in the vcl) and how long it took (the %D parameter). Then we can filter to just log requests which have the SlowQuery tag and are for the hhvm_api backend (the others are recorded by New Relic, but we could of course do them here as well).

We then collect those with [logstash](http://logstash.net/) and send them to our [graylog2](http://graylog2.org/) server. From there we can do for example alerts or just analyse the requests and try to reproduce the slowness and make it faster.

It's of course not as powerful as the full toolset of New Relic (or similar), but at least we know now, which requests are slow. And we can also use that for other backends, should the need arise.

If you have any input what could be improved or if you know about other tools to collect stats for HHVM (or other currently exotic systems), let us know. We're eager to hear about them. (Btw, Javascript, browser-based solutions are out of question here, since the API backend just sends JSON or XML, no possibility to inject JavaScript).
