---
layout: post
tagline: "App Academy W7D2"
tags : [app academy]
---


### Picture Tagger

Today we continued with our big JavaScript project PictureTagger, which emulates the functionality of Facebook in that someone can log in, add pictures and then tag a picture with their friends. Such a simple piece of functionality as it turns out is something extremely and deeply complex. It really is something to be amazed about.

Picture Tagger is our way to learn BackBone (which I keep confusing with BootStrap to turn into something called "BootBone" or "BackStrap") without actually using BackBone. We split out our JavaScript code into different sections that hold templates, "controller" code, and views. We also wrote and created objects that would hold different objects, features, and then functions that we can then call. We had at first written all the code in the application.js file that comes in Rails but it quickly grew into a monstrosity that could fight Godzilla in some cheesy Japanese movie ... like Rodan or Mothra or something. The key to understanding and getting past that was to realize that application.js is really one big compiled piece of JavaScript code. Once we have added things in the right order then we could quickly compartmentalize everything into neat and well understood piles.

We also had to very well understand callbacks and how they work. We wrote a number of callback functions that we could then call as certain information comes available from the server. The goal was not to get too confused by all these callbacks by being very definite about what exactly each particular function will do. It is kind of amazing when we go into our "model" class for photos and then see all these callbacks being passed back and forth. It looks like a hot potato race. There are callbacks after callbacks.

One of the bigger challenges that we had after figuring that out was to figure out how to create and position tags so that they float on top of the image. Dylan helped us a whole lot by coming up with a great solution. We would wrap the image with a div. The div would be placed in a "relative" position to the image, which is positioned "absolutely". Then we would create and render the individual tags with "absolute" positioning and after hardcoding the x and y coordinates into the page we could then just append them onto the main wrapper div.

There was also the challenge and going about and configuring the Rails backend too. It turned out to be quite essential to making the product work. There was a whole lot of stuff to be done. I find it hard to imagine that no matter how much Ryan says about just how separated the two halves - the front end and the back end - are from each other that how much they have to be integrated with each other. There were so many times that we would have to go back into Rails, switching between JavaScript and Ruby (so there were many Ruby statements with semi-colons on them), because we had to reconfigure the controller to make it all work smoothly between each other.

I am sure there are plenty of bugs left behind to figure and work with but I am glad that this core functionality was implemented: A true full stack application with interactivity on the front end and robustfulness on the back end. You can take a look at our completed - and hackish - Picture Tagger here:

https://github.com/jonathanstyu/picture-tagger

Other Things I Think I Think:

Jonathan's weather report continues. It rained lightly today and the rain moistened my legs. It was one of those random days when it rained and it was dark but the air still felt kind of warm.

Went to go see World War Z last Friday. Was pretty good but thought that we needed more scenes of zombies massing up and attacking people.