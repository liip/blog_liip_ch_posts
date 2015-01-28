We use [AngularJS](https://angularjs.org) here at Liip, and as developers concerned with the quality of our work, we employ a multitude of tools and patterns to ensure that everything works as expected. In short, we test our software.

The goal of this post is to give you a short overview about some of our experiences with testing AngularJS applications.

##End-to-end testing

Modern web applications have to integrate a variety of external services, database systems and APIs. On top of that, they have to accomodate an ever-shifting landscape of devices and browsers. 

This is an area that is notoriously difficult or even outright impossible to test with traditional methods such as unit tests and simple mocks. A database can fail, an external service can return an invalid result and a new browser version might have introduced a simple bug that we didn't know about when we initially wrote our code.

This is where end-to-end testing comes into play. We want to test our application as a whole and make sure that it works as expected. We want to test the entire application, starting from the user interface down to the individual subsystems, such as the backend storage or external services.

It's important to note that end-to-end testing is no panacea, we employ it alongside other testing methods, but the high level and broad scope of end-to-end testing helps us tremendously when developing a complex web application.

##Enter Protractor
Google has released an end-to-end testing framework for AngularJS applications called  [Protractor](http://angular.github.io/protractor/) that integrates existing technologies such as [Selenium](http://www.seleniumhq.org), [Node.js](http://nodejs.org) and [Jasmine](http://jasmine.github.io/1.3/introduction.html) and makes writing tests a breeze. 

With Protractor we can write automated tests that run inside an actual browser, against an existing website. Thus, we can easily test whether a page works as expected. The added bonus of using Protractor is that it understands AngularJS and is optimized for it.

If you already know Selenium and Jasmine, getting started with Protractor should be pretty straight forward.

##Installation
Protractor requires Node.js, Selenium and a testing framework such as Jasmine to be installed on your computer. You can find more about the installation process [here](http://angular.github.io/protractor/#/).

##Writing a simple test
Protractor expects your tests to be written in so-called *spec files*. Spec is simply another word for test. These spec files are written using the syntax of your test framework, and the [Protractor API](http://angular.github.io/protractor/#/api). Out of the box, Protractor uses [Jasmine](http://jasmine.github.io/1.3/introduction.html) as its default test framework, but it also has tentative support for [Mocha](http://mochajs.org) and [Cucumber](http://cukes.info).

Let's assume we want to test whether a login page displays an error message if we do not fill in the password field.
In protractor, we'd create a spec file (`login_spec.js`) for it that might look like this:

    describe('login page', function() {
      it('should display an error if the password field is empty', function() {
      
        // Visit the login page
        browser.get('http://mysuperawesomepage.com/login');
        
        // Find the element that matches ng-model="userName" and type 'gandalf' into it.
        element(by.model('userName')).sendKeys('gandalf');
        
        // Find the submit button and click it
        element(by.id('btn-submit')).click();
        
        // Check whether our error message is displayed
        expect(element(by.css('.password-error')).isDisplayed()).toBe(true);
      });
    });

So, what have we done here?

The `describe` call is from Jasmine, and we use it to describe the page we want to test, in this case, our login page. The `it` call is also from Jasmine, and we use it to describe the scope of our test. In this case the login page should display an error message if the password field is empty.

`browser` is a global variable exposed by Protractor, and we use it to visit our (`get`) login page.

`element` and `by` are also globals created for us by Protractor. We can use them to find and interact with elements on the page.

`expect` is again from Jasmine (and extended by Protractor), we test whether our expected error message is displayed.

It is important to note that Protractor is entirely asynchronous, so all API methods return promises. Under the hood, Protractor uses Selenium's control flow (a queue of pending promises) to allow us to write tests in a pseudo-synchronous way. Protractor is smart enough to make it all work.
 
##Elements and Locators
As you can imagine, a large part of writing a test against a web site deals with finding and locating DOM elements on a page and executing actions such as clicking on them. As explained above, Protractor offers globally available constructs that help us here: `element`, and `by`.

A call to `element` requires a `Locator` and returns an `ElementFinder` - it is used to find the first (or only) element that is matched by the Locator. Similarly, there is also `element.all` if you want to find more than one element. 

A `Locator` can be created by using the functions available with `by`. There are methods that return `Locators` like `by.css`, `by.id` or `by.binding` that pretty much do what you'd expect. It is even possible to create your own Locators and attaching them to `by` during the start up of Protractor.

```
element(by.css('input.username'));
element.all(by.css('a.btn'));
```

An `ElementFinder` exposes multiple actions, we've already seen `sendKeys` and `click` above. We can use these actions to interact with an element, or to find out more about an element, for instance whether it is currently being displayed or not, or what its text value is. 

```
element(by.css('a.home-page')).getAttribute('target');
```

It is also possible to chain `element` calls, it is somewhat similar to jQuery in this regard. 

```
element(by.css('div.article')).element(by.tagName('h1'));
element(by.css('ul#main-menu')).all(by.tagName('li'));
```

Last but not least, `element.all` exposes some additional functionality, as in getting the first child element, 
or getting a child element by index, and these calls can also be chained.

```
element.all(by.css('p')).first().element(by.css('span'));
element.all(by.css('div.article')).get(2).element(by.css('a'));
```

 
##Organizing your code: Page objects
If we only rely on `element` calls to structure our tests, our life gets progressively worse as the application grows. We'd have to duplicate a lot of code for components that are used across different pages, or pages that are used across different tests. One change to an element's class name could force us to rewrite many of our tests.

We use a design pattern called *page object* to overcome this problem. It is described in more detail by [Martin Fowler](http://martinfowler.com/bliki/PageObject.html). The gist of it is very simple, we try to encapsulate and wrap most of our Protractor calls in them:

    var LoginPage = function() {
      this.username = element(by.model('username'));
      this.password = element(by.model('password'));
      this.loginButton = element(by.id('btn-login'));
      this.passwordRequiredError = element(by.css('error-password-required'));
        
      this.visit = function() {
        browser.get('http://mysuperawesomepage.com/login');
      }
        
      this.setUsername = function(username) {
        this.username.clear();
        this.username.sendKeys(username);
      }
        
      this.setPassword = function(password)
        this.password.clear();
        this.password.sendKeys(password);
      }
        
      this.login = function() {
        this.loginButton.click();
      }
    };
    module.exports = LoginPage;

We can now use the LoginPage page object in our tests. You'll proably notice that the test is now much more readable, which is a nice side effect of using page objects:

    var LoginPage = require('./login-page');

    describe('login page', function() {
      it('should display an error message if the password field is empty', function() {
        var page = new LoginPage();
        page.visit();
        page.setUsername('gandalf');
        page.login();
        expect(page.passwordRequiredError.isDisplayed()).toBe(true);
      });
    });
 
We extend this principle to shared page components, such as headers, footers and AngularJS directives and can then re-use these components:

    var HomePage = function() {
      this.header = new Header();
      this.sideBar = new SideBar();
      this.footer = new Footer();
      ...
    };
    module.exports = HomePage;

Another neat trick: We often unify common methods in a base `Page` class that other page objects can inherit from:

    var Page = function() {
      this.clearAndType = function(element, text) {
        element.clear();
        element.sendKeys(text);
      };
      
      //...
    };  
    module.exports = Page;

##Conclusion
Protractor allows us to test our AngularJS applications in a consistent and automated way. We're better able to make informed statements about the overall state and soundness of our AngularJS applications because of it. 