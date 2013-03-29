I have been on the conference tour for quite some time now and on top of that I travel about every 2nd weekend to some frisbee tournament around the globe. Yet I have never visited the UK. So far the most I have seen of the UK was transiting via direct bus from one London airport to another. So I was quite thrilled when my talk about the Symfony2 CMF was accepted for [PHPNE in Newcastle](http://conference.phpne.org.uk). I was also quite keen to learn more about the PHP community over there. At any rate I flew in on Monday evening and made my way to the hotel in a light drizzle which perfectly matched my image of UK weather. But even in the dark one could make out the historical feel to the architecture in the city center. I cut the sightseeing short and crashed into bed. Next day I made my way to the conference venue which was set at a movie theatre. There were countless busy bee's from the organization team and in general this conference was organized top notch. Quite an impressive achievement given that this was the very first PHPNE. The theatre also provided top notch projectors and of course comfy seats.

### Rowan kicks off the show

Rowan did an awesome intro keynote talking about [how to make better developers](http://conference.phpne.org.uk/schedule/#rowan), touching both and practical aspects in terms of tech setup and continuing on to useful tips about self education and human interaction.

### API Design by Alex

Next talk I listened to was about [API design](http://conference.phpne.org.uk/schedule/#alex) by Alex. It was a combination of general tips and lesson learned from introducing an open API at a university. I pondered raising my hand when the presenter flat out recommended to always go with JSON over XML for REST APIs. Personally JSON is my preferred format when the client is usually a browser as it integrates so naturally there. For server to server interaction I much prefer XML, since its simply a much better format when one needs to evolve data format as one can get away with many changes without requiring a new API version.

### Bastian on logging and metrics

After this talk I attended was delivered by Bastian about [logging and metrics](http://conference.phpne.org.uk/schedule/#bastian). This is a topic I find quite important but unfortunately I have found its hard to convince clients to invest into this as they tend to focus more and features end users can see. However to ensure a stable environment which is sort of the basis for ensuring that customers can enjoy your site, logging is key. Furthermore only with metrics it is possible to figure out what features customers actually use, where they might get stuck and what could be easily dropped. It was nice to see that the tools Bastian chose to introduce are exactly the tools I had on my radar already. He specifically mentioned graylog, logstash, statsd and graphite.

### Lunch time chat on knowledge exchange

Over lunch I ended up chatting with Rowan about internal knowledge exchange. Rowan works for Invica. Just like Liip they have multiple offices and deliver projects using various frameworks and applications. Interestingly they seem to have several teams that span across offices. At Liip we of course also sometimes collaborate between teams in multiple offices, however project teams for the most part always come together in a single office.

### Fabrice showing the path to Symfony2

After lunch the sessions commenced again. I had seen Fabrice's talk about [migrating legacy applications to Symfony2](http://conference.phpne.org.uk/schedule/#fabrice) before. However it has evolved quite a bit since then. I will definitely apply many of his tips next time I have the task to migrate a legacy application and look forward to Theodo release some of their solutions as open source.

### Intro to the CMF

Right after Fabrice's talk I delivered [my CMF presentation](http://conference.phpne.org.uk/schedule/#lukas) to a noticeably smaller crowd. I hear the talk that was running in parallel about dealing with failures was very good. At any rate I must admit I have seen this pattern before at other conferences. I tend to have a fair number of people asking questions in my talk and afterwards even more. So I figure that the market for people willing to explore a new content management approach is simply a bit smaller than other topics, yet for those who do have issues with the current solutions its quite an important topic. At the start of my talk I asked what frameworks people are using and it validated a feeling I had gotten in the previous talks: There was no clear goto framework for conference attendees. The preferred frameworks were all over the map.

### Theij's on the cloud

The [closing keynote was held by Theijs](http://conference.phpne.org.uk/schedule/#thijs) who did a very energetic delivery to drive home the point that really the cloud is nothing more than .. the internet! Additionally he provided several good hints about what things to consider when using SaaS.

### Bar chatting

After the closing ceremony with lots of prizes we then moved on to dinner and finally everybody met back at a local bar where we had free drinks all night. However instead of simply getting drunk there were many lively discussions. I talked quite a lot with people about Symfony2 and the need for RAD development layers on top of the core. I also chatted a bit about e-commerce solutions in the PHP scene, specifically Magento, Oxid, FoxyCart as well as Vespolina and Sylius. At any rate, people were so busy talking that the free drinks money actually only ran out just before the bar was closing anyway.

### CMF workshop

For me however the conference wasn't quite over then as thanks to a late afternoon return flight I had offered to provide a [CMF introduction workshop](http://symfonycmf.eventbrite.com) on the next day. Anthony had managed to secure funds to give us a room large enough that I could give 10 people a 4 hour run down of all the details about the CMF with much more depth than my talk at the conference. Some people had already played with the CMF while others had only heard about it in a blog post. I led the attendees decide how to best spend the time. In the end all attendees installed the Symfony2 CMF Standard Edition, I answered several specific questions that some of the people had that already played with the CMF but aside from that I just ended up going through the PHPCR and Routing/Menu [slides](http://cmf.symfony.com), showing a bit more about the SonataAdminBundle and various other pieces. After the workshop I grabbed lunch together with Lars and Marc. Lars had been posting various questions about how to implement publishing workflows on the mailing list and is now diving into implementing some missing functionality into the Jackrabbit transport layer of Jackalope, which was [just merged](https://github.com/jackalope/jackalope-jackrabbit/pull/38).

### Wrapping up

Overall I must say it was a great conference and I was especially grateful that I could do the workshop. As we are approaching the first stable release its important to get as many people double checking our concepts as possible. Secretly I am also hoping that the attendees will take what they have learned and spread the word in their cities and organizations. Even with the internet frameworks and applications tend to be strongest in close geographical proximity to the core developers. So having an opportunity to make the Symfony2 CMF known outside of continental Europe will hopefully help grow the user base.