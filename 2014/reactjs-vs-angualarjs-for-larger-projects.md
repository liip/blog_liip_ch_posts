
On the September 2014 hack day at liip explored how AngularJS compares to ReactJS with Flux for large web applications. 

## Summary; tldr
...

## Getting started
We started the hack day by collecting aspects we thought are relevant for larger web applications. 
Since we were four people, we decided to have each liiper look at the framework they had the least prior knowledge. The liipers Matteo De Micheli and Mano Speck dived into ReactJS + Flux while Bastian Feder and myself took a look at AngularJS. We felt this maximised fun and learning of new technologies. Then each team got their hands dirty until we discussed our findings at the end of the day. 

For AngularJS we first tried to scaffold an example app with [yeoman](http://yeoman.io). After some setup issues we got an app generated, but we realised that we wanted to see the use of AngularJS in existing apps. We found an example [angular-app](https://github.com/angular-app/angular-app) based on [ngbp](https://github.com/ngbp/ngbp) which argues to _contain a best-practice directory structure to ensure code reusability and maximum scalability_. To understand AngularJS, we scanned and modified the code while reading the great [AngularJS documentation](https://docs.angularjs.org/).

For ReactJS and Flux we looked at the [ReactJS](http://facebook.github.io/react/docs/why-react.html) and [Flux documentation](http://facebook.github.io/react/docs/flux-overview.html). With [yahoo/flux-examples](https://github.com/yahoo/flux-examples) we found nice examples how the architecture works in practice. 

## Comparison 
In the following list we compare the two libraries in aspects we think are important for larger web applications. We don't claim this list to be complete but hopefully you'll find it useful.

#### Community support
AngularJS: http://ngmodules.org/ currently has 870 modules

ReactJS: http://react-components.com/  the community isn’t too big but there are quite some packages out there and with facebook there’s a huge company already using it


#### Complexity
AngularJS: steep learning curve because a developer has to learn many language constructs. Declarative style using data binding reduces complexity and side effects compared to css selector matching in jQuery. Independence of css selectors for data binding is useful simultaneous development of ui and business logic.

ReactJS: HTML in their awesome .jsx files seemed weird at first, but it has good reasons. There’s (nearly) no state since the HTML is tightly linked with their Javascript Components. Good for data-binding, and “view” specific stuff, but not more (Models, Routing...)

#### Interoperability (use of existing frameworks)
AngularJS: because services can be written the way the developer wants, it is no problem to write encapsulated code. 

ReactJS: Predefined methods used by react.js (render) are not clearly visible on first sight

#### Modularisation 

AngularJS: Modularisation is a core concept of angular and automatic dependency resolution supports code reuse. we like the dependency injection.highly extensible with directives, own services, …  http://clintberry.com/2013/modular-angularjs-application-design/

ReactJS: Components are modular and re-usable, seems similiar to Backbone Views


#### Performance
For performance we focused on two main things: download size and dynamic loading of modules. Especially for mobile devices, download size can make a big difference.

AngularJS: Minified and compressed the size of the angular core is under 36KB which is quite reasonable. depending on other external dependencies the download size could increase but that is not specific to angular. If the functionality of jqLite is enough. Modules have to be loaded or can be dynamically loaded using for example [ocombe/ocLazyLoad](https://github.com/ocombe/ocLazyLoad).

ReactJS: The rendering performance is quite fast, thanks to their Javascript React.DOM


#### Stability
AngularJS: 

ReactJS:Seems quite stable but isn’t (Adrian used it and there are quite some changes)

#### Testability
AngularJS: karma for continuous testing and built-in testing support in angular.

ReactJS:testing is possible with facebook’s inhouse framework http://facebook.github.io/jest/

#### Tooling 

AngularJS: Plugins for major ides (webstorm, eclipse, netbeans), yeoman scaffolding, examples how to bundle with grunt/gulp. Server rending with https://prerender.io/ 

ReactJS: no PHPStorm Plugin, there’s one for Atom and Sublime Text 2













