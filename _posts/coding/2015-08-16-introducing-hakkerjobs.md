---
layout: post-no-feature
title: "Introducing HakkerJobs"
description: "A Better Way to Browse Startup Jobs"
tags: [coding, iOS]

---

I launched JumpShot 2 on July 13, 2015 but just when it was hitting the App Store, I was already thinking about the next app that I should make. This would be the fourth app that I would make and the second that would tap into an online API. I needed an idea and this one came to me while I was browsing the monthly automated thread: "Who is Hiring?" Hackernews. I found myself irritated that it was not all that easy to read the content that came out of it. It is a web thread, yes, but at the same time it has a lot of valuable information inside. But it is all about in a mumbo jumbo and not that easy to work with, not to mention read. I wanted at least a better way to read all those listings. 

One month later, I am introducing to you HakkerJobs, a Hackernews client specifically for finding jobs. A few days ago I submitted it to the App Store. Who knows if it gets approved, but I wanted to sit and reflect a bit on the effort that went into making it and what I have learned. 

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/HJ-Preview.gif' style='width: 400px'>
</figure>

### Lessons

* The key is to do it fast 

JumpShot 2 had been such a long and grueling slog (the entire project went 5 months in between initial commit and last commit) that I was not sure if I wanted to do another programming project. I thought about going out and starting on a new book or something. It was only because the idea was so compelling that I decided to dive back into Xcode again. I was not sure if I wanted to spend another few months fiddling with code. For a long time I had no idea why I found going fast such a powerful concept but I felt that [this blog post](http://jsomers.net/blog/speed-matters) really nails. 

>>If you work quickly, the cost of doing something new will seem lower in your mind. So you’ll be inclined to do more.

Underwriting this concept, I wrote HakkerJobs within a month. The unskinned, no-feature prototype was committed in 20 days. 

There are a lot of reasons HakkerJobs got done so much faster. One is that the app does not need to extensively use fancy-shmancy layouts. It is a blunt simple thing: It reads a feed, presents it thoughtfully, and lets you do stuff with it. The things that did take time to organize and arrange though I used a few libraries (but not too many as see below) like [SnapKit](https://github.com/SnapKit/SnapKit).  

Second, I reused a lot of code from Jump Shot 2. If you could look at the code behind the app you would find a lot of similarities. Certain products and features that took hours for me to understand when going through it the first time I quickly recognized and fixed with a simple copy-and-paste. 

Third, I just spent more time at HakkerJobs. Instead of working on it during the weekends, I wrote code - even just a little bit - during my commute home from San Francisco. I was tired a lot, but much better to get this done so quickly.

* Build as much as you can on your own (the less plugins the better)

One of the biggest problems that I came across came at the end of finishing Jump Shot 2. I had used a vast array of plugins and downloads to make the app do what it needed to do which led to HUGE errors when archiving and compiling the final app product. So much of the code was not mine, I had no idea what was broken! But I could not do anything about it because I had already designed the feature around this plugin (called a Cocoapod). At the end of it, I had this crippling Xcode compiling error that took a full 3 days to resolve - and I only resolved it by creating an ENTIRELY NEW Xcode project and transferring the core files to it. 

This in addition to a new upcoming transition that was making everything a bit more challenging. Xcode 7 had been released along with iOS 9 and Swift 2.0. Brand new code that changes everything across the board. Not all of the projects that I had been relying on would capably bridge the transition. 

In the end, I wanted to keep only the things that were absolutely necessary. I kept things that helped me with layouts (SnapKit) and simple things like customer onboarding (Like [EAIntroView](https://github.com/ealeksandrov/EAIntroView)). 

### What's Next?

If the app gets some traction and people like it, the next things that I would need to work on is obviously filtering. People want to be able to easily filter through jobs that don't meet their criteria (job tasks, location, salary, etc.). Problem is that I am not quite sure how to do that. The job posts are in natural language with little structure (oh why, people, why?!). Sometimes there are multiple jobs, multiple criteria. 

Until I come across my next app or thing idea, I will be tweaking and tinkering with some natural language libraries like NSLinguisticTagger (already built in the app but not used yet) to see where I can take this next. 