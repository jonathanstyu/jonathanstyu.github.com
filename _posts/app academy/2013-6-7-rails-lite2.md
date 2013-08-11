---
layout: post
tagline: "App Academy W4D5"
tags : [app academy]
---


### Rails Lite Continued

Today we worked on recreating Rails. The idea is to build on the ActiveRecord work that we did last week and extend that, building out many of the features in Rails that many people would think is simply magic. Rails is a pretty awesome product and much of what it does is cool. However, as someone who would express that they are an expert at that, they cannot simply just attribute it to magic. A rails programmer should know a lot about Rails and how its internals work just like how a mechanic has to know about how a car works.

We started out by recreating the WEBrick server that will listen to and serve up the web pages that Rails receives. We created a server that receives HTTP requests and sends back web pages. We used several HTTPSERVER methods that then take the web requests that we send it and then pass those requests as well as a response to a Controller. We created a Base ControllerBase class that serves as the core foundation of all the Controllers that we used on Thurs and Wed. That was pretty awesome to see this empty shell of code suddenly turn into something that can behave somewhat like the actual thing. I was pretty impressed.

Then we worked on ERB templates. Using bindings - which is a Ruby thing that lets you pass in variables out of their scopes to other processes - we could render ERB templates. We point them to a particular URL and then have it render the page.

Then after that we implemented the Session part, which stores cookies and such. Back on Wed and Thurs we were able to save and set cookies for users with browsers with Session. Ned back then told us that that was a method. Today we actually went ahead and created that with the HTTPCookies class. We opened it up, found our relevant token, and updated the cookie with our data.