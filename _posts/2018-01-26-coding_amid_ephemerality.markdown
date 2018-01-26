---
layout: post
title:      "Coding amid ephemerality"
date:       2018-01-26 18:23:31 +0000
permalink:  coding_amid_ephemerality
---

![via Macintosh Repository](https://www.macintoshrepository.org/_resize.php?w=640&h=480&bg_color=333333&imgenc=ZmlsZ916XMvbWcvc2l0ZXMvbWcvZmlsZXMvc2NyZWVuc2hvdHMvc2NyZWVuX3Nob3RfMjAxMi0wOS0yNl9hdF8xMC41Ni41NV9wbS5wbmc%3D)

Earlier this week, while puzzling over the best way to move calls to an external API from a React frontend to a Rails backend to keep the API key safely hidden, I was listening to Prince's Sign '☮️' the Times album and was launched into a Proustian reverie by the fourth word on the third track. The song's called "Housequake" and the word is "Damn!", exclaimed in nasal frustration by The Artist. (Shamefully his symbol is not a part of Unicode at the moment.) I'd heard it before, I realized, outside the context of the song, where that shout portended Death itself.

Specifically, it meant that your spaceship had been hit in [Solarian II](http://www.sticksoftware.com/archive/Solarian.html), an old MacOS game. If you're not familiar with it, head over to one of those bars packed with old arcade games and seek out Galaga, Space Invaders, or anything where you're required to move a spaceship or plane back and forth across the bottom of the screen while shooting at and avoiding enemy swarms of what-have-you. That will give you a baseline. Now listen carefully above the din of the bar: do you hear any Prince samples? Do the game's instructions include the question, "Why is there a bird?" If not, the game is not Solarian II.

The game had a bit of a cult following: it seems far-fetched these days, but back when it came out the main mode of distribution for it was to copy it onto a disc and give to others, who were supposed to try out the game and, if they enjoyed it, send money in the mail to the creator. This "shareware" distribution model is not exactly in vogue these days, but before widespread adoption of the Internet it was fairly common. The game circulated for years in this fashion. As more people went online the game still chugged along, but the switch to the Unix-based Mac OS X required a new port of the game. That port quickly became obsolete when Apple adopted Intel processors and Mac OS dropped support for the older PowerPC processors.

All this has me thinking about what I've built so far. We know from Shelley or at least Brian Cranston that everything we build only lasts through a certain time horizon, and there are plenty of articles and concern about the difficulties of  preserving digital data over the years. So, beginning with the premise that in the long run all webpages are dead, what can we do to keep them going for as long as needed?

To a large extent, clear, DRY code that largely adheres to best practices and conventions seems like the best approach to durable code: even in the short term, difficult-to-read code is unlikely to survive the review of the next developer who is tasked with updating it.

On the other hand, though, part of coding is embracing the ephemeral: things will change, and most of what you make will need lots of updating in order to not look firmly ossified in the time in which it was created. There are not many companies online promising to keep things exactly as they are.

Still, it is worth keeping in mind what parts of one's code should be preserved or adapted over time, and building that code to last. Even though the backend for a Google search has changed dramatically over the years (they once scoffed at a search engine supported by ad revenue, for example, which suffice it to say is not their current attitude), the Google homepage doesn't look much different from back in the days when it was still feasible to load up a game of Solarian II.


