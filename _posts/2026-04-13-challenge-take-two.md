---
layout: post
title: "Challenge Take Two - Electric Boogaloo"
description: "The continuation of \"Yes, I Like A Challenge\" - Troubleshooting scss style sheets for a Gem based blog and reflecting on how to learn to learn"
date: 2026-04-13 12:00:00 -0700
categories: [Blogging, Troubleshooting]
tags: [github-pages, jekyll, linux, tutor, learn, document]
author: Siona
---

Since about High School, I've tried to live by a core principle: don't learn the subject, learn how to learn the subject.

I got it from my dad, who went to school for English writing but also computers. His father was a librarian and my dad also did some teaching at the college level as he was doing his studies. So he came from a pretty studious background and passed that on to his kids as well.

There are definitely times where that is a challenge, when I'm in the mad dash at the end of the class and I just need to *memorize* what I need to pass the final test. But the principle still remains as I learn anything, whether for school or starting a new job.

This is probably on my mind right now because I will be starting a tutor position this quarter and there's a kind of recursive logic to learn how to learn how to tutor others in how to learn how to learn. But I think one key thing that can get lost as you master a subject is remembering *learning* the subject. Because it can get easy to boil down a concept that took you years to learn and go "oh, it's simple" as you teach others.

So I guess I'm hoping to record the process of me learning.

***

Getting my blog to actually load would, it turns out, be only the first hurdle. I had another two day challenge on my hands as I posted my first blog post and I didn't even know it at the time. This next part, I thought, was going to be easy! 

Certainly the Chirpy starter guide acted like it was easy.

```
### Customizing the Stylesheet[](https://chirpy.cotes.page/posts/getting-started/#customizing-the-stylesheet)

To customize the stylesheet, copy the theme’s `assets/css/jekyll-theme-chirpy.scss` file to the same path in your Jekyll site, and add your custom styles at the end of the file.
```

Well the thing was that the `assets` folder in the starter repository I forked is completely empty. So I'm scratching my head going "There's nothing to copy." 

Since it doesn't exist, I guess I'll make my own. I create `/css/jekyll-theme-chirpy.scss` and start asking Gemini how best to set up a style sheet. I set up the initial test, loaded up the server, and...

Nothing. Still all default.

Okay, a bit of back and forth and we determine we need to make sure we specify the colors are used for dark mode, my default. That worked. Great, I figure the rest is just nailing down the aesthetic and tuning the colors. Once I get something I like, I push the changes and check the actual site.

It was broken. 

{% include browser-img.html src="/assets/img/posts/brokensite1.png" alt="screenshot of a portion of the chirpy site with purple background and mint green text colors but new white dots, skewed text, and icons arranged vertically" %}

Things had been completely shifted around on the page, the clean structure completely scrambled. The colors had worked but it wouldn't do if my technical blog looked broken.

{% include browser-img.html src="/assets/img/posts/brokensite2.png" alt="screenshot of a portion of the chirpy site with purple background and pink text but the searchbar aligned left and most of the main panel acting as the link to the blog post" %}

***

Picking and choosing what details to put into these blog posts is difficult sometimes. If I go into every single change and tweak and thing I tried, this post would be far too long. But I also know that in a year, two years, ten years or whenever I'm looking back on this, I'm going to realize there was a detail that I was looking for that I left out.

Because I don't know that it's important right now. That's the thing with learning, when you start out knowing barely anything, it all seems important. But holding on to everything does you no good and at the same time you don't know enough to know what *will* be important.

It's the beginner's catch-22 I suppose. That's why teachers exist, why repetition is important, why knowing how to learn the subject is important. Your future self is going to thank you more for the link to the tutorial that got you through a task much more than they will for rote memorization just before the final exam.

***

Gemini starts giving me code to try and fix the structural issues on the site. I'm opening up the developer pane and trying to read the code to figure out where the mismatch in instructions is happening when the site loads in. It's obviously something that happens when Github assembles and launches the site because when I'm working locally, it looks fine.

We'd get one thing fixed but the moment we start focusing on something else, the original thing broke in new and creative ways.

I decide to nuke it all and start over. Introduce one thing at a time. Just some text color. Push that to the site and it seems to be working. Okay, just a little bit more...

And no, it's broken again.

I'm conversing with Gemini and I'm trying to find tutorials but every single one just skips to how to write up the code for coloring and none of them mention this format breaking. Not helpful, I've got that code. The few times I do find some forum post that talks about the format breaking, there was never any solution.

I'm taking swings at the code myself. I changed the `_config.yml` file to only do dark mode so the `scss` file only needs to load up one set of colors. Still not the solution. Gemini wants me to use `:root`, I'm having better luck with `html`. We'd get the structure fixed and the colors wouldn't work; the colors worked, the structure of the site failed.

Finally, I got fed up with Gemini and I asked Claude. That conversation started about the same, though I think Claude struggles more with interpreting photos than Gemini, so I had to do more explaining. Just as Claude is about to have me dig into other `.yml` files, mid post it goes "Wait. Do this instead."

***

So, remember earlier in the post where I shared the only directions the Chirpy getting started page had to say about customization?

Yeah, so they meant to go copy the `scss` file from the **original** chirpy repo. It really was that crazy simple. 

And yet I do believe that it wasn't clear. I certainly didn't understand what it was saying, I still look at that sentence and don't think it's descriptive enough in its instructions. There's a big level of assumption going on in the wording that means I missed the "obvious", two AI chat bots missed it, and yet nowhere online could I find a single explanation of what I was missing.

Because it's really simple, right? That's basic stuff that you should just know and understand.

But I didn't. I've taken *one* IT class. I don't know what I don't know. And I think the opposite sometimes becomes true to experts: they don't know what they know. Asking them to remember that not everybody comes in with the same knowledge level to understand where to copy a file from is like asking anybody to remember when they learned the Earth was a sphere.

I've got a freakishly good memory of my childhood; I remember learning about acid rain when I was four. I do not remember learning the Earth is round.

***

Okay, the tl;dr

> If your site structure keeps breaking when you update colors in your `scss` file, the answer to how to fix it is below:
{: .prompt-tip }

Create `assets/css/jekyll-theme-chirpy.scss`. Use `mkdir` or use Visual Studio gui to make it.

Insert this at the top of your file:

{% raw %}
```bash
---
---

/* prettier-ignore */
@use 'main
{%- if jekyll.environment == 'production' -%}
  .bundle
{%- endif -%}
';
```
{% endraw %}

You're welcome. 

That's literally it. Once that was input at the top of my file the blog site worked a treat. I don't know why this isn't just included in the starter repo or explicitly stated in the getting started guide.

I guess so I could go on a two day journey that I could reflect on and have a philosophical discussion about the process of learning so we can better teach. Is that cope? I certainly wish I could get *some* of that time back. But I also didn't have to sink all those hours into it consecutively. I could have stayed with the default grey colors and come back when I had actual time.

But I knew the answer was out there and all of my methods from "learning how to learn" weren't doing it for me. I figured that accepting the default grey wasn't giving up on coloring my blog, it was giving up on learning *how* to uncover the answer.

But if this helped you please let me know; I've got comments set up now! (That also had a learning curve but was less of a headache)

Thanks for reading.
