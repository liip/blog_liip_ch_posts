This is a guest post by [Tim Bezhashvyly](http://tim-bezhashvyly.github.io/).

Have you ever wonder why are there so many ways of adding JavaScript or CSS file into a Magento page? The task is quite ordinary and can be accomplished by even junior Magento developer. The thing is each of us has just one approach in his arsenal when in fact the variety of available methods is much wider.

Finding out the complete list is easy. All methods reside in Mage_Page_Block_Html_Head class. The first pair is addCss and addJs. The syntax is quite simple:

    <action method="addJs">
        <script>scriptaculous/scriptaculous.js</script>
    </action>

Some core modules use ``<file>`` instead of ``<script>``. There are some talks over the internet regarding which approach is correct. The fact is there is absolutely no reason what container to use. Magento will accept anything as long as the closing tag matches opening one and will send the contents as the first argument to the addJs method of Mage_Page_Block_Html_Head class.

You got it right, there is also a second argument. It allows you to send any additional parameters to be included into ``<script>`` or ``<link>`` tag:

    <action method="addCss">
        <name>print.css</name>
        <params>media="print"</params>
    </action>

The path of a script added by addJs method is relative to Magento root /js directory. For addCss method it is relative to skin directory of your theme.

In fact both those methods are just wrappers for universal addItem method which can add either CSS or JavaScript into your page.

The basic syntax is the following:

    <action method="addItem">
        <type>js</type>
        <name>lib/ds-sleight.js</name>
    </action>

The type can be js, js_css, skin_js, skin_css, link_rel or rss.

js will include a JavaScript file and will search it in /js directory of your Magento installation. This is similar to what addJs method does.

js_css will create a CSS link and will also search the file in /js directory. This is handy for including stylesheets of some JavaScript libraries.

skin_js will try to locate a referred JavaScript file in js directory of your theme.

skin_css will create a CSS link to relative to skin directory of your theme. This is similar to addCss method described above.

link_rel can be used to create a custom ``<link />`` element with custom attributes. Can be used for including CSS files located remotely.

rss will create an RSS link but you most likely will never use it.

In general addItem method accepts 3 more parameters. The params is the same as the one used for addJs and addCss. Then there’s if and condition.

If if is set the added link will be placed into  wrapper. That means you will be able to limit its inclusion to some specific browser versions. The syntax is the following:

    <action method="addItem">
    <type>skin_css</type>
    <name>below_ie7.css</name>
    <params />
    <if>lt IE 7</if>
    </action>

The blank ``<param />`` is important here because as we already found out earlier Magento is not looking at the name of container tag but at it’s position. More info about conditional comments can be found here.

The last parameter is condition. It is not documented and I have not found any references on internet about how it works. The syntax is following:

    <action method="addItem">
        <type>skin_css</type>
        <name>custom.css</name>
        <params />
        <if />
        <condition>this_custom_css_is_allowed</condition>
    </action>

Once Magento comes upon such a statement it checks if this_custom_css_is_allowed is set on the head block and if yes and it is not false the js/css item will be included.

This is quite interesting technique. As you surely know it is possible to control whether the js/css will be included into the page by sticking to the particular layout handles. The condition parameter gives you an ability to provide even more sophisticated configuration. So within your controller you can do something like this:

    $this->getLayout()->getBlock('head')->setThisCustomCssIsAllowed(true);

Or you can set this value through XML layout. However this will not make much sense as again you are coming back to layout handles:

    <reference name="head">
        <action method="setThisCustomCssIsAllowed">
            <flag>1</flag>
        </action>
    </reference>

There are some other valid methods available which are all just another shortcuts for addItem method:

addCssIe and addJsIe are working same as addCss and addJs but also sending IE value as the if parameter to addItem.

addLinkRel is just a shortcut for addItem with link link_rel type. The first argument is used as a file path and the second can be fed with all attributes like rel, media, etc which will be directly injected into the ``<link />`` tag.


It may look like you don’t need any of those shortcuts if you have mastered the usage of addItem method. It may look like all those shortcuts are kept in the code for backward compatibility and you only need to know them to pass Magento Frontend certifications. Not quite. addItem method was included into Magneto from the very beginning and is will be converted into the private method in Magneto 2 leaving us all those shortcuts plus something new.
