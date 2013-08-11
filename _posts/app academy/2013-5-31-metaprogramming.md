---
layout: post
tagline: "App Academy W3D5"
tags : [app academy]
---


### Meta Programming?

On this fine day we spent our time working on metaprogramming, something that we were supposed to hit on Wednesday but they rearranged it in order to make it on Friday. Where on Tues, we worked with the most basic of basic of ORMs and then on Wed-Thurs we spent time working with ActiveRecord, which is something fully featured, today we were going to build our own little ActiveRecord Lite - I call it Rebuild of Activerecord 1.11 - using metaprogramming techniques like Send and Define Method.

The reading on metaprogramming was as usual tough. There was a lot of things that I did not get and stared at a long time before I was able to finally just say, Gosh whatever and move forward. Send and Define Method are the bread and butter of metaprogramming and while it took me a bit to get metaprogramming, define_method presented a much bigger challenge. It required me to understand that classes are objects themselves and that there exists class instances. I found that concept particularly challenging for me as I like to visualize things when I try to understand them. It is tough to see visually the concept of a piece of programming writing itself and instances of classes.

The challenge with creating AR 1.11 - You Can (Not) Recurse - is this twisty turny thing that makes people use the metaprogramming part of it. You start out with this object which is to be the most basic of objects. It is then extended into a SQL object, which writes its own SQL code so that it can interact with the database. It abstracts out all the SQL that the user would have otherwise had to use. I found it a rehash and good review of the work I had to do on Tuesday when we built our own generic ORMs. Then we went one step further and started to create the associations that are the hallmark of what makes the grown up AR so powerful. We created a has_many and a belongs_to association, which turns out to be just a method that invokes a define_method on a class instance of that object. It then writes the SQL code needed to make things like Class>>subordinate_objects = subordinate_objects possible.

4 Things I learned I learned

1) Classes are objects too. You can make subclasses of classes!

2) Define_Method

3) Send

4) A much better cementing of the concept of what Belongs_to and Has_many are, with belongs_to existing because it holds the foreign_key to the thing you are looking up

Other Things I Think I Think:

I thought the Heat were really done for last night but it seems like LBJ - the best player in the world - was able to drag them out again. Sigh. I like him personally but he wins too much.

To celebrate Friday, I am going to go eat IN N OUT today. Little things, my friends.