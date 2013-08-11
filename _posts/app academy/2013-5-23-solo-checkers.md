---
layout: post
tagline: "App Academy W2D4"
tags : [app academy]
---


### Solo Checkers

Today was a solo project where we all spent time on our own creating a checkers program. We had been very fortunate in that Ned had given me a roadmap on where and what this program should be doing. While I was studying the Chess solution last night it occurred to me that some of the design choices that we made in the creation of that program had caused us more pain that was necessary. Namely, in the section where the program was determining the chess piece's possible locations, we had undertaken then to fix the direction of the chess piece through a number of mathematical formulae. The result was that we saved a few compute cycles but it kept us from reusing that function in determining the check function.

Checkers was different because Ned had given us some good instructions here. It was pretty straightforward. I simply went ahead and made Ruby methods that did what it was supposed to do. Though not before spending half an hour staring at the checkers diagram to see if there was some easy, mathematical way to simulate the entire thing. I did come up with one - but it only hit me several hours later when Checkers had been mostly built. The potential solution would have required me to build a "Rosetta Stone" of hashes that translated between the two like how RubyMotion bridges Obj-C and Ruby. It could have worked but alas I won't find out until I build it again - some day.

Checkers is a simpler game than chess. There are two types of pieces and even then the second piece - the king - is not really much more than a pawn with a true/false variable attached to it. I instructed the board to simply represent the piece with a "K" when the king variable is turned on. The major challenge is in representing the movement that particular piece takes when it traverses the board. I had never played checkers before in my entire life so I had to pay close attention to this. So a checkers piece can normally just move 1 slot diagonally. (Did you know that the entire game is played on the dark slots of the board? What a waste!) This is fine but there are situations where a checkers piece can hop over another piece of the opposing color - capturing it in the process. Then not only that, if the "hop" action can be repeated at the locale at which the piece landed then it can be repeated over and over again.

Ned had instructed to model the two actions differently and then for the chained sequence to check them in sequence for legitimacy. I wrote methods that gave us all the possible spots on the board that the checkers piece can move regardless of where it is on the board (so it can be off the board's bounds if necessary). Then we would winnow that down with a number of methods. One would check if it is in-bounds. Another would tell the user "NO" if the proposed move is a slide (1 spot diagonal) and hits a checker piece of the same color. Once that was done, then it was a small, fumbling step towards creating a function that can capably take in a lengthy array of proposed moves and then check the sequence for broken errors. One challenge was to figure out a way to safely contain all these errors and catch them. Sometimes you want an error message. Other times you want just a returned false or true.

The one big show stopping bug I came across was in my king promotion functionality. So in checkers if a pawn crosses the forbidden threshold and enters the realm of the senses at the other side of its board then it matures like a caterpillar into a butterfly to a king. The actual pawn promotion side was not that tough. I created a promotion function that is invoked at the end of every part of the play loop (right before we switch the players but after we make the move) that switches the pawn promo if it senses its location to be in the right place. However then I found that an earlier function that I wrote to artificially limit the pawn's possible moves to a simple forward march would not turn itself off! Whisky tango foxtrot?! An hour's of debugging later, I found that the issue laid with how the program was checking for valid moves - a necessary part of the normal make_move process. I had to duplicate the board to create a fake board that I would then play my proposed moves on. If any of them broke, then we knew that this proposal was not going to fly with the actual board and so I was free to yell at the player. However, the duplication of the board had gone wrong. I was passing through variables that would create dups of the Pawn and Board objects but I had not passed through that a pawn had turned into a king! So basically to the game, there was this pawn sitting there in its realm of the senses doing nothing! A single line change fixed the whole thing.

3 Things I Learned:

1) Saving a lot of time with creating fake tests in my ruby doc that run when I run the doc with the scenarios that I need to test. I could say that so much time in Chess. I hope this is going to be made much easier once we have rSpec.

2) Directional board stuff is less challenging that it seems. X and Y is a directional concept that I had trouble grasping at first but I am glad that checkers + chess had helped me familiarize myself with it. Games are a big part of programming and getting these right are important.

3) I learned how to play checkers

Things I Think I Think:

Jonathan's weather report: It was bright outside. In SoCal this means that you break out the cargo shorts. Unfortunately this is SF and so I ended up bringing the sweater. Ugh. I still cannot predict this weather because it is freakin' hot

I learned about Marion Tinsley, the best checkers player in the world. The only guy who could beat a computer one on one. How about that. The guy is the BEST ever at this game and I have never heard of him.

Marion beat the computer in 1996. At that time the computer was already the second best in the world. Holy crap this means that my iPhone 5 is literally the best checkers player in the world. No wonder so few people play that game anymore.