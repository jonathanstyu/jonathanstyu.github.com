---
layout: post
tagline: "App Academy W8D4"
tags : [app academy]
---


### Final Projects

Today I spent more time on building up more features of PerfectPerusals, with the significant feature being the central API that would serve as the data consumption for the other parts of the application. It would presumably be the heart of a proposed iPhone or Android app and if I had the time and energy for it then I would be able to go write that too.

The goal for today that I set yesterday was to build on the friending infrastructure that I set up yesterday in like ten minutes and create a full friend feed that would give someone access to a stream of events that a person's entire set of friends have performed in the past two or three weeks. I had thought deeply about this throughout my morning run and shower. When I started the system I had created a number of model objects that would be separated throughout the system. My concern was that there was no relations between any of the model objects and if I should go and create a friend feed out of it then I would have to 1) Somehow gather them all together under one unified umbrella and like the Ring of Power unite them and 2) somehow create a single front end that would be able to go and consume all that varying data. Now looking back on it, it might have made more sense to be able to go and create a single Rails model Object that all the others - taggings, friendings, reviews and comments - and such would inherit from: Event. This event would then be easily gathered and exported. Of course the grass is always greener on the other side. Who knows what other problems that I could have came across then.

The center point of everything that I was working on was the introduction of RABL, which is something that I had seen before scattered throughout the CareDox site but never really figured out. I was wrestling the alligator of as_json when I asked Paul for some help. He simply said that I should use RABL instead to export all that data. I decided that this would be a good idea and then for the next four hours spent them wrestling with RABL and trying to make it such that it would capably export all the data in a format that would be consumed by the front end Backbone that I set up on the dashboard home page.

The challenge with RABL is implementing all the logic that would be able to go and smooth out all the data that is getting thrown at it. I created some lmabda if statements that would be able to parse out the different data points. Thank Goodness this is Ruby and I could query the data structure for its class name. If this was on the Client side then it would all be called "Object" and then I would have to had to create something to work around that. Then after that the challenge was making it so that the data could be easily accessible and that the relevant data points are all available. For example, I wanted the API to be able to say that a Review was written by a certain writer. I had Rails export not only the Review object but also the associated writer "Reviewer". But then since the Reviewer on the actual Review object is just a number then I also modified the RABL to query and export the name. Then the client side would have very easy access to all the data that it needs to have. The RABL became all the unified data points that I need. With a good API, things became must easier.

Things I Need to Remember

After adding new searchable blocks, remember to run "rake sunspot:reindex" so that it will go through the database and reindex stuff. I spent like 35 minutes in the morning working on this as I fiddled with SunSpot throughout the entire part of the morning while munching on a bucket of fried chicken.
What I want to Work On Friday:

* Complete following (if possible)
* Friend messages
* Friend recommendations
* More stats?