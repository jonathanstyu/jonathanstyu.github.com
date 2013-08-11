---
layout: post
tagline: "App Academy W5D1"
tags : [app academy]
---


### Rails Lite Continued

Today we continued our Rails lite project and spend extensive time building through some of the more essential parts of Rails that creates the "magic" that is so often attributed to the rails workflow including routes, params, and more.

The major challenge with params is that information is passed to us in a query string in a format that is often disarrayed and not so easy to pick through. Rails, not Ruby, picks through this information and formats it in a more neat and suitable way for processing. So that means that when we create our Rails lite, we have to build that out. Ruby took us half way there with parsing out keys and values into a nested array. However, we want that information to be presented to us in a nested hash so that all the information that are going to be the params for an object to be instantiated, created, or updated is in one place. This way we can just say params(;cat) and voila, all the cat information related shows up right there. This means regular expressions.

And now we have two problems.

Recursively using regular expressions, we are able to parse through a number of series of cat(name) and cat(age) name attributes and create from it a single hash that is headlined "cat" and then follows through with the "age" and "name" data. It was interesting to work with and extremely verbose. I spent a lot of time agonizing over my regexs.