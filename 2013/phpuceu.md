So this weekend I visited my hometown Berlin for the [PHPUCEU](http://www.phpuceu.org). Actually this was in very close proximity to where I grew up, which was nice as I was staying at my parents place. Usually staying there requires a one hour ride to the hipster areas of Berlin to attend a conference. In this case it was just a 2 stop U-Bahn ride. That being said, this wasn't a "normal" conference. This was an [unconference](http://en.wikipedia.org/wiki/Unconference). As such attendees proposed talks they could give, but also talks that they would want to hear. Every morning every attendee would then have two votes for talks. The top voted talks would then be distributed across the 4 slots in the 3 available tracks. As such several of the sessions ended up being quite ad hoc with multiple people chipping in with what they new about the topic. What is also special about this event is that the sessions are just as long as the breaks to facilitate idea exchange about the session topics, but also about other topics. Overall I found this to be an absolutely thrilling experience.

## Day one

### PHPCR

I proposed a talk on PHPCR which was selected for the first session. I essentially gave the talk I had given multiple times before. But its still exciting to share the content repository vision for PHP. Looks like there might even be PHPCR support in the future for [Jimdo](http://dev.jimdo.com/2013/05/06/phpuceu/)?

### Clean puppet

I attended this session by Soenke, Jakon und Hans-Christian mainly because I am still quite a noob when it comes to Puppet and Chef. I must admit, the possibilities excite me but I still prefer others to do the development for me. Somehow I just don't like the DSL of Puppet nor the Rubyness of Chef. This session didn't change this but did highlight some important aspects, specifically that you would keep the concept of environments as for up the chain as possible.

### Behavior Driven Development

Kore and Gaylord gave this session. Gaylord was representing the more business level perspective while Kore the more technical one. The exciting part about BDD is that BDD can really bring those two sides closer together. However the most important take away for me from this session is that [Behat](http://behat.org) can now generate stubs for Context definitions, which will drastically reduce the effort for expanding the DSL for non developers. Gaylord mentioned that he thinks that using BDD seriously increases development time. Yet the benefits of additional testing and always uptodate documentation outweigh these costs. We have played with BDD at Liip several times, but its time we get more serious about it.

### Evening programm

I missed the 4th session of the day because I was chatting with people outside. After the last session ended we got together and each session was briefly summarized. We headed to a local beer garden which unfortunately was a bit overwhelmed with us and so the service was quite non existant. However we still stayed several hours. Talking about technology and organization challenges.

## Day Two

### Agile documentation

This was the [first session](https://speakerdeck.com/s0enke/agile-documentation) of the day for me. Soenke was making some important points about how documentation does not only provide benefit but also cost. And this cost is best managed by single sourcing documentation (ie. not duplicating documentation) and by making documentation executable (via BDD).

### Hood.ie

This session was slightly sabotaged by github. Kore and Hans-Christian wanted to explore [Hood.ie](http://hood.ie) life on stage but couldn't get the necessary code in time. So instead Kore spoke a bit about the core idea behind [CouchApps](http://couchapp.org). I was also able to add some tidbits about what Hood.ie adds as I recently got a personal demo by [Gregor](https://twitter.com/gr2m), one of the core developers. We then went through some of the code examples on the site. In general I believe that the coolest thing in CouchDB is the replication protocol, which is why I never understood why Couchbase decided to focus on elastic scaling instead. Thanks to async multi master replication with local storage one can build a whole new class of mobile applications. There is a very high chance that Hood.ie will become a big part of the Liip technology stack.

### Symfony2 Rest API

I held this session by basically doing a walk through of a client application we are currently building as well as the Liip Symfony2 techtalk edition. Overall I was showing off how to integrate various Bundles: [FOSRestBundle](https://github.com/friendsOfSymfony/FOSRestBundle), [JMSSerializerBundle](http://github.com/schmittjoh/JMSSerializerBundle), [NelmioApiDocBundle](https://github.com/nelmio/NelmioApiDocBundle), [FSCHateoasBundle](https://github.com/TheFootballSocialClub/FSCHateoasBundle) (requires this [PR](https://github.com/FriendsOfSymfony/FOSRestBundle/pull/327)) and [HautelookTemplatedUriBundle](https://github.com/hautelook/TemplatedUriBundle). I also highlighted several limitations which let me to post [this](http://pooteeweet.org/blog/2221) on my blog as a follow up.

### NoSQL is not the answer

I had given [this](http://www.slideshare.net/lsmith77/no-sql-isnottheanswer) as a lightning talk at GotoCon in Zurich and figured it would be a useful topic for this event as well. I even managed to get Kris to come out of the sun for this session! He added some more insights. The tl;dr; of this session is: Stop using the term NoSQL as well as that there is lots of room for innovation on top of relation theory which the vendors are finally addressing thanks to all the hype around MongoDB, CouchDB and friends.

## Take away

The big take away is that unconferences are awesome and from what I hear our yearly Liip internal techday will heavily draw from this concept. The second take away is that Symfony2 is the dominante framework in continental Europe. Third BDD deserves more attention at Liip.