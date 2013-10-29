With the recent announcement of Facebook that their HHVM is now more and [more compatible with most of the popular framework](http://www.hhvm.com/blog/1301/hhvm-2-2-0), I was intrigued to finally try it out. We’re currently building a Symfony2 based application, which has pretty high performance requirements (but we can mostly achieve them with varnish), so I went and did some performance tests on that real-life app.

The application basically gets data from an ElasticSearch server and then transforms them to JSON. Nothing too fancy from the output perspective, but we use many features of Symfony2 and what was clear from the beginning is that the more data it gets from ElasticSearch the slower the requests get. Meaning that we loose a lot of time in the serialization part (we use [JMSSerializer](http://jmsyst.com/libs/serializer) for that).

In short, the numbers were amazing. I also compared PHP 5.3 with APC against 5.5 with opcache, that alone gave some pretty decent improvements.

The setup I did the tests on was a QuadCore Intel i7-4770 CPU @ 3.40GHz server over at Hetzner with SSD disks and more than enough RAM. The actual application ran in a VirtualBox container with 4 CPUs and 2 GB of RAM. I used Apache Bench for the tests with the [HHVM vagrant VM](https://github.com/javer/hhvm-vagrant-vm) running Ubuntu 12.04, but I installed the pre-built HHVM from their repo in the end.

I made 3 different requests with different amount of data the script had to get from ElasticSearch. One was approx. 7kb in response, the other 80kb and the last 220kb.

As you can see below, in general the longer the PHP request ran, the more we gained from HHVM, up to and sometimes more than 300% against PHP 5.3, and about 200% against PHP 5.5. Only switching from PHP 5.3 to PHP 5.5 can save you up to twice the time, as well. So it is very much worth upgrading to 5.5. I find both numbers pretty amazing.

If you really need every millisecond performance, considering HHVM is worth some investigation. These numbers are very promising. And I don't had to change one line of code in our application. As we don't use it yet in production, I can't say anything about stability. Obviously tools like New Relic are, at least for now, also out of the question with HHVM.

## Requests per second, small response

The higher the better

Concurrency | PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---: | ---:
1|52|84|81|157 %|96 %|163 %
2|120|163|207|173 %|127 %|136 %
3|154|219|285|185 %|130 %|142 %
4|172|247|317|185 %|129 %|144 %
5|186|256|342|184 %|134 %|138 %
6|203|259|340|168 %|132 %|128 %
10||253|443||175 %|
20||247|441||179 %|
50||258|380||147 %|

<img src="/dynimages/370/files/images/blog/hhvm/small-req.png" width="50%"/>


## Requests per second, middle response

The higher the better

Concurrency | PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---:  | ---:
1|10|14|26|269 %|184 %|146 %
2|19|29|56|295 %|193 %|153 %
3|28|42|79|286 %|186 %|154 %
4|34|51|92|271 %|179 %|151 %
5|35|53|98|279 %|186 %|150 %
6|35|50|101|285 %|203 %|140 %
10||56|113||202 %|
20||53|105||197 %|
50||56|108||193 %|

![image](/dynimages/370/files/images/blog/hhvm/middle-req.png)


## Requests per second, large response

The higher the better

Concurrency|PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---: |  ---:
1|4|6|12|283 %|179 %|158 %
2|8|12|25|333 %|207 %|161 %
3|11|18|37|324 %|208 %|155 %
4|15|22|46|307 %|207 %|148 %
5|15|23|48|318 %|208 %|153 %
6|15|24|51|339 %|215 %|158 %
10||24|51||218 %|
20||24|49||208 %|
50||24|50||212 %|

![image](/dynimages/370/files/images/blog/hhvm/large-req.png)



## Median response time in ms, short response

The lower the better

Concurrency | PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---:  | ---:
1|15|11|9|60 %|82 %|73 %
2|16|12|10|59 %|79 %|75 %
3|19|13|11|55 %|81 %|68 %
4|23|16|13|54 %|78 %|70 %
5|26|18|14|54 %|78 %|69 %
6|29|22|17|57 %|75 %|76 %
10||38|21||55 %|
20||78|43||55 %|
50||191|105||55 %|

![image](/dynimages/370/files/images/blog/hhvm/small-median.png)



## Median response time in ms, middle response

The lower the better

Concurrency | PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---:  | ---:
1|98|61|32|33 %|53 %|62 %
2|103|66|34|33 %|52 %|64 %
3|107|66|36|34 %|55 %|62 %
4|112|71|40|36 %|56 %|63 %
5|138|89|49|36 %|55 %|64 %
6|171|112|57|33 %|51 %|65 %
10||174|83||48 %|
20||366|185||51 %|
50||872|451||52 %|

![image](/dynimages/370/files/images/blog/hhvm/middle-median.png)


## Median response time in ms, large response

The lower the better

Concurrency | PHP 5.3|PHP 5.5|HHVM|HHVM vs 5.3|HHVM vs 5.5|5.5 vs 5.3
---: | ---: | ---: | ---: | ---: | ---:  | ---:
1|236|149|71|30 %|48 %| 63 %
2|249|158|75|30 %|47 %|63 %
3|253|161|77|30 %|48 %|64 %
4|261|168|81|31 %|48 %|64 %
5|327|211|98|30 %|46 %|65 %
6|394|246|115|29 %|47 %|62 %
10| |419|189| |45 %| |
20| |843|394| |47 %| |
50| |2069|968| |47 %| |

![image](/dynimages/370/files/images/blog/hhvm/large-median.png)

I will have a look at memory consumption in a follow up post.


