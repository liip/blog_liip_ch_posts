PostgreSQL is an excellent open source RDBMS. It has established itself as one of the most reliable database system available in the open source world. Over the years it even grew new features that very few competing systems offer: GIS functionalities, Foreign data wrapper, NoSql features with the recent integration of JSON datatype and V8 engine and Full Text Search (FTS) capabilities. In this blog post we will investigate on the FTS features it offers.


Full Text Search has been a hot topics for the last 10 years. It grew out of the need for wider search capabilities that were not provided by regular SQL database. Therefore systems like Lucene, Solr and ElasticSeach came to life and filled that need. The main requirements for a FTS system could be summarized as following:
  - being able to search on many fields at the same time (blog post title, body, tags)
  - being able to sort matching results according to their relevance
  - relevance can be driven by assigning weight on the matched field (finding the searched term in the title is more important than in the body for example)
  - relevance is also calculated based on other criterion (like how many time the terms have been found and how close to each other they were found in the given text).

## The problem of having two data stores.

Usually web applications are storing their data into a RDBMS, most of the time this would be MySQL or PostgreSQL. Recently this has slightly changed with the advent of the NoSQL movement with proponents like MongoDB but this is not relevant for our discussion. So most of the time your application data is stored in an SQL database. When comes the need to add FTS features to your system to most common path taken is to pull in your system Solr, ElastiSearch, Sphinx. And because you date reside in an SQL datastore, which is your main datastore, you will have to put in place some synchronization mechanism between your two data storages (RDBMS and FTS). There exist a couple of approach to do that.

### Updating both system whenever comes a new modification.

Every time a piece of information is changed in your SQL database you will propagate the change to your FTS system. This has the advantages to keep your system always in sync but make the processing of each request a bit more heavier. Of course this scheme comes with its variants to make the deal better. One could go async and use a message queue system in which it will write a message informing that this data modification has been performed and then another part will take over from here and update the FTS system.

### Batching the changes by updating at regular interval.
You keep the two systems in sync by running some synchronization script at regular interval. Depending on how long and heavy is this synchronization process on your system resources you will be able to determine how often this process should occur. Here the disadvantage is obvious, you will have a period of time between two synchronization points when your data in actually out of sync. Meaning that new content will not be searchable immediately and that old content will out-live after they have been changed. This is a tradeoff and depending on your requirements it might be completely acceptable. Here also there are ways to make this process more efficient. One could keep track in the RDBMS of the date of modification of the data. The synchronizing script could also keep somewhere track of when it last ran the update. Upon running again it can query only for the data that have changed after that saved date and therefore work only on the data that has changed in this time frame.

Either way you cut it, having to add a second storage perform FTS searches is not without effort. 
  1. you are pulling in a new dependency in your system and will have to deal with the configuration and maintenance of the FTS search engine (add scripts to make sure they are started at boot time, and restarted upon crashes, backup the data files created by the FTS).
  2. as we have seen, you will have to take care of the synchronization of data between the RDBMS and your FTS system. Above we blindly assumed that the synchronization was unidirectional (from the RDBMS to the FTS). If you have a system where you have to face a two-ways synchronization then you are in for much more work and complexity to make the synchronization happen. But even without that requirement synchronizing the two data stores is a time consuming task for many projects.

## Using PostgreSQL to cover your FTS needs

Lets be honest from the beginning, the FTS features found in Postgresql are not as powerful as those found in dedicated packages. In some cases you will need those power features but in many other you simply won't use them. The features provided by PosgreSQL are just good enough for what you want to do and because they are readily available in your RDBMS and you don't have to struggle with installing, configuring and managing another piece of software.

Here are some examples of features that you might need (or not) and help you in deciding which solution to choose from:
   - faceted search, as far as I know this feature doesn't exist within PostgreSQL, so if you really need it then you have no other choise than using Solr or ElasticSearch. This is maybe the simplest point on which you could base your choice.
   - sharding and replication, PostgreSQL does provide ways to shard and replicate your database but I think it is slightly more involved to do so than it would be with Solr. Anyway this kind of features are only needed when you have a lot of data. And by today's standard when we say a lot of data that could mean terabytes. So it is safe to say that it is not everyone use case to handle such an amount of data.
   - geo-spatial search, this is a rather uncommon feature for a FTS engine but Solr does provide you with some geo-spatial search capabilities. But here it cannot compete with what PostgreSQL has to offer with its PostGIS module (not bundled in PostgreSQL, you will have to install it). However if you have you have simple need in this domain Solr could be helpful here.
   
## Conclusion

As always, choosing a technology is not a straightforward decision. There are always tradeoff. But for what I have seen PostgreSQL FTE features can bring you already quite far and could avoid you having to deal with Solr (or other systems) in many cases.

We haven't talk about technical matter in this blog post, there are already quite good technical blog posts on the topic and I think it is useless to repeat here what has already been said. I have used these ones to guide me during the hackday. If you want to go into more technical details I can recommend you having a look at those: 

   - a good technical [blogpost](http://blog.lostpropertyhq.com/postgres-full-text-search-is-good-enough/) about postgres FTS 
   - [another one](http://dlo.me/archives/2014/09/01/postgresql-fts/) but with a focus on usage from Django application
   - [postgresql documentation](http://www.postgresql.org/docs/current/static/textsearch.html) about the full text search feature
  
I hope this might help you next time you have to add fulltext search capabilities to the system you are building.

