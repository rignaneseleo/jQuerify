A bookmarklet to insert jQuery into a tab.
-----------------------------------------

This bookmarklet is inspired by the many bookmarklets that label themselves "jQuerify" that can be found on the web via google.

Completely rewritten so that I could understand how it works, and reordered to remove an error occuring in the usual bookmarklet that occurred trying to initialize jQuery before it had been loaded.

<p>Clicking <a href="javascript: (function(){varjQueryPath='https://code.jquery.com/jquery-2.2.0.js';vars=document.createElement('script');document.getElementsByTagName('head')[0].appendChild(s);s.onload=function(script){if(typeofjQuery==="undefined"){console.log(jQueryPath'loadedbutjQuerystillundefined');}else{j$=jQuery.noConflict();console.log(jQueryPath'loadedandj$=jQuery.noConflict()called');}};s.setAttribute('type','text/javascript');s.setAttribute('src',jQueryPath);})();"
>jQuery</a> - see its code below - will insert jQuery 2.2.0 into your tab.
The link can be dragged to your bookmarks.</p>

<pre><code>
  javascript: (function() { 
    var jQueryPath = 'https://code.jquery.com/jquery-2.2.0.js';
    var s=document.createElement('script');
    document.getElementsByTagName('head')[0].appendChild(s);    
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
    })();
</code></pre>

Since I rewrote this from scratch, I am placing this implementation under The GNU General Public License v3.0. See LICENSE for details

I use Chrome for the most part, so that's where I use this and where it has been tested.

