---
layout: post
tagline: "App Academy W9D1"
tags : [app academy]
---


### Final Projects

In today's chronicle, Jonathan spends time cleaning up and tightening the screws on PerfectPerusals. I am pretty satisified with much of what was done but I still think that there are a few flaws here and there that would require some research to competently address.

The main issue regards how I want to implement the Friend Feed. The way I am doing it right now is to have a number of disparate objects which the back end collects and pukes out at the client. I then wrote client side code that would let the browser interpret all that and put it together in a simple organized way. To me this seemed like a pretty hackish way but for just displaying the friend feed it worked. Can't really complain.

However, then came the real issue. I realized relatively later that people can comment not only on Reviews but also shelvings of books and adding of friends. What the heck? I have never seen this sort of functionality before. Does this make sense at all? Well it is there so I have to implement it. Now the way I could probably do it is just to create separate "comment" objects. That would be a dangerous way to do it and I was not excited about that. The other way is to use inheritance, namely multi table inheritance, which is something that is going to be pretty cool. The parent class would have its own table but not only that there would be children and those children have their own tables. It is awesome, but it is not standard and though it is too late to use it for PP, I would think that it would be nice to try it for my next project.