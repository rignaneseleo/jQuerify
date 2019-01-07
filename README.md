jQuerify: A bookmarklet to insert jQuery into a tab.
-----------------------------------------

This bookmarklet is inspired by the many bookmarklets that label themselves "jQuerify" that can be found on the web via google on sites from Stack Overflow to reddit to all sorts of forums. (It is thus a member of the *jQuerify equivalence class where members all inject jQuery and otherwise congruent modulo actual implementation*.)

It turns out, somewhat surprisingly, few of these actually work and that can be seen by the myriad comments along the lines of "this did not work for me!"

So let's try it myself. Let's write my own implementation. 
My goals were to

+ inject a current, known, version of jQuery into the tab
+ do so in a way that does not conflict with any existing installed jQuery in the tab
+ do so so as not to conflict with Chrome Dev Tool's use of "$"

And here's what I came up with:

<pre><code>
  javascript: (function() { 
    var jQueryPath = 'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js';
    var s=document.createElement('script');
    document.getElementsByTagName('head')[0].appendChild(s);
    s.onload = function(script) {
            if (typeof jQuery === 'undefined') {
                console.log(jQueryPath + ' loaded but jQuery still undefined');
            } else {
                j$ =jQuery.noConflict();
                console.log(jQueryPath + ' loaded and j$ = jQuery.noConflict() called');
            }
    };
    s.setAttribute('type','text/javascript');
    s.setAttribute('src', jQueryPath);
    })();
</code></pre>

Drag Me
-------

<p>Clicking <a href="javascript: (function() { var jQueryPath = 'https://code.jquery.com/jquery-2.2.0.js'; var s=document.createElement('script'); document.getElementsByTagName('head')[0].appendChild(s); s.onload = function(script) { if (typeof jQuery === 'undefined') { console.log(jQueryPath + ' loaded but jQuery still undefined'); } else { j$ =jQuery.noConflict(); console.log(jQueryPath + ' loaded and j$ = jQuery.noConflict() called'); } }; s.setAttribute('type','text/javascript'); s.setAttribute('src', jQueryPath); })();"
>*jQuerify*</a> - see its code below - will insert jQuery 2.2.0 into your tab.
The *jQuerify* link can be dragged to your bookmarks.</p>

Now, if this page is served from Github,the *jQuerify* link above will not render when README.md is rendered, to prevent possible script injections. And so I've created a file, jQuerify.html which contains the draggable links and you can open that with the link below to rawgit.com.

[rawgit.com: jQuerify.html](https://rawgit.com/jerryasher/jQuerify/master/jQuerify.html) contains the bookmarklet as a link that can be dragged to your browser bar.

Lessons for me
--------------

Now many of the existing impementations failed when jQuery would not be loaded, or when it did load but still conflicted with an existing jQuery. I encountered those issues to. In working around them, what I seem to have learned from this is:

+ call jQuery.noConflict() only after the script has loaded, that is, in an onload event callback. If you call it straight after asking the script to be loaded, it's often if not usually too soon.
+ set the onload callback prior to setting the 'src' attribute of the script. Setting the 'src' attribute triggers loading the script itself, and if the onload has not been set by then, it won't be called.

Now, there was some advice on the net that the new script element contained in s had to be appended to the DOM as the first step. I don't believe that's accurate. What seems more (accurate and reasonable) is that the constraints above need to be filled in that order, and if doing so, the script element can be appended either as the first step, or the last step, after setting the onload callback and the script src.

I tried that out in this alternate implementation, and near as I can tell, that works just as well.

Alternate order
---------------

<p>Clicking <a href="javascript: (function() { var jQueryPath = 'https://code.jquery.com/jquery-2.2.0.js'; var s=document.createElement('script'); s.onload = function(script) { if (typeof jQuery === 'undefined') { console.log(jQueryPath + ' loaded but jQuery still undefined'); } else { j$ =jQuery.noConflict(); console.log(jQueryPath + ' loaded and j$ = jQuery.noConflict() called'); } }; s.setAttribute('type','text/javascript'); s.setAttribute('src', jQueryPath); document.getElementsByTagName('head')[0].appendChild(s);})();"
            >jQuery Alternate Order</a> - see its code below - will insert jQuery 2.2.0 into your tab.
  The link can be dragged to your bookmarks.</p>
Other order, document append last
<pre><code>
  javascript: (function() { 
    var jQueryPath = 'https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js';
    var s=document.createElement('script');
    s.onload = function(script) {
            if (typeof jQuery === "undefined") {
                console.log(jQueryPath + 'loaded but jQuery still undefined');
            } else {
                j$ =jQuery.noConflict();
                console.log(jQueryPath + ' loaded and j$ = jQuery.noConflict() called');
            }
    };
    s.setAttribute('type','text/javascript');
    s.setAttribute('src', jQueryPath);
    document.getElementsByTagName('head')[0].appendChild(s);    
    })();
</code></pre>

### And so at the moment, ... 

And so at the moment, I am left with my model of DOM behavior being that DOM won't load anything until the script's 'src' attribute is assigned. When it is assigned, the other attributes (notably the 'onload' callback) have to be already assigned. The node itself can be added into the DOM whenever is convenient.

License
-------

As I wrote this from scratch, inspired by the others, I am placing this implementation under The GNU General Public License v3.0. See LICENSE for details. Have at it.

I use Chrome for the most part, so that's where I use this and where it has been tested.

