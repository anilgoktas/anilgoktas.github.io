---
layout: post
title: Removing Title of Back Button on Navigation Bar
---

Tried lots of code, and ...
<pre><code>navigationController?.navigationBar.backItem?.title = nil
navigationController?.navigationItem.backBarButtonItem = nil</code></pre>

<a href="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-13-at-12.10.30-AM.png"><img class="alignnone size-full wp-image-77" src="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-13-at-12.10.30-AM.png" alt="Screen Shot 2015-06-13 at 12.10.30 AM" width="300" height="85" /></a>

&nbsp;

Frustrated? Just give yourself space, literally. Go to rootViewController of UINavigationController

and set backBarButtonTitle of UINavigationItem to " "
<pre><code>navigationController?.navigationBar.backItem?.title = " "</code></pre>
<a href="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-12-at-11.58.46-PM.png"><img class="alignnone size-full wp-image-76" src="http://anilgoktas.github.io/assets/2015/06/Screen-Shot-2015-06-12-at-11.58.46-PM.png" alt="Screen Shot 2015-06-12 at 11.58.46 PM" width="193" height="82" /></a>