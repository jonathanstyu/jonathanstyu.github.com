---
layout: post
tagline: "App Academy W5D3"
tags : [app academy]
---


### Reddit Clone

Today we worked on creating a clone of Reddit. I found this very interesting because I never was a big Reddit user. I barely know how the damn thing works and now we have to build something like it?! That is just crazy talk, right? Well, here at AA it is all about learning on the fly and making stuff up as you go along. So I went right along making stuff along despite a sore throat and recovering from a pretty nasty bout of food poisoning last night.

One of the challenges of building Saiddit - as I call it - is that there are many different rings of items surrounding the models. There are subjects/categories and then below that there are links which then have comments on them. These comments go on ad infinitem as people can comment on comments forever and ever. So the models had to be flexible and powerful enough that we could be able to go about and accommodate many of those features in addition to being able to up or downvote things too. This was a solo project so it was just me, myself and I. The goal is to finish as much as possible.

We had to create the standard authorization and user login system that we are by now very well familiar with. The twist today is that I decided to build much of the infrastructure around the first Users model/views/controllers with a scaffold. By running "rails generate scaffold \[attributes:type\]" I found that I was able to have Rails set up a lot of code that would have taken me time to rewrite. I managed to get the entire login system up within 10 min, something that would have taken me a much time longer. Combine that with improved knowledge of BCrypt and I think I will be much ready for the re-take assessment on Friday.

Subs are rather simple. They held within them a single database column called categories. This would be the overarching categories. However underneath them the models were more complex. A submitter creates links and comments so I had to create associations between the three. We are always able to track back a comment/link to its author.

The project asked for several nested forms, where we would create a parent and child item simultaneously. We also needed such a form for 'editing' and 'creating' items. I created a partial that would be able to handle both. Ned then showed us how to go and make it so that if someone submitted an invalid child or parent item the form would then re-render with their information already filled in from the previous attempt. This way, someone would not have to retype in their information in case they had perhaps inadvertently hit the ENTER button. This is pretty cool. We would create a local instance of the object being creating and pass it through into the Rails form partial - which would then render as according to whether or not the object has been already persisted to the data store or not - and the form would simply fill in the values with the values. In order to model the up and down votes, I created a join table that would simply link a user and a link that he is up or downvoting.

The big show stopping bug was when I was trying to hone down a form that gave users the options to create up to 5 child items alongside the parent item. If someone had just filled in one of the five, then Rails should simply go and ignore all those blank fields. However, it was turning out that Rails was seemingly looking to take in those records and then running afoul of a data validation that I had written within my ActiveModel record. So what the hell? Rails should be ignoring these things because I had gone ahead and written a specific method that connects to the association - called "reject_if" - that would reject any of the 5 child entry forms that would be empty. I struggled for 20 min on this before calling over Peter to help me. As it turns out, I had created a custom method that would do this checking - the one that connected to :reject_if - but the name was preempted by a standard method in Rails. So it was ignoring my customer instructions and waving through this invalid data model that should never have been waved through. I was glad to see that simply changing the name of my custom method made the whole thing work almost like magic.

The whole project was a comprehensive test of my skills and I had thoroughly enjoyed creating it, though some of the other work turned out to be a real slog. I was just too OCD to ignore things like adding links and all kinds of other things like the right spacing. To think that such a complex product - that has things like user authenticiation, upvoting, downvoting, categories, links to URLS, AND infinitely-nesting commenting - like this can be built within a few hours is just unimaginable without Rails. You can find the project repo here at GitHub: https://github.com/jonathanstyu/saiddit

Other Things I Think I Think:

* I enjoyed watching the Spurs dismantling of the Heat last night. But I don't think we can expect all those 3's to come raining down again. That being said, the team that wins Game 3 wins the series 92% of the time.

* The weather baffles me yet again. It freezes me inside here but whenever I go outside it is warm enough to make me uncomfortable. How can I find the Golidlocks temperature?!?!

* Woke up too early and had to get coffee in the morning. Sigh. Maybe I need to start brewing my own.