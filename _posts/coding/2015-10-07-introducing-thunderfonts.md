---
layout: post-no-feature
title: "Introducing ThunderFonts"
description: "Gifs, Fonts, Colors!"
tags: [coding, iOS]

---

Things have been just so busy lately with stuff at work that it has been hard to get a new post out. I apologize to my nonexistent readers. In this new post I would like to introduce yet another app: ThunderFonts. 

<figure>
    <img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/tf-icon.jpg' style='width: 400px'>
</figure>

It is right now submitted to the App Store and might not get approved, but I thought that it would be fun to write about again and reflect on the experience. 

The idea for ThunderFonts came from me stumbling across this little plugin on GitHub, [Koloda](https://github.com/Yalantis/Koloda), which gives you this out of the box Tinder swipe item. I looked at it under the hood and found it really well written. IT takes in a lot of cool visual effects like from Facebook Pop so to give that feeling of smoothness and transition. In order to write something like that myself it would have taken me a very long time. 

Whenever I start a new XCode project, I give it a funny name that is basically the reason to make this thing. In this case it is going to be TinderFonts. It is a way to swipe between a lot of different fonts on the iOS system and learn a bit about them. Simple concept, and I hoped that it would be Store-ready within a month. 

I had in my mind a very simple visual setup. A swipe interface and a rolling collection view right below it that would show the right-swipes. 

<figure>
    <img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/tf-page.jpg' style='width: 400px'>
</figure>

I was surprised just how quickly it took to get the basics. I had a working swipe interface within 2 days of creating the project and dumped in a few cards (with fonts on them) a few days later. 

I began to hit on problems though when implementing the less heralded features like the part where it saves a card that you have created to the internal store. The problem was that since I was generating the card randomly with random numbers I could not really easily just save the settings. There was no sanctioned iOS way to turn a UIView into a JPG or PNG other than to programmatically take a screenshot. This caused problems because the way I first conceived it, the screenshot would be taken as the card vanished off the swipe board. I had to rejigger the code so that it would take the screenshot the second that the card appeared. If the person swiped left then the image was discarded. 

<figure>
    <img src='https://raw.githubusercontent.com/jonathanstyu/jonathanstyu.github.com/master/images/tf-swipe.jpg' style='width: 400px'>
</figure>

I think the real fascinating thing about this project is just how much of the components I really enjoyed using. This is the first time I used a UICollectionView, which is the little image card that you see at the bottom of the screen. I was worried that it would be hard to make but it turns out to be super easy. I wanted to put in iADs page at the bottom. That turned out to be super easy too. Apple's API for that turned out to be as simple as pie. Just initialize the thing and drop it in. 

#### Other Thoughts 

* The problem with not building anything yourself is that you are not sure what is going on within the component. I built almost everything with HakkerJobs. I did not do the same with this one. I pulled together all sorts of plugins and while it certainly did a whole lot to get the project up faster than ever, it did make the whole thing feel a bit cheaper than if I were to make it myself. 

* Autolayout is a real pain sometimes. I generate all the components on my screen programmatically, which lets me avoid Interface Builder. This is my preference. However, in order to adapt to all sorts of different device sizes, I create autlayouts programmatically - most often using SnapKit to build the preferences. The problem is that because all of these layouts are created in relation to each other, it can be sometimes that a subview does not layout properly because its superview is still being generated. I had to spend a lot of time reading the Apple API to learn when specifically to place the autolayout code so that when the relative restrictions come into effect the superview exists and has the proper sizes. Viewdidload is too early, viewdidappear is too late, gah! 


#### Conclusion

I enjoyed making ThunderFonts but I don't think there was a lot to do with it once I had the basic thing going. I had imagined that I would do these dazzling animations and video gifs and all but I lost interest in that very quickly. I was ready to do something else. That being said, a lot of the code that you see here is going to get recycled into my next project - which I am still throwing myself at right now. Stay tuned. 