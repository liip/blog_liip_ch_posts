## Why edX?
Recently we decided to explore the edX platform. It’s an open source Learning Management System (LMS) and authoring tool. EdX is one of the most popular MOOC platforms alongside Coursera and Udacity, but unlike the two others their code is open source and you can host your own MOOC platform. EdX has been created by MIT and Harvard and is listing other well known universities as contributors. Also, in September 2013, Google, who was previously experimenting its own LMS (Google Course Builder), decided to join the open edX platform as a contributor too. The platform is implemented mostly in Python and they created their own component architecture called XBlock (https://xblock.readthedocs.org/en/latest/). On paper edX sounds great, but how does it compare to Moodle or Totara? Does it fulfill the criteria our clients are targeting?

## First steps
Installing a local instance is fairly easy since they provide a vagrant box and the documentation is good. But on the other hand, you will need to run three different servers with three different port forwardings. One for the LMS, one for the CMS (authoring tool) and the last one for the forums (this one is written in Ruby). Moodle or Totara use the same tool for standard users and administrative users which can lead to a bad user experience. So having different tools can have advantages and disadvantages.

## Managing users
One key feature Moodle and Totara are offering is the user management. This is also one of the first requests we usually receive when dealing with offers. With Moodle and Totara there are lots of ways to authenticate a user and also to enroll them to a course out of the box. You can also easily manage roles and create groups (even dynamic groups with Totara).
In edX a user can authenticate if he creates first an account on the platform but edX is built on Django so any authentication method from this framework should be available on edX too (e.g. LDAP authentication).
It was not so easy to find how you can restrict the access to a course, this seems to be feasible depending on different settings (they were not explained in the user documentation, but understandable from the code documentation). You can create groups but it’s not clear yet if you can have groups platform-wide too.
Inside a course it’s easy to manage your staff team, define your teaching assistants or other admin users. You can even add beta testers so that some students can test your course before its official public release. You can also have discussion moderators for your courses.

## Multilanguage
Having the platform in several languages is possible, and courses in different languages too. But you cannot have the same course in different languages (which is the same for Moodle and Totara, you simply need to create a course for each language). A good and bad thing with the current state of internationalization is that it’s a work in progress. It’s bad because if you want a german edX platform, the translations are not finished yet. But it’s good because there are working on it and plan to finish it for the end of second quarter 2014 and they have a translation tool where anyone can contribute (https://www.transifex.com/organization/open-edx). By default, it would use the browser language, but if you’re logged in you can also have a user language (at least it’s configured that way on www.edx.org).
Hosting
Configuring manually your server to host an edX platform is really difficult, there are a lot of requirements and dependencies to install. For Debian some of them don’t even have a package. But they provide a way to install it using Ansible requirements which seems quite easy to do. They also provide some script to execute update which is nice.

## Interaction between users
Like on Moodle and Totara, you can have discussion forums for a course. There is a nice WYSIWYG editor with the possibility to preview your post first.
A nice feature of edX is the ability to comment any exercise or theory page and allowing others to upvote/report those. It’s a pity though that the comments aren’t ordered by upvotes by default.
Students can find study buddies using Meetup communities. EdX can also integrate Google Hangouts.

## Reporting
EdX comes with basic reporting tools. One can generate for example the list of enrolled students and the grade reports. Compared with Totara’s custom reports feature, edX has still plenty of room for improvement.

## User Interface
Generally the interface looks very nice. The UX-UI concept is very strong and has a clear consistent interaction level. You can also implement custom themes as Stanford did for its own instance. Without going into details, it seems edX has a nice base theme and state-of-the-art Interaction Patterns but they don’t have a responsive theme - only the home page is responsive.

## Learning experience
The learning experience is appealing and has the same feel as new online platforms (team-treehouse, codeacadamy, …). Courses offer a lot of options out of the box: video integration (via Youtube), multiple question forms (radio buttons, checkboxes, dropdowns, text, maths inputs, ...), Google Hangouts, discussions, text, announcements, integrating an Iframe, full-screen images, zoom images, imaged-mapped input, drag and drop answers, electrical schemas, LTI integration, … but there are bad things too.  For example, the grading system is not suitable for the usual Swiss grading system (grades from 1 to 6), it’s apparently made for the American grading system (from F to A).

## Conclusion
EdX does less things than Moodle but does them nicely. It’s also better suited for open education which can be achieved in Moodle and Totara too but the user and learning experience seems to be better on edX. Sever configuration, user management, access restriction, internationalization and reporting, all those things are easier to set and ready to use out of the box with Moodle (reporting especially on Totara) and cannot be neglected. Some might say that Moodle does too many things but from a business point of view it’s easier to adapt client’s requests that way. Also the community around Moodle is bigger which is a great thing from a developer point of view. But to end on a positive note, edX provides a roadmap on what they are working or will work on, thus we will keep an eye on this innovative platform and check if their improvements could fit future client requests.

