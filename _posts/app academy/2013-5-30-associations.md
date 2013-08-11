---
layout: post
tagline: "App Academy W3D4"
tags : [app academy]
---


### Exploration of Rails

Today we continued with our exploration of Rails. Today we spent time on stuff like associations like "has_many" and belongs_to".

If yesterday's project was relatively easy, then this one was a little more challenging. The goal this time was to build a demo of a Polling product. This product would create polls underneath a user. A user would not be able to fill out his own poll and then later one we would be spending time creating teams of users that would restrict users to custom polls.

We also spent some time talking about the difficulty of the n+1 problem. The key to understanding this is rooted in the way ActiveRecord works. Every time you would call for an instance object's associated objects, AR then needs to create a SQL request for those objects. If that object has its own subordinate objects then it would need to formulate and create a SQL request for taht thing in turn. The problem is that if you are referring to these things in a loop, it is possible that the database would be getting a whole lot of objects unnecessarily. The way to get around it is to go and pre-load those objects into Ruby so that you can access them as local objects. The method that you would call is Includes.

The project is pretty cool but I really felt that over time it got so complex that I ended up quite confused. I was not sure if I was on the right path when I started and when I started traversing all of these associations up and down the chain and it hit the database again and again, I became quite insecure in the way that my system worked. Going forward, I am going to have to spend more time familiarizing myself with how software should be developed.