We did a small two person hackday to get to know Cassandra a bit. Lukas already learned a bit about Cassandra at [Berlin Buzzwords](http://blog.liip.ch/archive/2014/06/03/berlin-buzzwords-2014.html) but it was time to get some hands on experience to better understand how Cassandra would work out in a real client project. Our agenda for the day was however essentially: get it to run, see how to get it connected with PHP and then import some data and do some queries.

### Brief summary

But first a very brief summary of what Cassandra is about:

* Decentralization based on [Dynamo paper](http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
* Datastorage based on [Googles Bigtable paper](http://static.googleusercontent.com/media/research.google.com/en//archive/bigtable-osdi06.pdf)
* Some good presentations: 
 - http://www.slideshare.net/davegardnerisme/cassandras-sweet-spot-an-introduction-to-apache-cassandra 
 - https://skillsmatter.com/skillscasts/2489-cassandra

### Getting started

To get up and running we tried different approaches. One was just to [download one of the VMs](http://planetcassandra.org/try-cassandra/) provided by DataStax (the main company driving Cassandra). That sort of works but the VM operating system is not really nicely configured. But it was at least enough to run a first few [queries we found in the Cassandra wiki](http://wiki.apache.org/cassandra/GettingStarted).

Now we wanted to install the [PDO driver for Cassandra](https://github.com/Orange-OpenSource/YACassandraPDO). So we setup a new VM and [installed Cassandra](http://www.datastax.com/documentation/cassandra/2.0/cassandra/install/installDeb_t.html?). Compiling the PDO driver proofed to be a bit more complicated. We messed around for a while until [Henrik](https://github.com/henrikbjorn) saved us by uploading debian packages for us compatible for Ubuntu 14.04 64bit:
> wget https://www.dropbox.com/sh/srasqe93i9q3ymy/AACfILNpSlG38wRRU8iCNbWPa/thrift-0.9.1_amd64.deb
> sudo dpkg -i thrift-0.9.1_amd64.deb
> 
> wget https://www.dropbox.com/sh/srasqe93i9q3ymy/AAD78Ov9lUT8iKZ-cQjQThW7a/pdo_cassandra-0.5.1_amd64.deb
> sudo dpkg -i pdo_cassandra-0.5.1_amd64.deb
> 
> sudo php5enmod pdo_cassandra

### Playing around

We then started playing a bit. We looked for some interesting data sources and found [this blog post](http://rs.io/2014/05/29/list-of-data-sets.html) listing a bunch of options for us, we ended up going with a [dump of reddit posts](https://github.com/umbrae/reddit-top-2.5-million). Not a particularly challenging data set for Cassandra, but enough to get a feel for things. Cassandra provides a PostgreSQL insipired [variant of COPY to import CSVs](http://www.datastax.com/documentation/cql/3.1/cql/cql_reference/copy_r.html). Using the following table definition, we started writing some queries against the dataset.

```
use mykeyspace;

CREATE TABLE reddit_posts (
  id text,
  created_utc text,
  score int,
  domain text,
  title text,
  author text,
  ups int,
  downs int,
  num_comments int,
  permalink text,
  selftext text,
  link_flair_text text,
  over_18 boolean,
  thumbnail text,
  subreddit_id text,
  edited boolean,
  link_flair_css_class text,
  author_flair_css_class text,
  is_self boolean,
  name text,
  url text,
  distinguished text,
  PRIMARY KEY (id, num_comments)
);
```

We quickly realized that for [index supported queries](http://gettingstartedwithcassandra.blogspot.ch), one needs to define composite key indexes. With a bit of time we got it to work. 

```
<?php

$db = new PDO("cassandra:host=localhost;port=9160", null, null, array(PDO::CASSANDRA_ATTR_THRIFT_DEBUG => true));

$stmt = $db->prepare("SELECT * FROM mykeyspace.reddit_posts WHERE num_comments > 11 limit 1 allow filtering");
$stmt->execute();

print_r($stmt->fetchAll());
```

We then [played with a counter](http://www.datastax.com/documentation/cql/3.0/cql/cql_using/use_counter_t.html), but realized that counters cannot be in a composite index. Finally we briefly looked at their "transactions", which however require every query essentially [specify a condition for concurrency](http://www.datastax.com/documentation/cql/3.1/cql/cql_using/use_ltweight_transaction_t.html).

### Pros and cons

+ Write speed
+ High availability due to decentralisation, no master node 
+ Linearly scalable without downtime
+ Consistency highly controllable, use when needed
+ Ad-hoc queries possible via SQL-ish language (*)
- Index supported queries need to be planned in advance, may require denormalization to support different index supported queries in the same data-set
- Not all queries possible (very limited aggregation capabilities), so in practice other systems might need to be attached for searching
