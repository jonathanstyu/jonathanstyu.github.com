---
layout: post-no-feature
title: "A Dirty Swift Curriculum"
description: "For More Intermediate iOS Programmers"
tags: [coding, iOS]

---

Swift has been an amazing resource for people. It is a big improvement at least when it comes to readability over Objective-C. But for me the reality has been that my biggest challenge has not been tied to the language I am writing the app in but instead the various APIs that implement the assorted application behaviors people are familiar with on a day to day basis. Features and behaviors like swipe to delete, table views, and such. 

For the most part, you can connect the behavior you want with the desired API relatively easily - either just through a guess (Tables = UITableView? Who would have thought it?) or a clever Google search. Others are not so obvious. 

I have been trying to write apps for 3 years now and throughout that time I have come across pains in the asses in implementing such and such feature. Whenever I came across a good resource that solved that pain in the ass, I made sure to bookmark it - because it is likely that I am going to come across that pain in the ass again and I don't quite trust my memory to figure it out for me. Over time, my list of bookmarks have turned into an invaluable resource - a collection of great things that I can tap whenever I come across a problem I know I have solved before but don't quite remember how. 

So in this post, I have arranged my Swift curriculum list under a number of different desired features/processes/end goals. I might expand as time goes on (if that happens then I'll update the date on this post) but right now these are the main concepts that my bookmarks are organized around.

Note: This is all based on the assumption that you have some idea of Swift and also want to do all your interfaces programmatically - which is usually how I program.  

#### Overarching Navigation App Structure

[USING TAB BAR CONTROLLERS IN SWIFT](http://makeapppie.com/2014/09/09/swift-swift-using-tab-bar-controllers-in-swift/)

It could be hard to get started sometimes so this article gives you a spot to launch. Most apps are set up the same way: Tabs in tab bars that wrap individual UIViewcontrollers. This can be tough to set up programmatically so this is a way to start. 

[SWIFT SWIFT: PROGRAMMATIC NAVIGATION VIEW CONTROLLERS IN SWIFT](http://makeapppie.com/2014/09/15/swift-swift-programmatic-navigation-view-controllers-in-swift/)

UINavigationControllers help people dig into details without losing their place. They are a core portion of any app. This article helped me set them up for success. 

[SnapChat-like Swipe interface between ViewControllers](http://stackoverflow.com/questions/26033725/ios-how-do-i-swipe-between-views-using-swift)

Snapchat has a new way to navigate between ViewControllers that is not wrapped up in a tab. This is a theory on how to achieve it for your own app - if that's what you are going for. 

[iOS: How do I swipe between views using Swift](http://stackoverflow.com/questions/26033725/ios-how-do-i-swipe-between-views-using-swift)

This is the soft-core version of the above set up, where you are still in the same UIViewcontroller but want to swipe between specific views. A few apps have this and this is a nice solution for replicating that behavior. 

#### Managing Data (JSON, Serialization, Passing Data, Persistence, etc)

[Debunking the myths about parsing JSON in Swift](http://roadfiresoftware.com/2016/02/debunking-the-myths-about-parsing-json-in-swift/)

I personally use a library to help map data from JSON downloads to class objects, however if you want to do it without the help of a library then this was a great article for me. I like the part where it shows you a way to do it without using a bunch of if statements. 

[Realm Tutorial](https://www.raywenderlich.com/81615/introduction-to-realm)

Realm is a nice database that I came across during my Googling. I have not really reviewed or worked with anything else. They have a nice API that is smooth and easy to understand. There are also some things they say about speed and performance but I have not really gotten the chance to test that. This is a great first article to get you up to speed with Realm. 

[Use Settings with NSUserDefaults in iOS8 with Swift](http://www.ioscreator.com/tutorials/use-settings-nsuserdefaults-ios8-swift)

NSUserDefaults is a pain to deal with but you never really want to put User Default settings in your database right? This is a simple quick article to get you started. It helps me with things like, "Does this guy have the 2/3 point setting or the 1/2 point setting on?"

[Completion Handlers in Swift](https://thatthinginswift.com/completion-handlers/)

Is it embarrassing to say that I don't know how to implement Completion Handlers in Swift? I had an idea of how it works from JavaScript but how it is done in Swift I like a lot more. 

[Pass data when dismiss modal viewController in swift](http://stackoverflow.com/questions/25759945/pass-data-when-dismiss-modal-viewcontroller-in-swift)

This is a little more subtle problem. I did not have a real idea of what I was looking for other than the particular behavior that I wanted. I wanted this to automatically pass information and data (in this case, a photo) from a dismissed viewcontroller to its parent controller. This one came up on Google on the 5th row down and introduced me (and now, you) to the concept of protocols. I had seen them before when dealing with UITableViews but this teaches me how to create my own. Exactly what I needed. 

#### Understanding and Building With AsyncDisplayKit

It is one of the most popular libraries, a way to asynchronously generate user interfaces and display them. This allows for us to create data-rich applications without dropping frames and causing lag. It was tough to understand at first, but with the help of these articles, you can do it too. 

[Intro to ASDK](http://www.lukeparham.com/blog/2015/3/23/asyncdisplaykit)

Great visual and easy to understand language. A good Way to step into the library with your tippy toes. 

[NSLondon Talk](https://www.youtube.com/watch?v=-IPMNWqA638)

This video was the way I first understood ASDK. Sometimes you just need someone to explain it to you in person. The person who did this lecture is an amazing teacher. 

[AsyncDisplayKit Tutorial: Node Hierarchies](https://www.raywenderlich.com/107310/asyncdisplaykit-tutorial-node-hierarchies)

This takes you to the next step once you understand how ASDK works. This article helps you build your first UI elements and screens with AsyncDisplayKit. 

[Layout nodes using ASLayoutSpec #709](https://github.com/facebook/AsyncDisplayKit/issues/709)

This exchange can help you understand how to build interfaces using ASLayoutSpec, since ASDK does not support traditional AutoLayout. Once you get the hang of it, it is hard to go back.

[ASNetworkImageNode with unknown height don't update ASCellNode's height](https://github.com/facebook/AsyncDisplayKit/issues/718) 

Because of the way ASDK generates and renders and presents its images, sometimes images downloaded from the internet might not present themselves in the right way. This is because of its asynchronous nature. UI elements are rendered before the system knows the size of the downloaded image. As a result you have to do some refreshes and reloads. This code issue shows you how it is done. 

#### User Interface and Animations

[TableViewCell Swipe for Custom Actions Tutorial in iOS8 with Swift](http://www.ioscreator.com/tutorials/swipe-table-view-cell-custom-actions-tutorial-ios8-swift)

The swipe for actions action is famous for all apps. Seeing it on the standard iOS mail app, you cannot help but want to figure out how to do it for yourself. This is a page for learning how to do that. 

[How to Use iOS Charts API](http://www.appcoda.com/ios-charts-api-tutorial/)

iOS Charts is a great Github library plugin that handles a lot of the complexity of presenting data. Charts are a thing you do not want to write from scratch. There is a lot to deal with - tick marks, drawing the lines, and such. All of it is ridiculously difficult. This webpage helps you use the library. 

[Custom Fonts in Swift](https://grokswift.com/custom-fonts/)

I noticed that SnapChat brought in a new font recently, Avenir I think. Fonts matter, and the ones that iOS brings to the table can get a bit dry at times. This web page taught me to bring in my own fonts to an app. 

[Custom Transitions in Swift](http://shrikar.com/ios-8-custom-transitions-in-swift/)

The standard transition animation that you see when you push a new screen is very pretty but sometimes you just want something new. This webpage teaches you how to create and configure a new custom transition animation that looks slick, pretty, and really professional. 

[Harnessing the Power of Core Animation](http://merowing.info/2015/12/details-matter---harnessing-the-power-of-coreanimation/)

The title of this page is great. Details matter. Having a great little animation would help other people love your app. This one teaches you how they managed to get the little bounces and hops in UI (like a little pop when people touch the textview of an app), the things that people won't single out as being the thing that made them love it, but make them think, 'hmm, this is pretty nice'. 

#### Other Things 

[Definitive Guide to In-App Purchases in Swift, StoreKit](https://www.airpair.com/ios/posts/swift-storekit-in-app-purchases#5-how-to-configure-iap-in-apple-s-developer-portal-)

This is a little more advanced. I had read about a lot of libraries that help write the boilerplate code for setting up an in-app purchase selection but I wanted to write it myself. This helps me do it. 

[Getting to Know TextKit](https://www.objc.io/issues/5-ios7/getting-to-know-textkit/)

Most of what I do on my iOS apps is read so naturally the apps that I want to write have a big reading and text component. Kind of like Instapaper and such but I never came across something that helped with that. This is a great starter to help read up on the components behind those apps. 

[Introducing Stack Views](http://www.raywenderlich.com/114552/uistackview-tutorial-introducing-stack-views)

This is a new addition to the iOS canon. I have not used it in a very meaningful context (did use it for a practice app in setting up the interface for some other type thing) but from what I hear it is fundamentally a port of an Android development component. You add subunits, give them some broad guidance on what to do, and they sort themselves accordingly. 

#### Conclusion 

When I create an app, I have something in mind and try to create it in reality. At first I didn't really think about proper ways of writing code, just doing whatever I can to just make it happen. That's a bit slapdash but to me it is part of the learning process. You learn the fundamentals of creating an effect and then slowly work your way towards a more proper way to write code. 

Perhaps I should have just taken a course or read a textbook - that is kind of the way that I am taking on data analysis and machine learning right now - but at the heart of it, I don't really think I could have enjoyed it as much this way. Reading machine learning is fulfilling but more of a slog and I always have been a guy who wants to put his hands on things and start working. Doing it this way and building my curriculum this way has been way more goddamn fun. And in the end, that's what it's all about - fun.