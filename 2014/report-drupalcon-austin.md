![DrupalCon group photo](https://farm4.staticflickr.com/3904/14336326761_189ecdd39e_z_d.jpg "DrupalCon group photo (c) Michael Schmid, x-foto.ch")

DrupalCon Austin was last week, and I was the Liip delegation. Being my
first American DrupalCon (6th overall), it was very interesting to
experience the biggest DrupalCon yet.

The Drupal community was as vibrant and welcoming as ever, and the
amount of energy being expended by thousands of volunteers to get Drupal
8 done is astounding.

## Whither Drupal 8?

Having been through quite a few Drupal release cycles by now (4.6.x, 4.7.x,
5.x, 6.x, 7.x, and now 8.x), I know better than to ask (or attempt to
answer) the “When is it coming out?” question, because the answer is the
same as always: “When it’s done”.

That being said, we are getting close. The most important thing at the
moment is the [beta blockers list][bbl], which currently consists of 11
issues. Once all beta blockers are resolved, Drupal 8.0-beta1 will be
released, and if this release will be anything like the previous, we
will see a lot of new momentum when that happens.

Once the beta lands, contrib module authors will have a stable API to
develop against, which means that they can start the mad rush to get
their modules upgraded to Drupal 8.

It also means that using Drupal 8 for new projects becomes a reasonable
option, and given how much more functionality comes out of the box in
Drupal 8, I think we will see a lot of people doing this, since unlike
previously (especially with Drupal 7), they will not have to wait for
Views and other indispensible modules to be upgraded.

So, at least to a developer like me, it doesn't matter much when the
official, final 8.0 stable release will come. It matters a lot more when
I can start using it (I have a few Drupal 8 hobby projects already, but
it's not being used at Liip yet), and that is getting very close.

## How was the conference

Seasoned DrupalCon visitors will know that some the most important work
and interaction happens outside of the official prorgam. Hallway chats,
code sprints and social events are where long-time community members
spend most of their time.

And this year was no different. Every night had one or more different
parties (Viking Fest, woo), and hundreds of people were hanging out in
the coder's lounge, before, during, and after the main event. Meeting
new people, and collaborating is really the name of the game, and in
that regard, this year was a great sucess for me.

I spent quite a bit of time working on [Twig][]-related issues (one of
the new exciting changes in Drupal 8 is the replacement of the old and
tired PHPTemplate engine with Twig), and a general clean up of the
markup output by Drupal.

The [#DrupalTwig][] effort is just one of the many new things in Drupal
8, but it was rewarding to see several long-standing issues get resolved
during the week, and it looks like we will have a great theme system in
Drupal 8. Thanks to Scott, Joël, Morten and the others for spearheading
this effort and helping me and others get up to speed so we could help
move the project along.

Looking forward to next time :)

[bbl]: https://drupal.org/project/issues/search/drupal?project_issue_followers=&status[]=1&status[]=13&status[]=8&status[]=14&priorities[]=400&categories[]=1&categories[]=2&version[]=8.x&issue_tags_op=%3D&issue_tags=beta+blocker "Drupal 8 beta blockers"
[Twig]: http://twig.sensiolabs.org/ "The flexible, fast, and secure template engine for PHP"
[#DrupalTwig]: http://drupaltwig.org/
