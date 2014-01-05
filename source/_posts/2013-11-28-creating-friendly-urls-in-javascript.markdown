---
layout: post
title: "Creating friendly URL's in JavaScript"
date: 2013-11-28 13:57:32 -0200
comments: true
categories: development javascript
author: "João Lucas Pereira <a href='https://github.com/jlucasps' target='_blank'>@jlucasps</a>"
---

Hi people, what's new in this world of blogging and posting, hum?

Long time ago, Rafael Rocha <a href='https://github.com/rafaelrocha' target='_blank'>@rafaelrocha</a> and Me 
have decided to start blogging about **software development**, **computing**, **technology** and this kind of stuff. But, as always,
have been so hard to find a free time to write. Then this week I made the first move, now it is up to you Rafilds.

I'll start writing about a very useful feature in modern web apps: **friendly URL's**

<!-- more -->

Friendly URL's are important for SEO <a href="http://en.wikipedia.org/wiki/Search_engine_optimization"  target='_blank'>(See at wikipedia)</a> as well as for human readability. 

One of these days, in a project at <a href="http://synapses.com.br/" target='_blank'>Synapses</a>,  I needed a 
JavaScrip code to convert a user's input in a text that could be easily readable, and also used as URL of a web page.
Then I Googled a lot in blogs, post and Stack Overflow, then made a satisfatory function to do it.

Here is the source:

The function <code>friendly_url(str, max)</code> (in Ruby naming convention) takes two parameters: 
<ul>
  <li><code>str</code> being the text to convert;</li>
  <li><code>max</code> as maximum length of output String, set do <code>32</code>  if not defined.</li>
</ul>


{% codeblock Javascript Friendly URL lang:js %}
// Transforms String in URL
function friendly_url(str, max) {
  if (max === undefined) max = 32;
  var a_chars = new Array(
          new Array("a", /[áàâãªÁÀÂÃÅª]/g),
          new Array("e", /[éèêÉÈÊ]/g),
          new Array("i", /[íìîÍÌÎ]/g),
          new Array("o", /[òóôõºÓÒÔÕ°Ö]/g),
          new Array("u", /[úùûÚÙÛµÜü]/g),
          new Array("c", /[çÇ¢©]/g),
          new Array("d", /[Ð]/g),
          new Array("n", /[Ññ]/g),
          new Array("s", /[Šš§]/g),
          new Array("z", /[Žž]/g),
          new Array("y", /[Ÿ¥Ýýÿ]/g),
          new Array("ae", /[Æœæ]/g),
          new Array("ce", /[Œ]/g),
          new Array("r", /[®]/g),
          new Array("0", /[º]/g),
          new Array("1", /[¹]/g),
          new Array("2", /[²]/g),
          new Array("3", /[³]/g)
  );
  // Replace vowel with accent without them
  for (var i = 0; i < a_chars.length; i++) {
    str = str.replace(a_chars[i][1], a_chars[i][0]);
  }
  /* first replace whitespace by -, second remove repeated - by just one, third turn in low case the chars,
   * fourth delete all chars which are not between a-z or 0-9, fifth trim the string and
   * the last step truncate the string to 32 chars
   */
  return str.replace(/\s+/g, '-').toLowerCase().replace(/[^a-z0-9\-]/g, '').replace(/\-{2,}/g, '-').replace(/(^\s*)|(\s*$)/g, '').substr(0, max);
}
{% endcodeblock %}

It replaces non-common symbols, signals and spaces by usual letters. 

Here I show some inputs and outputs when using the function above.

{% codeblock output lang:js %}
  friendly_url("João Lucas Pereira de Santana")
  "joao-lucas-pereira-de-santana"

  # Long text
  friendly_url("Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a ty")
  "lorem-ipsum-is-simply-dummy-text"

  # Long text with limit of 50 characters
  friendly_url("Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a ty", 50)
  "lorem-ipsum-is-simply-dummy-text-of-the-printing-a"

  #
  friendly_url("string with ça and ção")
  "string-with-ca-and-cao"

  #
  friendly_url("a lot    of      spaces    between words")
  "a-lot-of-spaces-between-words"
{% endcodeblock %}


So... that's all folks. Next week I'll post about Ruby, may be Rails :-D