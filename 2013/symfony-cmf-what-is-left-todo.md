Symfony CMF: what is left todo?
Just as Fabien did in his "[Symfony 2.2 Schedule Update](http://symfony.com/blog/symfony-2-2-schedule-update)" I
would first like to wish everyone a happy 2013. But as as Fabien I also want to get back to business now too.

Over the holidays several people in the community have been quite busy. Especially [Emmanuel](https://github.com/EmmanuelVella)
and [Daniel](https://github.com/dantleech) have been pushing things forward. As a result the
[SonataAdminBundle](https://github.com/sonata-project/SonataAdminBundle) integration has been improved considerably.
Daniel has made several contributions to [PHPCR ODM](https://github.com/doctrine/phpcr-odm). Henri also just published
an update on the work going on around [create.js](http://bergie.iki.fi/blog/createjs-in-2013/) that we use to
provide inline editing. At the same time there are now several new contributors each month bringing a stream of
code and documentation improvements. I especially want to mention [Tiago](https://github.com/tiagojsag) here, as he
did a lot of very important additions and corrections to the [documentation](http://symfony.com/doc/master/cmf/index.html).

At the same time Liipers have also worked on several topics. [David](https://github.com/dbu) is putting the final
touches on updating [PHPCR](https://github.com/phpcr/phpcr/pull/52) and [Jackalope](https://github.com/jackalope/jackalope/pull/136)
to match the recent changes done in the JCR 2.1 (ie. JSR-333) spec. Many of the changes there were a result of feedback
from the PHPCR team, so this update will bring some important improvements. Adrien has brought the integration
of [create.js](http://createjs.org) for content authoring to a [usable state](https://github.com/symfony-cmf/symfony-cmf-website/pull/9).
I have been busy with some smaller improvements to [Jackalope Doctrine DBAL](https://github.com/jackalope/jackalope-doctrine-dbal)
and some bigger changes to [PHPCR ODM](https://github.com/doctrine/phpcr-odm).

At the same time we are quite excited that both [ezPublish5](https://github.com/ezsystems/ezp-next/blob/master/composer.json#L28)
and [Drupal 8](http://drupal.org/node/1874500) will leverage the CMF Routing component to handle the needs for dynamic routing.

So things are progressing and for people willing to live a bit on the edge all components are quite ready
for production use even today. However we are still stuck in alpha state, which means we try not to break BC but we
still break BC frequently. The next step is going to beta at which point we will become more hesitant to break BC
and where we will always provide upgrade documentation and if possible also an update script. In my humble opinion
there is really no reason not to move all parts in the stack to beta more or less right now.

As our goal is to make a stable release around the time of the 2.2/2.3 core releases we however have to quickly
however also move past the beta label so that we can provide a reliable development platform. In order to get there
I think we need to do work on the following:

1) Jackalope (MUST)
- Merge the JSR-333 updates mentioned above.
- Fix the left-over issues from the initial work to provide [XML import/export](https://github.com/jackalope/jackalope/pull/105)
- Wrap up the [open PRs ](https://github.com/jackalope/jackalope-doctrine-dbal/pulls) in the Doctrine DBAL implementation

2) Create.js (MUST)
- Fix the [open issues](https://github.com/symfony-cmf/symfony-cmf-website/pull/9)
- Create an example for Block administration in the sandbox and standard edition
- Improve support for non visible metadata (like tags, dates, publish states etc.)

3) Documentation (MUST)
- Continue to make improvements to the documentation

4) KnpMenu (SHOULD)
- Ensure that version 2.0 is finished and supports lazy loading the menu data to [solve performance issues with larger menu structures](https://github.com/symfony-cmf/MenuBundle/issues/19)

5) Sonata (SHOULD)
- Add more standard blocks (f.e. slide show)
- Add support for SonataCacheBundle to allow for caching of block content
- Add support for SonataMediaBundle to allow for more flexible media asset management