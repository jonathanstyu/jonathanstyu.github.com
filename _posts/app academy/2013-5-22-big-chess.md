---
layout: post
tagline: "App Academy W2D3"
tags : [app academy]
---


### Big Chess 2

Today we continued to work on the Big Chess Project in our pairs. Even though I felt like we had made astounding progress through yesterday, we were working right until the end of the day. Yesterday we had implemented a number of classes that would subclass an umbrella Piece class. We would subclass as according to certain behaviors. The queen, rook and bishop would all just be variations of one particular piece. The knight and king would all have to have their own particular classes as well. We grouped the pawn off into its own particular class because it had a number of different behaviors like the 2 step first move and the diagonal capture.

Up until yesterday all of that had been done. We had even implemented an untested check and checkmate function that would iterate through a board, simulate a valid move, and then return whether or not the game had been over. Working out the bugs was incredibly painful and tedious. Eventually we came across 2 show stopping bugs. One was a huge one where the board would simply return that it could not call a particular software method on a nil. This meant that somewhere in the program, almost in the middle of an otherwise ordinary search, the program had broken itself. The logic had been snapped. We looked at this bug for over an hour, trying different things. Stepping through the program line by line with a gem called "debugger". In the end we could not really find the answer and we had to call Ned over. Ned figured it out within a few minutes. Turns out the fix was just 4 characters added to a single line. How about that?

The second show stopping bug is more tied to the design. After finishing the checkmate functionality, we went ahead and built some optional features like castling and pawn promotion. Pawn promotion took about an hour but castling was very different. Turns out that some of the methods that we call in castling would end up in an infinite loop. That"s fine. We wrote a workaround for that. But then what happened was that the fix then breaks the part of checkmate that flags checkmate if the king is in check and can escape. Ugh! We are still working on that bug. It is a major design flaw.

Top 3 Things I Learned:

1) If you are having trouble thinking up a name for a commit, it is likely that you took too long between them

2) 'git log' is helpful because it shows the branches of the different commits and where the branches all diverge

3) If you have a certain situation where the program is breaking at a particular spot, you can go and run "debugger if (situation)" in order to prevent yourself from having to hit "next" through an unending parade of commands

Other Things I Think I Think:

This SF weather is just ridiculously unpredictable. Wind chill can take an otherwise warm day to like freezing level

It is hard to maintain a high level of concentration through the creation of a very big program. Walking around helps