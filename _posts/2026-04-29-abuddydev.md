---
layout: post
title: "Dev Log: I Made A Little Guy - Abuddy ૮(„• ⌔ •„)ა⛑"
description: "Discussing the reasons behind creating a script to motivate studying and stumbling into giving it personality "
date: 2026-04-29 12:00:00 -0800
categories: [Dev Log, Abuddy]
tags: [shell, bash, scripting, pomodoro, tux, devlog, code, development, abuddy, v1.0]
author: Siona
---

```
૮(„• ⌔ •„)ა
 Hi, I'm abuddy
```

*Note: This is a discussion of the original out of the box Abuddy. If you visit the repo, you'll notice there's already a v1.1 - v1.1 holds everything I'll be discussing but some features might be a little different.*

 [My life update also talks about abuddy a little]({% post_url 2026-04-29-abuddypersonal %}) and my [repo](https://github.com/sionalarsen-sys/accountabili-buddy/tree/main) can be found here.

## What I Wanted
A timer for studying that reflected my specific goals and schedule. I wanted to ensure I was taking frequent breaks instead of hunching over my computer for hours and actually keep track of my focused study time to better manage my schedule. I also didn't want the output of goals, break activities, and others to be exploitable - once they're set, they're set and the only thing to do is move forward. That way I could be sure my studying time wasn't filled up with hitting "randomize" over and over until I got what I wanted.

## The Method
I started from the basis of the Pomodoro study method: 4 blocks of usually 25-50 minutes with breaks in between of 5-10 minutes. This system already suggests setting your block goal each time and making sure to get up at the breaks to drink water, look away from the computer, stretch, etc. The rest was fine tuning until I felt I hit my own brief.

{% include browser-img.html src="/assets/img/posts/4292026/pomotest1.jpg" alt="Screenshot of a terminal showing a running pomodoro session labeled Harbor-Glitch Pomodoro with default settings except only one session, the first session wrapped, and the break beginning" %}

### Timer
At first I was concerned, because this requires a timing type feature and I hadn't done that before. That didn't stop me, obviously, but I worried that my ambition had gone much further than my reach this time.

But it's BASH. The answer is sometimes quite simple:

```bash
sleep
```

and

```bash
read -p
```

After that, it was all a matter of putting together the `for i in {1..20}`  and index caching to count down time. Honestly, I don't know why I had thought I hadn't dealt with this before -- this was absolutely a feature I played with in Ren'Py for dynamic day/night cycles!

### Goals
This was my "set the intention before each block" creation. I wanted to be able to input specific things I was working on (Network+ Module 5.1 for example) but if I couldn't think of anything, have a randomizer pull from a list I could update. My thinking for the method was that if throughout a day I thought of things I needed to do, I could add it to the .txt file. When I run abuddy, I might type my own goal or let it pick from the file to suggest. That way no study time is "wasted" by not knowing what to do.

### Breaks
This was honestly where I put most of my development. I had several desires to make the break time productive as well. The first implementation was a shuffled suggestion of things to do on the break: stretches, small exercises, get water, etc. But then I thought of other break time things that would be absolutely circumstantial so shouldn't come up every time: checking the laundry, answering a text message, etc.

For those reminder, I decided to handle it a little like the goals. Before you start a study break, put in that reminder. If you know your laundry timer might go off in the middle of the study block, set a reminder, and ignore that timer until the break reminds you to check.

### Passage of Time
My first several uses of abuddy were a little nerve racking. I had done some bug testing so I knew that the study blocks and the breaks were progressing, but with no visible suggestion of time passing I always worried that the terminal had frozen.

To address this I made two changes: 
1. The script printed out the time a block or break started and calculated what the end time would be. This meant you knew not to worry it was frozen until a certain time.
2. The breaks now offer a new break time suggestion every minute until end of break with a countdown. That way if I wanted to do more stretches or other break time activities I had suggestions.

### Transitions
At first, the script ran from study block through to the end of the break before stopping, with a time out function on the prompt at the end of break to continue forward if there wasn't input.

I didn't find that particularly helpful. Sometimes I was wrapping up a quiz at the end of the study block and wanted to finish it before my break. Sometimes I was in the other room when the break finished so I came back to an already rolling study block.

The solution was to add stops with "hit enter to continue" prompts. That way each start time was controlled by the user.

That then introduced the problem (or made it more visible) of not knowing the script was waiting for that input. To that end, I input a bell command. This required some troubleshooting to get it to work (a system side issue, not a script issue) but I've now had the bell work on three platforms: Windows Powershell, VSCode, and Github Codespaces.

{% include browser-img.html src="/assets/img/posts/4292026/noaidebugbrag.webp" alt="Screenshot of a terminal showing the ending of the abuddy script running with the final break complete with reminder, the answered reflection question, and the win prompt answered" %}

### Keeping a Record
The final key piece I wanted was to be able to log what I had done. Key information was cached as a variable and set up to write to a log: start time, end time, goals.

On top of that, I added a reflection portion before writing to the log. A place to comment on how the studying went. Make plans for the next study session or get excited about what went well. Whatever felt helpful, though I did incorporate a list of random questions as prompts.

{% include browser-img.html src="/assets/img/posts/4292026/logexample.webp" alt="Screenshot of a markdown file with a copy of a test log I ran to make sure I'd fixed some bugs" %}

This also got added to the log as the wrap-up piece.

### Wins
A few more use cases later, I realized what could be really nice is a motivational message at the start. But I didn't want those to be generic -- I wanted the motivation to come from me. Sometimes when things are getting hard and the to do list seems daunting, it's good to pull up a list of wins to remind you that you've done it before.

So I added another section to the final reflection. Input a win from the session and it would not only write to the log (if input) but it would append to the wins.txt file to potentially be pulled up on another session.

The final key was to input that message at the start of the script. 


## Building abuddy
`૮(„• ⌔ •„)ა🔧`

When it came time to create a repo so other could use this system I'd come up with, I knew I had to name it something. I was realizing that I had moved beyond the realm of a Pomodoro timer with some helpful remarks. I was building a helpful remarks timer that used a Pomodoro type shape - completely different.

To that end, I didn't want to name it Pomodoro, or Pomo-this, or Pomo-that. I started brainstorming words the script purpose made me think of: interval, study, focus, accountability...

### The Name
The word accountability clicked with me the moment I wrote it: the script gives you accountability and then it checks in to see your progress. No judgement, things happen, but here's a record so it's concrete fact rather than an abstract feeling.

I know, Accountabili-buddy is a bit of a mouthful and a little difficult to type. But I also found it charming and incredibly me. It also reminded me, strangely, of Clippy from old Microsoft Word in an affectionate way. And suddenly Abuddy had a voice and a personality because I'm still a writer at heart; I can't help myself.

### The Voice
I didn't want to mess with the main script too much and make it obnoxious - the Clippy comparison was supposed to be in concept only. But it gave me something to liven up the install script I was working on. Especially the demo I wrote. I wanted them to fit with the helpful and supportive vibe but a little quirky; like me!

{% include browser-img.html src="/assets/img/posts/4292026/abuddyfirstrun.webp" alt="Screenshot of a terminal running the start of the install script for abuddy complete with the stand in emoticon mascot and instructions written in a character voice" %}

### The Look
Once I had abuddy as an entity rather than just a time script, I suddenly wanted a visible mascot. But with 3 CompTIA courses, tutoring hours, moving checklist, and other life stuff, I knew I should prioritize something quick and put the actual art on the dream wish list for the future. I looked at ASCII art but ultimately a basic emoticon felt more right. I am still a person that regularly uses `XD` rather than emoji, so an emoticon felt on brand.

### The Story
This is where I got indulgent (ignore everything else about this script so far): instead of picking an emoticon and working from there, I came up with a little bit of a story about abuddy. Nothing massive, I'm not designing a Dungeons & Dragon character, but along the same lines of Linus Torvalds suggesting a cute, full, and satisfied penguin which became the Linux mascot.

Because Tux the mascot is open source, there are a lot of variations on him and I felt like that was a very good place to start. But again, I paired down what I was going to output and instead I went looking for emoticon shapes to mix and match and make a penguin. Thus, Tux's littlest brother (gender neutral) was created!

## Summary
As I was making the README and getting ready to push to github, I reflected on the question "why abuddy?" There are many pomodoro and focus apps out there, available on your phone or on your computer. Some have calendar connection functionality, internet blocking abilities, or push notifications. So I focused on what I did have:

- Built in session planning at the start
- Flexibility to change your goal each session based on what you know you need to do right now
- A focus on applicable breaktime activities; just as important if not more than the work
- A reflection log that saves so you can see how much you got done and reference back if you need to see where you left off or when you accomplished something
- Inspiration in the form of your own personal wins, written by you for you

## Use Case 
Yesterday, my to do list was:
- [ ] A+ 4.3
- [ ] A+ 4.4 and 4.5 labs
- [ ] A+ 4.6
- [ ] A+ 4.7/4.8 quiz
- [ ] EET 131 worksheet
- [ ] Two important emails
- [ ] Make updates to abuddy's script for v1.1
- [ ] Update the install script for v1.1
- [ ] Reorganize the .txt file structure for v1.1
- [ ] Create an update script that doubles as a reconfiguration script

I did one session of 4 blocks at 25 minutes each and one session of 2 blocks at 25 minutes each. After my to do list looked like this:
- [x] A+ 4.3
- [x] A+ 4.4 and 4.5 labs
- [x] A+ 4.6
- [x] A+ 4.7/4.8 quiz
- [ ] EET 131 worksheet *Half done, finish up before class tomorrow*
- [x] Two important emails
- [ ] Make updates to abuddy's script for v1.1
- [ ] Update the install script for v1.1
- [ ] Reorganize the .txt file structure for v1.1
- [ ] Create an update script that doubles as a reconfiguration script

The abuddy stuff mostly got done that night without using sessions. In just about 3-4 hours I got everything important I had on my list done and still had time in my day to work on some fun stuff. This contrasted heavily with my old system of blocking out the whole day and telling myself "get this done today" with a list and feeling like I was burning through 8-12 hours studying when I was actually distracting myself constantly, looking at the internet, answering text messages, etc.

Basically, I call it a success in modifying my studying behavior, aiding me in focusing on specific tasks, taking breaks to move around, keeping a record on my accomplishments to ward away my self doubt, and actually completing tasks in a reasonable amount of time that I can actually quantify instead of saying "all day."

{% include browser-img.html src="/assets/img/posts/4292026/abuddycodespacerun.webp" alt="Screenshot of a codespace terminal running the abuddy pushed to the repo successfully" %}

## The Future
As of finishing this blog post, I've already pushed a 1.1 version of abuddy and that is what you'll see in the repo. Version 1.0 is still available if you click on the tags, but the basics describes here are still there, there just some additions.

- A reward system that allows you to set a treat for yourself for the break or pick a random one
- A goal completion tag so that you have definitive marking in the log of goals that are completed (currently not on the final goal, that fix is incoming)
- A buffer timer that counts up time between the end of focus rounds and breaks to when you press enter to start the next block
- An end of break prompt where you can add material to the `exercise` and `nudges` lists that get pulled during the break for more immediate and active additions
- An options big break of 20 minutes after the reflection to encourage stepping away from studying for longer than 5 minutes to recharge and reset.
- Additionally, the configuration set up in the update script offers toggles on some variables to make them optional. This includes at the moment: 
	- Big break (set default time)
	- The label of the focus blocks (default is Focus Round)
	- Alerts (sound on or off)
	- Rewards (ask each session, always on, or off)
	- Goal completion tracking (ask, always, or off)
	- Buffer time review (ask, always, or off)
	- Break reflections for nudges (ask, always, or off).

In the name of self care, I wanted some aspects to be customized for what will motivate *you* rather than only features that motivate *me.* 

But I'm not stopping there. I have other plans that I want to implement -- some in the near future and some probably far off in the future:
- Fix the final goal completion check for the end block
- Change timestamps to include seconds
- Update the break questions to better get answers that will fit in the nudge list
- A demo mode: it'll still live in the install for first time installs, but there will also be another script that just runs the demo, especially for future updates so all you need to do is run update and demo
- Mid-round reward reminder
- Big reward list for your big break at the end of the session
- Dynamic focus labels so you can choose to randomize every session for fun
- Mascot art

## Feedback
૮(„• ⌔ •„)ა📝

If you think of anything that would make abuddy work for you better, I'd love to hear about it! Maybe I'll also want it for my copy but I can definitely see if adding it makes sense - after all, the toggle system is already set now, it would be easy to add in other optional content.

I also want to hear back on if some of my wording missed the mark on inclusivity. I tried to make the default exercises something that most people could do ("go to a window" rather than "walk to a window"). I haven't made the same pass on the rewards list yet and something is always bound to slip past me. 

Please also send me your updates! Did you use abuddy and get through a massive list like me? Have you added some rewards or exercises that you think would make a good addition to the starter pack? Did you go into the script guts and change it in some way for you? I truly want to hear back!
