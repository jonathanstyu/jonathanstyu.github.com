---
layout: post-no-feature
title: "Achieving Data Persistence in RubyMotion and XCode"
description: "Working with MotionModel, NanoStore, Realm and avoiding Core Data"
tags: [swift, apps, coding projects]

---

### Background 

A while ago, I wrote about RubyMotion and making apps for the iPhone and iPad. The first app that I made was a simple basketball stat tracker for the iPhone and the iPad called [JumpShot](https://itunes.apple.com/us/app/jumpshot-simple-basketball/id646086703?mt=8). This is probably the app that I was most proud of having. 

It was a really interesting experience because it got me familiar with many of the simple APIs of the iOS system - TableView, Cells, and such - while keeping me away from Objective C. I had first started studying Ruby back in 2011 and I was not sure if I was able to quite pick up the Objective-C language in any way other than just reading it and translating it to RubyMotion. I figured that I was going to have to keep using RubyMotion as a crutch going forward. 

At the end of my dalliance with RubyMotion, I had two apps in the store which over the year had given me $100. Not really all that much, but at least it paid back the money I put into getting them into the store in the first place. 

That change during the WWDC presentation in 2014 when Apple unveiled Swift. I got really excited and since I had already renewed my Apple Developer registration (in order to keep my two apps in the App Store), I figured that I could go and pick it up so to try it out. After running through some great tutorials like [this](http://ios-blog.co.uk/tutorials/developing-ios-apps-using-swift-part-1/) and [this](http://selise.ch/tutorial/build-a-facebook-album-browser-using-swift/) I decided to work towards a particular goal. In this case, I wanted to port JumpShot to Swift. It would be totally rewritten from the ground up and feature some cool new stuff that I never got around to learning like location services and Facebook integration (the bane of my existence).

### Persistent Storage in JumpShot 1.0

The biggest challenge with this new program though - and what turned out to be one of the most frustrating parts of the entire app - was the persistent storage. For JumpShot, I needed to be able to create players, create games, create statlines for the players in those games, and then save those games to a database for future use. This needed a pretty robust set of data management and persistence tools. I actually created two versions of JumpShot and had started on a third before finally starting from scratch. 

The first iteration used a RubyMotion-exclusive gem called [MotionModel](), which offered a set of cool tools that let us create, query, and associate objects in a Rails-like manner. It was great, but at the time I made JumpShot in 2013 the gem was not completely robust. I was able to wire up the models and get the project running in a very satisfactory manner but ran into serious issues with persistence. Whenever the app closed down it would not save the data! I wanted it to save to a .dat file (which was what the documentation had suggested to be best practices) but it either did not create the file nor was it able to read it when the user opened up the app again. This was the point with the entire thing! 

MotionModel was still in production when I was building JumpShot and this bug was not cleared out yet. After a few cracks at attempting the beast myself, I decided that it was worth going in a different direction. During my research for a persistence solution on Cocoapods (which was the default storage that I used to find cool tools for JumpShot) I came across something called [NanoStore](https://github.com/tciuro/NanoStore). Described as an "open source, lightweight schema-less local key-value document written in Objective-C", someone had written a RubyMotion port of it. I immediately seized on the idea and quickly rewrote a version of JumpShot that used NanoStore as its local storage. There were several associated challenges with this conversion. For example, while MotionModel had accommodated the idea of relationships NanoStore did not. I had to restructure the data schemas to disregard relationships. So I could not call something like the following: 

<code>var statlinesForPresentation = Player.statlines</code>

This would get me nothing because NanoStore did not recognize these associations. They did something that sort of reminded me of it with the concept of "bags", where you can insert several items into bags. I tried it but found that it did not work as advertised. I wrote it off as a RubyMotion thing. What eventually became the situation was that I created keys on objects like Statlines and Games that belong to Players. A Player's ID is recorded on a Statline and Game. The Statline needs it because they need to know what Player performed the Statline. The Game needed it because it needs to know which Games the Player played in. I would then be able to query up the necessary data objects by using the searchWithStore function on the Player and Game keys.

(I did not know it at the time, but I had inadvertently replicated a part of ActiveRecord's functionality. When I went to App Academy later, one of the projects was to learn how ActiveRecord worked and it was there that I learned concepts like primary keys and secondary keys. How about that?)

### Persistent Storage in JumpShot 2.0 ... Realm?

Several months later, I decided to revisit JumpShot and update it for iOS7. What had started out as a simple cosmetic touchup turned into an epic multi-week odyssey that ended with my head in my hands wondering why the app was crashing yet again. NanoStore did not update well for the new operating system and MotionModel had gained substantial work since then. I started then to tear out all the changes that I made and reinstitute MotionModel. It was in the middle of this work that the news about Swift came out. 

Two (and a half, I guess) versions of my app and now I want to take it out of TextMate and into XCode. I have to learn Swift before I can do this but after a few online tutorials it seemed like I could understand it well enough. I was familiar with the UITableView APIs as well as little bit of the App Delegate stuff. The rest I could Google. But how can I put together a new persistent data store now that without RubyMotion I was not able to use MotionModel and NanoStore sucks?

Enter something called [Realm](www.realm.io), which came to me during a random HackerNews scroll. What I read reminded me a lot of NanoStore in that there was this free floating database that lived locally within your app and you used to query up items and objects. 

In the next post, I try to install this on xCode 6 Beta 4 with much pain and trouble. 