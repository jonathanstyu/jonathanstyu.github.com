---
layout: post-no-feature
title: "The Anatomy of a Stupid 4-Hour iOS App Bug"
description: "Also known as why I am the stupidest coder in the world."
tags: [swift, coding, iOS]

---

#### The Reason For This

Sometimes people ask me why I don't go and become a developer in real life. The reason is because I know that I am a horrible developer. No, literally. Every time I write a line of code I look at it and think, "My God, Jonathan this is the worst line ever written in the history of computers." That's how I feel everyday. 

For today's post, I am going to write about an unusually nasty bug I came across in a coding side project that I have been working on for a while now. It is also going to be my written reminder that I am the *Worst Coder in the World* and should never be let into any sensible Developer space. 

Some background, I have been working on an update to the JumpShot basketball stat tracking app that I created two years ago. Whereas the [previous app](https://itunes.apple.com/us/app/jumpshot-simple-basketball/id646086703?mt=8) had been written with the help of [Rubymotion](http://www.rubymotion.com/), this is going to be a fully native app written entirely within the XCode IDE and with Swift. I started this project in early February and have been working on it on and off in coffee shops and such with my free time. This is one of the many bugs that has caused progress on this app to come in fits of stops and starts. 

####The Route to the Bug

It was March 29th and I had been working on an essential part of the app, the visual score and stat tracking view that would be used to record the stats of players in games. This was the most important part of the app in my opinion because it is what people will see. I also intended it to be the main marketing image for the app. So to be short, it is goddamn important.

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/jumpshotmenu.png' style='width: 400px'>
	<figcaption>What the Score Menu in Jumpshot 1 looks like</figcaption>
</figure>	

When you are building an menu like this, I learned that it is possible to create and place elements on the screen without the WYSIWYG interface of the storyboard/XIB. Programmatically placing these would be generating them with code elements and placing them using code. It is generally more flexible but really slow and frustrating work.

    @player_viewer = UITableView.new
    @player_viewer.frame = CGRect.make(x: 0, y: 0, width: view.bounds.width, height: view.bounds.height)
    @player_viewer.delegate = @player_viewer.dataSource = self
    @player_viewer.backgroundColor = 0xecf0f1.uicolor
    @player_viewer.separatorColor = 0x7f8c8d.uicolor
    @player_viewer.rowHeight = 75


The code sample above demonstrates how one might initiate and place a UITableView programmatically. You can see how it might result in a lot of repetitive code that is tedious and difficult to review. Considering how long it took, I decided to opt for an alternative solution, listed below. 

In the RubyMotion app JumpShot 1, I initiated the GamePadViewController (the piece of code that would handle all the logic and views of the game pad as it was called) by calling on a XIB file, which is sort of like a PSD but for XCode. It would have the visual representation of what the screen would look like. I would then wire up the different elements by calling the [viewwithtag](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIView_Class/#//apple_ref/occ/instm/UIView/viewWithTag:) function, which would essentially "target" it in the code. 


    @team1_label = view.viewWithTag 4
    @team1_label.text = "#{tally_points(1)}"
    
    @team2_label = view.viewWithTag 5
    @team2_label.text = "#{tally_points(2)}"

Again, JumpShot 1 is a RubyMotion app so this piece of code is written in Ruby and not in Objective-C or Swift. Once the variable has been targeted successfully with the viewWithTag function, we can then manipulate it as we wish programmatically within the ViewController. 

I tried to repeat the trick again for JumpShot 2. I had just struggled through programmatically creating all those UI elements on the other viewcontrollers - the ones that handled the creation of a player or a game. This seemed like an easy short cut way to get to a striking visual design. However what I quickly found was that what worked in RubyMotion did not always work as well when it came to Swift and XCode. The NIB file failed many times to initiate and when I began switching between the different devices - iPhone 6, 6+, or 5S - I saw horrendous errors related to autolayouts and constraints. What was worst was that at some point after all that jiggering the whole shmuck decided to crap out on me and do some sort of ridiculousness like what you see below: 

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/wtfgamepad.png' style='width: 400px'>
	<figcaption>To quote my sister when she reads through her IRA docs, 'WTF IS THIS?!'</figcaption>
</figure>	

So forget that. I decided to bite the bullet and make the whole thing programmatically. I was wasting so much time on it that I might as well spend that time on the long and difficult path. So that is the backstory. Here is the bug. 

####A Wild Bug Appears!

I was programmatically generating a skeleton of the GamePadView - which would get styled later as I saw fit. The goal was to create a section within the GamePad that would let the user select a player to whom he can assign a point made or rebound made or assist made or whatever. 

I had implemented an interesting For Loop that would programmatically add the buttons within a specific UIView as a Subview that would contain the buttons. Problem though was that when I ran the code in the iOS simulator I didn't see anything.

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/errorview.png' style='width: 400px'>
	<figcaption>Yeah it's ugly but I'll fix it in Post</figcaption>
</figure>

The player names are supposed to go in the grey and yellow areas as Buttons. Why can't I see them?

Puzzling still was what I went into the debugger hierarchy provided by Xcode, I could see the buttons just fine. 

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/debuggerhierarchymystery.png' style='width: 400px'>
	<figcaption>The Mystery Deepens ...</figcaption>
</figure>

The buttons seem to be instantiating just fine. The For Loop, which I had thought to be a bit risky, seems to be doing the core work just fine. So what is going on? Why was this visible on the debugger but not the iOS Simulator?

####Throwing Safari Balls

I did the first thing that I did when I come across a bug: Google it. There was no error message so I did not have anything to go on so I Googled the next couple terms: 

* "uiview addsubview button failing"
* "uiview addsubview not working"

A few StackOverflow articles came up and discussed the possibility that the relevant UIButtons might be going outside of the bounds of their parent UIViews' frames. This would cause the buttons to not be tappable. 

I went and fiddled with the UIButtons within the code. I pulled back some of the cute stuff I had tried to dynamically size the button based on the parent view's frame (I can always add it back later. I just want to get it working). Didn't work so I moved on. It seemed like a promising idea though so I kept it in mind and it will appear again shortly later. 

* "opaque subview"
* "opaque=NO subview"

At this point I decided that it might make sense to output the containing UIViews (the ones that held the For Loop instantiated UIButtons) to code with a PrintLN. I did and noted that the Subviews had been identified by the iOS debugger with an interested property: "OPAQUE=NO". So back to Google ... 

I came across [this article](http://stackoverflow.com/questions/10444212/uiview-opaque-property) that says that by default this should be set to YES instead of NO. This seemed promising so I went ahead and worked with that. I tried to hard code the UIButtons as they were added to the UIView. Nope that did not work. In fact it seemed to not even have affected the debugger PRINTLN output either. I must have done something iOS does not allow. Jesus I suck. 

* "uiview addsubview on top"
* "bringSubviewToFront swift"
* "subview overlaying uiview"
* "subview on top of parent uiview"

At this point I began playing with the properties of the UIView parent view. I switched their background color to transparent and saw that the buttons did exist underneath them. It just seemed like the parent view was blocking them, preventing them from being seen or touched. 

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/interestingsubview.png' style='width: 400px'>
	<figcaption>The Mystery Deepens Yet Further ...</figcaption>
</figure>

I felt then that maybe I could just avoid the core problem and just force the code to basically place the UIButtons at the top of the view stack no matter what. I found out how to do this through this [helpful article](http://stackoverflow.com/questions/26850501/swift-bringsubviewtofront-not-working) and implemented it into the app. Nothing happened. Same view as above. Why couldn't I goddamn see this thing? 

I briefly considered not having a UIView at all but determined that it would be a bad experience just adding the buttons directly on the view. Placing them in context of the whole view would be a nightmare and if someone selects just 1 player for his game then we either get a blank empty space for a button or 1 big huge button. Eh, not the best experience but I kept that in my back pocket as a possible thing to try if I finally got the end of the road. 

By now I was getting pissed. This one bug had already sunk about 1.5 hours of my time. I consider myself a fast learner and a problem-solver. I have come across bugs like this before. Usually I crack them relatively quickly and move on to the next problem. Except ... this one was being unusually resistant to me. What was I missing? And the worst thing was that I did not actually have an error message to work with. Jesus, what the hell, man?

* "uibutton restrict bounds"
* "uibutton bounds"
* "subview superview bounds"
* "set subview superview bounds"

At this point I needed to pee and since I was all alone in this coffee shop and couldn't get someone to look after my stuff I packed up and went to the bathroom. While peeing I revisited my previous theory: That the UIButtons were somehow invisibly violating their parent view frames. I looked for a way to programmatically force them to stay within the definitely known bounds of the parent UIView. 

The most helpful article that I came across was [this one](http://stackoverflow.com/questions/25808875/how-to-programmatically-set-a-subviews-frame-to-fit-it-inside-its-superview) but it did not really teach anything that I did not know. In the code, I guided the frame variables by using Self.view.frame and decided to switch it on a lark to "bounds". Didn't do anything. The UIButtons still did not appear despite appearing just fine in the view hierarchy. 

* "didlayoutsubviews"

This was a tangent shot in the dark kind of idea that I had while staring at this girl who walked by with a muffin (actual muffin) overflowing over its paper wrapper. Perhaps the bounds idea was not working because the dynamic sizing that iOS was doing to fit the GamePadView within the particular iOS device. This had solved a previous issue while I was still struggling with the Autolayouts of the XIB file failure so I decided to recall it and try it again. You have to override the didlayoutsubviews function called in the ViewController to make these changes. I took about 15 minutes to write a line of code to see if I was on the right track.

Yeah that didn't work either. I went back to Google. 

* "uibutton init programmatically swift"
* "uibutton init programmatically swift bounds"
* "uibutton programmatically border"
* "set border uibutton swift"

I was adding "Swift" to all the queries because I was getting answers from 2009 and 2010. This was a discouraging sign to me because it indicated to me that the problem facing me was not a common one. That in turns means that I got a whopper here. 

Hour 3 rolls around and I was getting desperate here. I only work on JumpShot 2 during my weekends - usually at the cost of hanging out with the few friends that I have (in their dedication I had them be my test data players). And out of the 5-6 hours I allocate for it, it was all being sunk into this ONE goddamn bug! I was getting NO further than where I was at last week. What the hell?!

* "parent uiview blocking subviews"
* "uiview clip subviews"
* "cliptobounds uibutton"
* "uibutton frame border"

What you see above are the final spasms of a brain that just really wants to go home. My back was killing me and I had a hangnail (which really does make a difference) and the coffee I drank a few hours earlier was making me feel empty. I closed the laptop and went home. 

####"BUG" Was Captured!

As with all things in my life, the next way to approach this problem came to me while I was in the shower that night blankly staring at the tiles and soaping up a Loofah with my hands. 

Continuing on the bounds thesis, I decided that I may go and manipulate something in the parent UIView, expanding it wider until it covered basically the entire view. This way there would be absolutely no chance that this could be still hidden right?

I opened up my laptop and then moved up to the part of the file where the UIView was being initialized in the code. That is when I saw it. 

    self.team1View = UILabel()
    self.team1View.frame = CGRectMake(0, yOrigin + self.scoreBarView.frame.size.height, self.view.frame.width / 2.0, visibleHeight / 3.0)
    self.team1View.backgroundColor = UIColor.lightGrayColor()
    self.view.addSubview(self.team1View)

Can you see it? The very first line, the one that says "self.team1view" was being *initialized as a UILabel* instead of what it is supposed to be - a UIView. The amazing thing is that I only saw it when I had to go and manipulate that exact line. It was literally invisible to me until that moment. I immediately changed the line (and its corresponding sibling self.team2View) and got the right result. Tapped fine and everything. 

<figure>
	<img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/completegamepad.png' style='width: 400px'>
	<figcaption>Too Tired To Even Care</figcaption>
</figure>

That, my friends, is why I work in marketing. 