---
layout: post-no-feature
title: "Introducing JumpShot 1.5"
description: "A finished post about an unfinished app"
tags: [coding, iOS]

---

It took a while but I think I am finally ready to talk a bit about [JumpShot 1.5](https://github.com/jonathanstyu/jumpshot-1.5-swift), the half-baked but functional update to the JumpShot app that I created over 2 years ago. 

One thing that bothered me a whole lot about the RubyMotion app that I created back then was that it had not been written in a native iOS language. Instead, I wrote it in RubyMotion, which had been some sort of jank. Through creating the app I learned a whole lot about the system's APIs. I had resigned myself to continuing to use RubyMotion until I finally stumbled across Swift. It has been an exciting thing to go and create something real that people can actually use right in xCode just like a real programmer. 

While the app is written from the ground up and shares no code with the RubyMotion predecessor, the concepts behind it remain the same. There is a set of Game and Player screens as well as one that helps set up a New Game. You select the players on the game and then start the game. What pops up is a nice little UIView that lets you select a player and then assign to that player a particular action/stat. If you go to other parts of the app in the TabBar you can check out things like Stats. I would also like there to be some charts so that you can see where how a player's point count has been over time. 

There are a few things that I would like to use in the Settings part of the app, including most importantly, color themes. That part doesn't work yet. Dammit. 

### Cool Things I Used

* [Realm](realm.io) is the database that made this project first usable. I skipped using Core Data and used this super simple mobile database that I read about in the iOS programming Reddit thread. I loved using and highly recommend it, even though I am using the Objective-C version and needs to upgrade it to the Swift-native version (oops, haha). 

* [UIColor Extension](https://github.com/melvitax/AFDateHelper) - Tired of using UIColor.whiteColor() and blackColor() and (my personal favorite) greenColor() all the time? Well then this is going to be helpful for you. Use it in combination with one of the cool Flat Color sites that are out there ([this one](http://flatuicolors.com/) is great) and then you got something that does not look totally like a horrible piece of poop. 

* [AFDateHelper](https://github.com/melvitax/AFDateHelper) - A great little library that helps me corral the mess that is the NSDate data concept. Nothing like the clean, rich creamy smoothness of Ruby's Date function. This kinda helps. 

* [FrostedSideBar](https://github.com/edekhayser/FrostedSidebar) - I was not 100% satisfied with the way that the original JumpShot handled the player-stat assignment process in the GamePad screen. However, I was not sure of a better way to do it. While I was in the shower (where all great ideas are born) I came up with the idea of having a sidebar, one that would pop up with various menu items. I am not sure about the execution. It works alright enough but in some ways it feels a little worse. It screws up the view of the entire screen. Not only that, the code is all a jumbled up mish mash of callbacks. 

### What Still Needs To Be Done

* Graphics. I need to make the App Icon and clean up the graphics. I made the ones that I have right now in the system on PhotoShop and they look horrid. No way that this app is ready to put onto the App Store unless we have some sort of improvement 

* Add Color themes to the app. The whole thing looks horrid as it is (but I am not a designer so bite me) but one thing that I am grimly determined to build is the Color Themes part of the app. I took a crack at it about two weeks ago, factoring out the UINavigationBar and then subclassing it to give it the future capacity to accommodate a custom color theme. However I am far from actually making it work. Namely, storing the color themes seem to be an issue for me. I will figure it out, just a matter of whether or not it is going to be a horrible ugly hack that will cause real programmers to scream out as their eyes burn from the vision-acid on the screen. 

* Localization. I am excited to make JumpShot something that people all over the world can use, with a special eye towards Chinese people. Chinese people love to play basketball. They also love to track stuff. (Source: Me). The next step is definitely to open the app to a large audience by converting it into the Chinese language. 

* Bugs. I actually have not used it to track a game yet. I need to do that before I can release it. There is a lot of the flow issues that need to be addressed. Test data is not good enough. 

* Charts. I have found a fantastic [iOS charts library](https://github.com/danielgindi/ios-charts) that needs to be integrated and used to fill out the player profile screen, which is super half-baked. 

* Upgrade the Realm database to Swift. Though this is a smaller detail, I think it would be a nice to have. 

There is still a whole lot to do, but I am still super happy about this day. Hours and hours at the coffee shops and libraries, just struggling over bugs and unknown issues and Googling. It sorta feels like a tiny bit of validation even though this is nowhere near the App Store. 