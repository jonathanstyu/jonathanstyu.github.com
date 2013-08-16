---
layout: post-no-feature
title: "Making SEC Research with RubyMotion"
description: "Writing an iPad App with Rubymotion. My experience."
tags: [ruby, apps, coding projects]

---

<figure>
	<img src='https://raw.github.com/jonathanstyu/resume/master/images/secresearch/secresearch.png' style='width: 200px'>
	<figcaption>SEC Research</figcaption>
</figure>	

### Background 

For a long time I had the idea of creating an app that would help me read filings from the SEC database. If you are not quite sure what I mean by that, every publicly traded company has to send filings every so often to the SEC. There are quarterly filings and there are annual filings. Each of these filings have information about the company's business and its financial performance. The SEC collects these and then posts them for the public to see. Anybody can go and read them. They are public information. 

There is a lot of bitching and moaning from organizations about how much it costs to hire accountants and have them audit a company's balance sheet and financials. However this is one of the most valuable services that was ever provided to the US economy by the government. Back before the SEC was created, all information belonged to the companies themselves and the banks who lent to them even if they were public. It kept people from knowing about the companies that they were investing in. The result was that people invested in crappy companies and they all lost their shirts during the Great Depression and the Market Crash of 1929.  

<figure>
	<img src='http://upload.wikimedia.org/wikipedia/commons/a/a9/Depression-stock-market-crash-1929.jpg' style='width: 200px'>
	<figcaption>Courtesy of Wikipedia</figcaption>
</figure>	

And this is just my own opinion but some of these financials are actually fun to read. They are pretty interesting and it is chockful of information about the company's industry, its financials, and more.

### The Problem 

[The SEC's website](http://www.sec.gov/edgar/searchedgar/webusers.htm) is not easy to navigate and it was not fun to parse through the website and get information on the filings available. It is not mobile optimized which means that I would be best served by going to the website on the desktop, download the PDFs and HTMLs, and then put them on DropBox for reading later. The task did not excite me so I wanted to get an iPad app for it. I love to read on my iPad and an app would help a lot, right?

The problem was that there was just 1 app available on the Store and it was not cheap at all. It also had a whole bunch of different little features that I was not sure that I wanted to use. Most of them are social and highlighting features that were pretty useful but I would have hardly noticed if they disappeared. For this set of features, I had to pay a pretty penny. Good for them, because it seems like that they have a good thing going, but it thus seemed to me that you could make something that does a whole lot less and then sell it for a whole lot less. 

Thus came my idea for SEC Research, the app. 

### RubyMotion

I have tried to code stuff in Objective C before, trying a bunch of different tutorials, reading all kinds of books before finally collapsing into a steaming heap of whatever. I was totally confused about how to even go about creating a new app. It just seemed like there was no real easy to start guide. 

Then I read [a great blog post](http://www.alasdairmonk.com/journal/from-idea-to-appstore-in-2-weeks/) by some dude about how he made an iPhone app with Rubymotion. This blog post got me thinking again. I went ahead and bought Rubymotion and then threw myself into the task of making some apps. Two weeks after I first bought the product, I had created JumpShot, which was the first app that I ever made. I was extremely proud of it. It had a cool interface that let you swipe to assign players to teams and cute colored buttons that you could tap alternatively in order to let you assign stats to players. I enjoyed the work I did making this particular app. 

SEC Research would be an extension of what I did in JumpShot. It would be the first app that I made that would require me to actually connect to the internet. It also had to be something simpler and better to use. It also would have to have a good reading experience. It is not something that you can just open and close whenever. 

A lot of people ask me about my experience with RubyMotion. I can give those later on this post. For now, you will have to be satisfied with my chronicling of how I created SEC Research. 

### Making SEC Research - Starting Off

The core functionality, that which I was most concerned with, was that of the actual summoning of the SEC query, putting it on the screen, searching, and saving. That was what I wanted. JumpShot had a lot less functionality but when I was making that particular app I was concentrating more on the look and feel of how it worked. In this case, I just wanted SEC Research to be able to do what I wanted it to do.

Objective C has a bunch of gem-like libraries called Cocoapods. RubyMotion is cool in that you can use them inside your project with the use of the RubyMotion gem [motion-cocoapods](https://github.com/HipByte/motion-cocoapods). I had used a number of different Cocoapods while making JumpShot and wanted to bring them into the loop this time for a second go around: 

* [SVPullToRefresh](https://github.com/samvermette/SVPullToRefresh). This is the first CocoaPod that I was ever able to get working and my struggles with it taught me a whole lot about how to handle them in general. When I first started using this, I was not quite sure how to use it. I was in fact very confused. There was not a lot of documentation about how it was going to work. It basically just said, "You can use this too." I ended up Googling through the world and finding [this code snippet on GitHub](https://github.com/hendrikswan/motioncasts-pull-to-refresh). I copied what this guy did very closely. It still did not work. Then after a lot of fumbling around, I found out that if I was to add any new sort of CocoaPod to my project (it is done in the RakeFile), then I would have to go and delete the entire Pod folder under the Vendor directory in my project and then run rake again so that it could redownload the entire set. Weird behavior, but *consistently weird*. Consistently weird is always something that you want and can handle when dealing with computers. 

* [NanoStoreInMotion](https://github.com/siuying/NanoStoreInMotion) See below with the section on Motion Model to get a sense of why this was eventually helpful. NanoStore is an Objective C data store that is non relational. NanoStoreInMotion helps me use it in my RubyMotion project. I cannot set up things like associations and such or callbacks, which is a pain, but its serialization and data storage is rock solid. This was enough for me to use it over Motion Model. 

* [AKTabBarController](https://github.com/alikaragoz/AKTabBarController). Drop in easy to use. It basically invalidates several hours of me fussling with a number of different photo imaging programs to make the image SVG or PNG look good for the tab bar at the bottom of the app. Before I used to have to be like, "Oh this is 48x48 pixels so let us go and change that ..." Hilarity ensues as it does not work as planned. AKTabBarController just takes any regular PNG, shrinks its down to size and then applies a sort of monochrome filter to the image so that you can get a nice sheen to it. It looks professional and it really is the easiest thing ever to use. 

The first thing for me to do was to start off with a bunch of different gems and such so that I could get a sense of what I could just plug in and what I might need to start from scratch on. I quickly found a collection of cool gems: 

* [HppleMotion](https://github.com/siuying/hpple-motion) This is a gem that seeks to be the Nokogiri of the Rubymotion world. It has nowhere near the functionality that Nokogiri has though. It basically takes in a string of HTML/XML and parses it in a certain way. I used it a lot when I was parsing XML and HTML that returned from the SEC's queries and web pages. I wish it had more functionality, similar that of Nokogiri, though. 

* [GeoMotion](https://github.com/clayallsopp/geomotion) Geomotion is great. It simplifies the way that you can position and move around certain objects. You can place UILabels in relation to one another in a simple and easy way. You can also create frames for objects in a very easy way: CGRect.make(x: ..., y: ...., height: ..., width: ...). This is pretty awesome. 

### Stuff That Did Not Work 

Okay, so all the good stuff out of the way. Let us get to the real work. The way that I code and make stuff is pretty simple: Find out what does not work and then do not do that. Unfortunately I do not have enough brains and knowledge to do it the Tesla way. I call it the Edison style. Just make the nonworking light bulb 999 times and eventually you'll even screw up the task of doing it wrong. Backing ass-front forwards into the right way. While I was making SEC Research, I tried a whole lot of things and found that they did not work. They only contributed to my knowledge of RubyMotion and my eventual loss of hair. 

* [Motion-Model](https://github.com/sxross/MotionModel) Motion-Model is something that comes up a lot. Basically it gives you a bunch of methods that lets you call ActiveRecord-like methods on a RubyMotion model object. This is pretty cool. You call a "Filing.all" method and then you can go and find all the Filings that are in the data store. I loved using Motion Model for the most part. The main problem came about when I started needing to close and open the app.

**Motion Model**

The problem with MotionModel is in the serialization. The instructions to using the serialization part of Motion Model are pretty simple: 

*@filing = File.serialize_to_file('filings.dat')*

The way that I understood it, calling this class method for any sort of MotionModel object would cause the application to serialize every instance of the object into the phone's storage. Not the most ideal way to do it, but what can you do? When creating the favoriting functionality, I decided that it might make the most sense to create a Favorite filing class that would be created when someone touched the "Favorite" button. Then when someone closes the app, I would automatically have it call the serialize\_to\_file method through the *applicationWillResignActive* method in the app\_delegate file. When the app was opened then I would have it call the *deserialize\_from\_file* method

The problem began when I started playing around with that functionality. Somehow the functionality had changed since the last version. Serializing no longer meant that it would simply "replace" all current objects on the data file with brand new ones, it meant adding to them. So if I had 3 favorites on the disk and then opened and closed the app again, then there were 6. And so on. This would eventually crash the app, much to my amusement. I never figured out how it worked even when I went into the gem's code, so I decided that enough was enough and went back to NanoStore, which was more stable and left me without a lot of the cool ActiveRecord-like methods but did the more important serialization thing just fine. 

* [SECQuery Gem](https://github.com/tyrauber/sec_query) This is a gem that is in Ruby that does a lot of the hard work for you. You can search the SEC for specific entities, companies, and people and then read the filings associated with them. It sounds fantastic! Combined with the below gem, this would be perfect right? But well ... let us see ... 

* [Motion-Bundler](https://github.com/archan937/motion-bundler) This is a RubyMotion gem that seeks to solve a very real problem. There is a huge library of Ruby Gems out there in the world. However, not all of them can be used in RubyMotion for whatever reason that it might be. There are a few gems that cannot work because they depend on other that do not work in RubyMotion. The best gems for RubyMotion are the gems that have no such dependencies. Very few gems are like this. Motion-Bundler attempts to make it so that you can use non-Rubymotion gems in RubyMotion. It sort of tries to mock up a bunch of the dependencies so that they are usable. 

**Motion Bundler and SEC Query**

The problem with Motion Bundler is that it was very limited in what it could do. I wanted to use the SEC Query gem in my RubyMotion project. However, the problems began. The project would not compile correctly. It would basically crash with an unspecified error that is not helpful at all. Something about a weird character in the code. 

This is unfortunate because it would have been awesome had I been able to actually just plug SEC\_query into the app. This did not turn out to be the case however. I eventually decided that it might make sense for me to pull some of the gem's code into my own app. However, it seemed like this program used Hpricot, something that would not have been able to work on RubyMotion. I ended up tearing out whatever I could find that might have worked including URLs and such that suited my purposes. I feel sad that I was not able to use such a beautiful product, but that is the trade off that you make so that you can use Ruby to write iPhone apps.

### Thoughts on RubyMotion

I like using it. People ask me occasionally about what it is like coding in RubyMotion. Well, it is not like I am an ultra experienced coder. One thing that I can be sure of is that I was never able to create something in Xcode and Objective-C. The farthest I was able to get was to tap out stuff in a tutorial. I was much more familiar with Ruby and RubyMotion helped me a whole lot.

One thing I did felt after using it is an emptiness. I felt like even though I had been able to do so much that there was still a whole lot lacking. The only problem is that I wonder if this is because I am simply not familiar with how the thing works or I lack design assets or anything else. It just makes me wonder. 

In a later post, I will write more on specific things that hung me up. 