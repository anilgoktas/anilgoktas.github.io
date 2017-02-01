---
layout: post
title: Cocoapods "use_frameworks!" issue
date: 2015-10-22
---

Allright, here's the story:

&nbsp;

I was working on an Obj-C application and want to add a Swift project via cocoapods.

Added "use_frameworks!" to project's podfile and installed successfully.

Made some adjustments, opened the project in order to see if everything is on track and ...

<a href="http://anilgoktas.github.io/assets/2015/10/Screen-Shot-2015-10-22-at-6.16.49-PM.png"><img class="alignnone wp-image-133 size-full" src="http://anilgoktas.github.io/assets/2015/10/Screen-Shot-2015-10-22-at-6.16.49-PM.png" alt="Screen Shot 2015-10-22 at 6.16.49 PM" width="420" height="63" /></a>

&nbsp;

It turned out that one of the pods was using ".xib" and was calling it by;
<pre><code>
    [[[NSBundle mainBundle] loadNibNamed:&lt;#nibName#&gt; owner:self options:nil] objectAtIndex:0];
</code></pre>
&nbsp;

After some research, find the cause

<a href="http://anilgoktas.github.io/assets/2015/10/Screen-Shot-2015-10-22-at-6.28.22-PM.png"><img class="alignnone wp-image-139 size-full" src="http://anilgoktas.github.io/assets/2015/10/Screen-Shot-2015-10-22-at-6.28.22-PM.png" alt="Screen Shot 2015-10-22 at 6.28.22 PM" width="550" height="400" /></a>

&nbsp;

However couldn't figure out how can I "use_framework" just one pod, so I inserted Swift pod manually (luckily just dragged files and added some @objc stuff)

Heads up!