---
layout: post
tagline: "App Academy W3D2"
tags : [app academy]
---


### ActiveRecord Foundations

Hello again to my thousands of incredible loving fans! Hah. That was a real riot of a joke. Who the heck reads this?!

Today's work focused on creating some of the foundations of what would turn out to be ActiveRecord. If yesterday we spent a lot of time executing SQL inquiries and familiarizing ourselves with the SQL language, today we dove back into Ruby so that we could start applying what we have learned to our Ruby products. We decided to create methods and class instances to a SQLITE3 (which is a opensource database) database so that we can execute and run commands on them. The goal is to turn an otherwise discombobulated angry fruit salad of bits and boops into a coherent Ruby class. It helped me understand a lot about how a product like ActiveRecord in Rails would work. Tomorrow we go and work with the real kahuna but doing it this way helped a lot in getting a better grasp of what goes on underneath the covers.

Top 3 Things I learned:

1) Subqueries in SQL language are powerful. We had a SQLzoo problem where we were to use a number of JOINS to locate a route between two train stations. If one were to simply "extend" from the starting station, then we would have to write and JOIN the same table to itself up to 4 times. This is unpleasant. What I ended up doing was to write 2 subqueries that represent the two legs of the trip. One would start at the start and one ends at the end. Then I joined the two subqueries together at the place where their stops would be the same.

2) Learned how AR is supposed to work. The way that I had understood ActiveRecord to work is that it would literally save and serialize the entire object into the nebulous memory. Turns out this is not the case. The initialize function that creates the object is actually invoked and fed the database's vomit of random bits and boots. It then creates an object with all the associated methods that we can use. This explains the existence of a "save" method. We ended up writing this method today in class.

3) The bottleneck in many apps is how often it hits the server for data. We had a method in where our program would get some initializing information from our SQLITE3 database and then hit that database again in order to convert it through a findbyID method. The way that we saw it, it would have the advantage of helping us keep all the methods where we create a particular object with that particular object's file. We are not creating the "user" class in the "dictionary" class for example. However, our mentor told us that this would probably not be efficient in the long run. It is good to keep the code clean, but hitting the server so many times would slow us down. So even if the alternative is to have this ugly piece of code disorganized in our classes, it would probably be for the better. Surprising to me.

Other Things I think I think:

The weather in SF continues to volatilize. Today it was freaking hot right after it poured watery rain on my head yesterday. I checked the weather app and it seems to imply that the next few days will also be hot. I will make appropriate adjustments. Get ready for JY in cargo shorts!

I did not eat well in breakfast before I headed out today, microwaving a hot dog. This had dire consequences for me later on in the day. I feel that I should spend more time eating something less processed. Little steps. How about some potato chips?

The stock market opened up quite high today, tapered off in the afternoon but still ended up. My portfolio has been leaning more and more conservative. I sold off a number of individual ideas and added to my longer-term index fund and high dividend positions. Based on a number of risk appetite indicators around the world - including the bloodbath in Japan - I am unconfident about the next coming months.