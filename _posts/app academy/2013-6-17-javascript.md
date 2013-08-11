---
layout: post
tagline: "App Academy W6D1"
tags : [app academy]
---


### JavaScript

Today was supposed to be a solo day where we spent time working on two projects. Instead, it turned into pair days where we were to work extensively on a number of projects including a crazysort solution in JavaScript.

The most challenging concept for me to grasp was that of callbacks. Callbacks are a part of Javascript and seemed to be a part of Node.JS. In Node there is a library called readlines. You would be able to invoke a method called "Question" that inquires someone of a query and then after receiving a response would go and invoke a "callback", which is another function that has been passed to this particular method that is kind of like a Proc in Ruby. It is different from what I am used to because while I have dealt with Procs in Ruby before, I have never really dealt with them to the extent that I am doing so in JavaScript. The difference is that with JavaScript, there are instances where a web developer does not want a program to completely take over a web browser or a page with a loop. So with these callbacks, someone can essentially "queue" up procs for the future.

The biggest project that I was to deal with was something called Crazysort, which is a Bubblesort implementation. It would be in JavaScript and would use a number of Callbacks. It would ask the person to sort out the numbers. I managed to finish it but it took more time than I wished. About an hour and a half.

Ned spent some time after to go over the solution and as expected it was quite more elegant than the one that I came up. The biggest challenge ironically as it turns out to be is that the program does not do what Ruby does: It does not stop the entire implementation of the program in order to wait for the author to give input. Dylan noted that this was a constant issue with a lot of client side JS programs. Always a bitch to debug too. The solution for me was to create three different functions that passed each other the same information. There is one that manages the entire loop so it kicks things off and tells everyone to stop. There is another that goes and manages the checking and swapping of an entire row in a single pass. It may or may not do any swapping. The last was to to actually inquire of the user. It would take a callback function and call on that "true" or "false" if the answer is smaller or larger.

Ned's solution was much more elegant. My program had simply went through and implemented a counter. The counter was reset whenever we made a swap. If we went through four instances (or whatever the length of the array was) without a swap then it means that the entire thing was done. He implemented a true or false boolean. If they made a swap within the scope of a single passthrough then he knows that he has to continue on.

Javascript is unfurling its secrets to me, but I have to say that it continues to challenge me.