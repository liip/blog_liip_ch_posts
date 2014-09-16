
On the September 2014 hack day at liip we explored how AngularJS compares to ReactJS with Flux for large web applications. 

There have been [other](http://www.quora.com/Pete-Hunt/Posts/Facebooks-React-vs-AngularJS-A-Closer-Look) [comparisons](https://medium.com/make-your-own-apps/facebooks-new-react-javascript-library-tutorial-rewritten-in-angularjs-e71bcedc36b) before but none focused on larger web apps.

## Summary; tldr

We like both frameworks a lot and since the choice for one library always depends on the applications requirements we won't give a recommendation. Given its age, AngularJS is way more mature than ReactJS which results in more available libraries and widespread use. It is to be seen how ReactJS can use its traction to catch up with AngularJS.

A key difference between the philosophies of AngularJS and ReactJS is, that AngularJS relies on the browser DOM while ReactJS builds a virtual DOM which then can be converted to either a HTML string for server rending or to the browser DOM. This enables ['isomorphic javascript'](http://nerds.airbnb.com/isomorphic-javascript-future-web-apps/) (see also the [JS Zürich event](http://techup.ch/1731/jszurich-building-isomorphic-apps) hosted at liip Zürich) and tackles problems like first page load performance and search engine optimisation.

## Getting started
We started our hack day by collecting aspects we thought are relevant for larger web applications. 
Since we were four people, we decided to have each liiper look at the framework they had the least prior knowledge. The liipers Matteo De Micheli and Mano Speck dived into ReactJS + Flux while Bastian Feder and myself took a look at AngularJS. We felt this maximised fun and learning of new technologies. Then each team got their hands dirty until we discussed our findings at the end of the day. 

For AngularJS we first tried to scaffold an example app with [yeoman](http://yeoman.io). After some setup issues we got an app generated, but we realised that we wanted to see the use of AngularJS in existing apps. We found an example [angular-app](https://github.com/angular-app/angular-app) based on [ngbp](https://github.com/ngbp/ngbp) which argues to _contain a best-practice directory structure to ensure code reusability and maximum scalability_. To understand AngularJS, we scanned and modified the code while reading the great [AngularJS documentation](https://docs.angularjs.org/).

For ReactJS and Flux we looked at the [ReactJS](http://facebook.github.io/react/docs/why-react.html) and [Flux documentation](http://facebook.github.io/react/docs/flux-overview.html). With [yahoo/flux-examples](https://github.com/yahoo/flux-examples) we found an example how the architecture works in practice. 

## Comparison 
In the following list we compare the two libraries in aspects we think are important for larger javascript web applications. We don't claim this list to be complete but hopefully you'll find it useful.

### Community support
AngularJS: http://ngmodules.org/ currently shows an impressive number of 870 modules. Learning resources in the form of courses are available for example form [codeschool](https://www.codeschool.com/courses/shaping-up-with-angular-js), [egghead](https://egghead.io) and [udemy](https://www.udemy.com/angularjs/). [Lots of companies](https://builtwith.angularjs.org/) build their apps with AngularJS. Support channels can be found on the [AngularJS website](https://angularjs.org/) under 'Discuss'.

ReactJS: http://react-components.com/ is growing but with under 100 components not quite there yet. The community compared to AngularJS isn’t too big but with Facebook, Instagram, [Github](http://blog.atom.io/2014/07/02/moving-atom-to-react.html) and Yahoo ([migrating Yahoo! Mail from YUI to ReactJS](http://pablolmiranda.tumblr.com/post/97242478366/first-react-meetup-at-yahoo)) there are more companies using it and we see lots of traction. 
See the [support channels](http://facebook.github.io/react/support.html) when looking for help.

### Complexity
AngularJS: Because of many [language constructs](https://docs.angularjs.org/guide/concepts) a develper has to learn, we experienced a steep learning curve. Previous knowledge of the MVC pattern helps. The declarative style using data binding reduces complexity and side effects compared to css selector matching in jQuery. Independence of css selectors for data binding is useful for simultaneous development of UI and business logic. AngularJS brings lots of functionallity like [routing](https://docs.angularjs.org/api/ngRoute) and models either build-in or as a module.

ReactJS: When it comes to ReactJS itself, things are pretty clear. Mixing template and view logic in components seemed weird at first, but it has [good reasons](http://facebook.github.io/react/blog/2013/06/05/why-react.html) and [JSX](http://facebook.github.io/react/docs/jsx-in-depth.html) is more familar to traditional HTML. Components [reduce complexity](http://jlongster.com/Removing-User-Interface-Complexity,-or-Why-React-is-Awesome) by only caring about their own state and declarativly rendering it. State flows down the component hierarchy while events flow up. ReactJS is good for a "view" but an application also needs routing, models etc. 

Looking at Flux things get more complex. The [flux application architecture video](http://facebook.github.io/react/docs/flux-overview.html) and [fluxxors documentation](http://fluxxor.com/what-is-flux.html) give a good overview but at the end of the day we felt that we didn't fully grasphed the concept yet.

### Interoperability (use of existing libraries)
AngularJS: Because services can be written the way the developer wants, it is no problem to write encapsulated code. For example [using ReactJS in a AngularJS directive](http://www.williambrownstreet.net/blog/2014/04/faster-angularjs-rendering-angularjs-and-reactjs/) is not an issue. For Controller, Directive, Filter and Animation you have to adhere the corresponding interfaces.

ReactJS: Reusing existing libraries like [for example markdown](http://facebook.github.io/react/docs/tutorial.html) is possible as long as they don't rely on the DOM. Unfortunately a lot of libraries rely on the DOM. When writing a component it needs to implement the [Component API](http://facebook.github.io/react/docs/component-api.html).

### Modularisation 

AngularJS: Modularisation is a core concept of AngularJS and automatic dependency resolution supports code-reuse. We like the dependency injection. Applications build with AngularJS seem to be highly extensible with directives, own services (...) and [composable with modules](http://clintberry.com/2013/modular-angularjs-application-design/).

ReactJS: Components are modular and [reusable](http://facebook.github.io/react/docs/reusable-components.html) and [share functionality using mixins](http://stackoverflow.com/questions/21854938/using-mixins-vs-components-for-code-reuse-in-facebook-react). The use of [composition over inheritance](http://facebook.github.io/react/docs/multiple-components.html#children) allows for composing an application.

### Performance

AngularJS: Minified and compressed the size of the AngularJS core is under 36 KB which is quite reasonable. Depending on other external dependencies the download size could increase rapidly but that is not specific to AngularJS. If the functionality of jqLite is enough, a dependency on jQuery could be avoided. Modules have to be loaded or can be dynamically loaded using for example [ocombe/ocLazyLoad](https://github.com/ocombe/ocLazyLoad). Key-Value observation like in Ember, Knockout, Backbone and Meteor (...) has a performance impact which will hopefully decrease in the future with [`Object.observe()`](http://updates.html5rocks.com/2012/11/Respond-to-change-with-Object-observe) in [AngularJS 2.0](http://blog.angularjs.org/2014/03/angular-20.html).

ReactJS: About 26 KB (min+gzipped) for only the 'view' part of an application might [seem big](http://stackoverflow.com/questions/19807946/why-is-reacts-js-size-so-big-given-its-small-api) but given that a lot of Reacts functionality overlaps with jQuery and templating engines (which won't be used anyway) it's actually pretty small. The rendering performance is fast, thanks to their javascript virtual DOM. Because the virtual DOM is rebuilt every time something changes there is [no need for observation](http://stackoverflow.com/questions/21109361/why-is-reacts-concept-of-virtual-dom-said-to-be-more-performant-than-dirty-mode). Rerendering of the actual DOM can be avoided and in many cases like the [Khan Academy TeX editor](http://joelburget.com/backbone-to-react/) or [Githubs Atom Editor](http://blog.atom.io/2014/07/02/moving-atom-to-react.html) this greatly improves user experience.

### Stability
AngularJS: AngularJS is widespread. Given it's [upcoming 1.3 release](http://blog.angularjs.org/2013/12/angularjs-13-new-release-approaches.html) there are some [breaking changes](https://github.com/angular/angular.js/blob/master/CHANGELOG.md) and also the API is cleaned of deprecated functions which is very normal. We would consider AngularJS as stable.

ReactJS: The library is production ready but hasn't reached version 1.0 yet. [There is a lot planned](http://facebook.github.io/react/blog/2014/03/28/the-road-to-1.0.html) and changes to the API are to be expected.

### Testability
AngularJS: [Karma](http://karma-runner.github.io/0.12/index.html) for continuous testing is fast and the built-in testing support in AngularJS feels great. We really like their end-to-end scenario runner and how testing works thanks to dependency injection.

ReactJS: Testing is possible for example with facebook’s inhouse framework [Jest](http://facebook.github.io/jest/) based on Jasmin or any other testing framework. [Test utilities](http://facebook.github.io/react/docs/test-utils.html) help to make this a little easier. [Using Karma with ReactJS](http://myshareoftech.com/2013/12/unit-testing-react-dot-js-with-jasmine-and-karma.html) could be interesting.

### Tooling 

AngularJS: Plugins for major ides (webstorm, eclipse, netbeans) are available. Yeoman generators and examples how to bundle an app with grunt/gulp are numerous. In general there is good tooling support.

ReactJS: There are [tools](https://github.com/facebook/react/wiki/Complementary-Tools) and some editor support but IDE support is not there yet. Yeoman can be used to scaffold. 

Thanks for reading. For a summary see the top.
