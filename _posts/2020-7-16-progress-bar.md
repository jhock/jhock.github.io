---
layout: post
title: How to Make a CSS and Javascript Gradient Progress Bar
description: How to Make a Vanilla CSS and Javascript Gradient Progress Bar
image: 
published: true
---

Let's make a progress bar to denotate the position in an article as can be seen on this page or in the demo below.

First, we create a header with two div elements, one name "progress-container" to contain the progress bar and one named "progress-bar" which we will later change the width of to fill up the progress bar as the user scrolls.

<pre><code class="language-html">  &lt;div class=&quot;header&quot;&gt;
    &lt;h2 id=&quot;test&quot;&gt;Scroll Indicator&lt;/h2&gt;
    &lt;div class=&quot;progress-container&quot;&gt;
      &lt;div class=&quot;progress-bar&quot; id=&quot;myBar&quot;&gt;&lt;/div&gt;
    &lt;/div&gt;  
  &lt;/div&gt;
</code></pre>

Next, we will add the styling. You can set the width to whatever you like, in my case I used 1vh, although you may use whatever unit value you like and make it larger or smaller.

<pre><code class="language-css">.progress-container {
    width: 100%;
    height: 6px;
    background: #ccc;
  }
  .progress-bar {
    height: 6px;
    background-image: #4caf50;
    width: 0%;
  }
</code></pre>

Finally, we will add the Javascript code which will update every time the user scrolls. First we get the height of the document, then caluclate the current position on the screen, then convert to a percent to fit the width of the scroll bar. Finally, we change the width of the "myBar" id element and set the background to a <code>linear-gradient</code> using <code>hsl</code>. We chose to use <code>hsl</code> because the hue value fits our height position data well; each position percent value is tied to a hue value. Because hue in <code>hsl</code> is based on 360 degrees of the color wheel, we multiply the percent value by 3.6 to get hue values from 0 (the beginning of the page) all the way to 360 (the end of the page). The second <code>linear-gradient</code> value is the same as the first, with the hue offset by 100. You can choose to offest by any value you like to give a larger or smaller color change.

<pre><code class="language-javascript">
function myFunction() {
var init = 0;
  var winScroll = document.body.scrollTop || document.documentElement.scrollTop;
  var height = document.documentElement.scrollHeight - document.documentElement.clientHeight;
  var scrolled = (winScroll / height) * 100;
  var other = scrolled*3.6;
  document.getElementById("myBar").style.width = scrolled + "%";
  document.getElementById("myBar").style.background = 
  "linear-gradient(to right, hsl(" + scrolled*3.6.toString(10) + ", 50%, 50%) ,
   hsl(" + (scrolled*3.6-100).toString(10)+", 50%, 50%) )";  
}
</code></pre>

<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="html,result" data-user="Jahock" data-slug-hash="xxZQOEo" data-preview="true" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="xxZQOEo">
  <span>See the Pen <a href="https://codepen.io/Jahock/pen/xxZQOEo">
  xxZQOEo</a> by Joshua Hock (<a href="https://codepen.io/Jahock">@Jahock</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Additional note: be sure to place the javascript script tag so that the code is loaded early. This element will be one of the first things the user sees, and should have higher priority than anything else lower on the page. If the code is loaded too late and your codebase loads a large amount of javascript, I have noticed that the bar width can jump around jarringly as the user scrolls to different areas.

Or better yet, do not load the bar until all other javascript is loaded, then slowly fade the bar in or have it pop down. It is not needed at first anyway.
