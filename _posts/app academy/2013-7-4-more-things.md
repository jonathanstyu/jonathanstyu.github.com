---
layout: post
tagline: "App Academy W8D1"
tags : [app academy]
---


### Creating Perfect Perusals (Not this blog)

For the first half of my two final projects, I decided to create a clone of GoodReads, which is a service that I greatly enjoy and like using. It is indeed quite of the most used products out there on the web. I use it more than Facebook.

The part that I felt was more like cheating was that GoodReads is not all that much of a single page app. It is in fact feels much more like something that almost entirely serves up its pages through Rails and has little if any JavaScript to do heavy client side stuff other than pop ups and scrolldowns. I wondered if a straightforward clone of GoodReads would be enough to challenge myself and create a product that would be something that be a great learning experience.

PerfectPerusals is something different. It is still heavy on the Rails stuff. It is not something ridiculous like Trello. This is in accordance to my interest and better comfort with Rails than Javascript. It is going to use Rails to serve up a lot of pages but from time to time I am going to use a dollop of Backbone in order to update and keep the server going.

The most challenging part of the entire project has been trying to figure out a right way to do these things that would make things easy for the future features that I want to build and implement in the future. I look at the finished GR product and I marvel at all these features. I end up thinking real hard about a simple easy way to get all of these into PerfectPerusals, like as if there is one single path that would just gracefully ice skate in between these Scyllas and Charybidses. I doubt that there is ever one such in the world. You can only look ahead ever so many steps.

I am not looking to implement every one of GR's features. In this demo I am looking to focus on a few:

* User profile pages
* Author profile pages
* Signup and Sign in
* Book profile pages
* Book shelves
* Reviews of Books
* Comments on Reviews of Books
* Friending (maybe)
I started the project on Sunday and so far I have made encouraging progress.

User and Author profile pages were implemented with a single model User. I created a number of different filters in the User Controller that would look at whether or not a user had published any books, provided an email, and as such would then render the right page. I spent a lot of time getting the logic right and making sure that at least the right pages would show up.

Signup and Signin were done without BCrypt, which I do think is a flaw but could be easily remedied. It had been some time since I had done something like this so I had forgotten a few things.

I had made it so that people could embed their book reviews right into the profile page. It would also show the rest of the community's reviews. This embedding of the book review form page was done with a relatively simple data-success helper from Rails. I felt like this would do a good job of simply posting the review to the database and appending it to all the other reviews listed in the page. However, I felt that this was a little cheating and would not give a lot of headroom for future expansion of the product going forward. For some of the other features I wanted to get more detailed.

For the comments under a review, I went ahead and created a full on Backbone app that had its own particular set of routes and views that activated only if you hit the Show Review page. It lets you add comments onto the page and does something kind of like what the AJAX call did with the Add Review page on the Show Book page however I feel that the Backbone implementation could handle a lot more meat on its bones if I wish it to have in the future. This is probably going to be case as I go and implement Friending and friend commenting.

Book shelves is challenging and I am working on this right now as we speak. We will talk more about it tomorrow as it is going to be the thing I hit head on!