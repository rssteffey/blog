---
layout: post
title: Appetite For Frustration
tags: design games logiraffe
comments: true
published: true
description: In which Shawn reacts to an old design decision
---

Uh oh.  

Welcome to the first in a series of ...*older*... posts. \\
This post was originally written in January of 2018, but due to technical issues and other changes - it remained unpublished.
The concepts in it still stand, though, so I figured it would be good to get out there anyways - especially now 4 years later when we've just gone through a resurgence in Wordabeasts interest.

Stick around to the end, I promise I'll pop back in to shed some relevance.


### It's All Semantics

Today I’m going to briefly invite you behind the scenes of a problem solving expedition.  Unfortunately that will require a tiny bit of understanding of how our game<span class="ref"><span class="refnum">[1]</span><span class="refbody">Wordabeasts - Have you missed that?</span> functions.  So let's start with a brief elevator pitch on what [Wordabeasts](http://www.logiraffe.com/wordabeasts) is:

![](https://i.imgur.com/QsG3ZDj.png "Before we get started, does anyone want to get out? (a pen?  To take notes on this incredible pitch?)")

*\*ahem\**
{: style="font-size: 1.2em"}

Wordabeasts is a multiplayer party game in which all players use their smartphones or tablets to submit their answers.  The gameplay itself is similar to that of CatchPhrase or Codenames, in that players attempt to think of a clue to help the other players guess two words out of a lineup.  If, for example, the game secretly gives me the words “hat” and “coat”, I might submit the word “clothing” as my clue.  Then when 5-7 misc. words pop up on the main screen, hopefully my friends and family all select “hat” and “coat” from the collection, while avoiding the decoy words of “shark”, “microphone”, etc. (which have naught to do with clothing)

These words are entirely randomly drawn out of a deck of 350 words with zero calculations done on how they will impact the game.  The next word in the randomly shuffled pile is the next word that gets dealt out to a player.  Absolutely 100% random.

Of our beta testing within a group of \~30 people, this has worked well enough.  Initial acceptance of the game is incredibly slow, but so far everyone who has stuck around past the first game or two has realized they are enjoying it, and end up playing much much more.
Unfortunately, that is not a curve that actually works in marketing games from an unknown entity.  First impressions matter.

![](https://i.imgur.com/s0N6c3A.png "After the third or fourth iteration of the same lines, Eric starts to grow on ya")

We've had fun identifying what exactly is causing the initial frustration.  Part is almost certainly due to the lack of any easily understood tutorial.  Despite my best efforts, the 30 second instruction set at the beginning of new games and the optional full instruction book on the main menu hasn’t seemed to  alleviate too much of the initial confusion.

However, it’s not only that lack of direction impeding impressions.  Due to the nature of the purely randomized word deck, it is very easy for the game to create seemingly impossible scenarios or screw players over at the last second by throwing in a very similar fake word, causing them to miss out on an otherwise perfect clue.  Ideally, a game should feel challenging - but never frustrating.  Frustration is born of a negative experience, and although it only happens in probably 10% of rounds, that is still 1/10 of first-time impressions  that have been tainted. Anything we can do to alleviate that potential possibility would be great.<span class="ref"><span class="refnum">[2]</span><span class="refbody">Shawn from the future here - keep this particular point in mind so later we can laugh at how dumb Past Shawn was</span>


We’re going to be primarily concerned about two concepts of linguistic measurement here, _semantic relatedness_ and _semantic similarity_.

- Semantic similarity is a quantification of how (you guessed it) _similar_ two words are.  This can be thought of as the thesaurus test. For example the word “pasta” and the word “soup”, while clearly very different in a culinary sense are actually scored very high in terms of semantic similarity.  Both are used in the same general contexts, and are in the same category of Food.
<br/>
<br/>
- Semantic relatedness deals with how often and how closely words are used in context with each other.  Relatedness captures a broader contextual sense than pure similarity.  “Bowl” shares a high degree of semantic relatedness with both “pasta” and “soup”, as it’s often found in the same context.<span class="ref"><span class="refnum">[3]</span><span class="refbody">It’s this same proximity measurement that allows us to calculate the aforementioned similarity of the two foods.  If many analyzed texts use the phrase “pasta in a bowl” or “soup in a bowl”, then we can begin to see those words are contextually replaceable</span>  

![](https://i.imgur.com/aL5q3zZ.png "Bowl-ing, less semantically related")

Well from an abstract point of view, that’s all well and dandy to know.  But how do we measure these things?  Surely there are millions/billions of potential data points to be analyzed here, even for a list of only 250 words.  What are we going to do?

Well as established in a previous post, I’ve got a bit of a history with using basic neural nets.  If we create a light neural-network to analyze a selection of Google Books tex—

![](https://i.imgur.com/Zjg7Hzh.png "Don't you go trying to ask the alt text for a different answer. Still No.")

Okay fine.  I’m admittedly too guilty of jumping straight into the neural network pool as soon as I’m given a problem in an unfamiliar space.  This is roughly the equivalent of me buying an airplane ticket from Nashville to Knoxville because I can’t be bothered to look up the roads to drive. <span class="ref"><span class="refnum">[4]</span><span class="refbody">I-40.  It's just I-40</span>

Let's do it for real then.
<br/> <br/>

### The Problem Space

To start solving this, it’s easiest to break the root issue down into the two problem possibilities:

1. The player receives two words with absolutely no discernible connection/clue word <br/> <br/>
2. The player submits a clue that they believe is spot-on, but then is totally blindsided when the decoy words appear and also perfectly align with the same clue
{: style="font-weight: 500; font-size: 1.1em; margin-left: 1em; color: #222222"}


Assuming we already have the two scores of similarity and relatedness from above for any given pair of words, this should be easy to weight!  Only give the player two words with a high score between them, and only add decoy words that have low scores.  Easy peasy. Both problems solved with one stone.

But instantly we’ve broken the number one reason for the game being fun.
The hail mary moments; the moments when a player receives two words that have no logical measurement of similarity or relatedness, and yet have the perfect connection to bring down the house.  This is playing “Helen Keller” for “Touchy Feely” in _Apples to Apples_.  The wordplay that no computer is going to be able to quantify.<span class="ref"><span class="refnum">[5]</span><span class="refbody">Yet.</span> \\
While squirrel and psychiatrist have a relatively low score of relatedness, a player receiving them could easily submit the clue “nut”.  Semantically, this is a very tough clue to predict, but it’s straightforward for a person to see the connection.

Removing these moments deflates Wordabeasts' main appeal.  Without a doubt the most powerful rushes in the game come from these shots of innovation, not mundanely typing “food” to earn points for the two food items you received.  As such, issue **#1** - while occasionally frustrating - simply cannot be removed without risking our cornerstone.

*“Okay fine, let’s solve issue **#2**!”* \\
I hear you declaring confidently. \\
*"Purposefully unsimilar decoy words!"*

I did end up implementing this one. For a small span of two weeks. It sounded like a great idea on the surface, but in practice…  \\
First, there’s no practical way to compare the player’s entered clue word with the answers. The only reason we could run similarity and relatedness measurements on our game's word combos is because they’re a predetermined static list and can be processed at compile time before we ship.  The [NLTK](https://www.nltk.org/) is both too bulky to bloat our game’s footprint and too computationally expensive to fit in the time between rounds. \\
Player input just isn't feasibly analyzed and compared in that time span.

So the only possibility left was just ensuring that the decoy words aren’t too similar to the correct pair of game-generated words. \\
I did this.  The game became _entirely_ too easy, and with "easy" came "boring". Even with what I thought was a relatively high threshold for similarity, the words were just too blatantly different. \\
It turns out the forced specificity, the process of having to find the *perfect* clue word to avoid getting shafted by random chance is just as crucial to the game flow. 

![](https://i.imgur.com/vQAgdvx.png "Starting to think I could take over for the Keanes on Family Circus")

### Results

“Shawn, why did you bother writing any of that down here if you’re just going to say you can’t do it?  What a waste of my time”

Well, dear reader:  Because this is a behind the scenes post, and this is what “behind the scenes” has looked like for a majority of these last 8 months.  Tackle a problem head on, realize my solution was actually worse, evaluate why, realize I had the problem space wrong, repeat.

In this specific case, the problem wasn’t that the game is too frustrating, it’s that it’s too frustrating _in the beginning_.  It’s totally fine for players on their second, fourth, tenth matches to get difficult word combos.  That’s an enormous part of what keeps the game actively challenging.  But players who barely understand why there’s suddenly an input box with bullet points on their phone screen are a lot more likely to intuit the meaning of the words “car” and “plane” than “squirrel” and “psychologist”.

So, I’m currently left with two options.  Utilize a full list of noun pair weights and rankings to alter the “randomly” dealt words for just the very first time someone plays the game and never use those weights ever again. \\
*or…* fake it. \\
Predetermine 10-20 pairs of words that are easily connectable, and have those set as the opening rounds the first time the game is played after being installed.

I know which one of those is easier and makes the most economical sense, but it still feels a bit like cheating.

<p style="font-size: 0.7em">Fun unintended consequence of all of this: while creating semantic metrics, I realized that the words with highest similarity scores were all food. Of 350 possible words in the game, more than 30 were food items. Shaving off 10 of the less pliable terms on there and replacing them with non-edibles has done wonders for the number of last minute surprises.  It certainly still happens, but to a degree that feels far more acceptable.
<br/>
<br/>
To complete the analogy from before.  I wanted to buy a plane ticket to Knoxville, ended up researching and driving the roads there a few times to learn them, and then found out the person I wanted to see actually lived down the block.
<br/>
Just an additional reminder that sometimes the problem you're solving isn't the actual problem at all</p>

![](https://i.imgur.com/33LacXf.png "2.21 GIGAWATTS")

### Back in 2022

Hi there.  2022 Shawn again.\\
If you wanted a tiny bit more closure from that previously circular post - I can shed some context from the last year of watching Twitch streams:

The unpredictable and frustrating randomness MAKES our game.

I didn't expect it either (see the analysis above from 2018 Shawn), but it turns out the outrage over getting screwed with a terrible random decoy word is infinitely memorable, and ultimately positive.  Of the dozens of streamers we've watched play WaB, time after time the moments that get clipped and saved and talked about months later are when the game punished an otherwise straightforward clue.

Our mistake was treating the competition as the primary target. How can I "win" a game when it's possible to be thrown under an RNG bus at any moment?
Looking back now, that's obvious.  I've never played a Jackbox game _needing_ to win; it's a nice bonus to see your name up in lights, but the best memories come from the minute-to-minute absurdity, and viewing the game as if it's a malicious AI bent on trashing your hopes and dreams somehow works in our favor because of that. <span class="ref"><span class="refnum">[6]</span><span class="refbody">Our #2 question we get asked most often is how the AI works so well at purposely screwing people over.  No one believes us that it's truly randomized. <br/> <br/> (#1 is how to pronounce "Logiraffe")</span>


