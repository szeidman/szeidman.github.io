---
published: true
layout: post
title:  Ascetic aesthetics&#58 programming my first CLI gem
date:   2017-05-24 20:43:18 +0000
---



## ![](http://i.imgur.com/gtNkMRf.jpg)

## First principles

There have been a [number](https://www.theatlantic.com/national/archive/2011/05/anatomy-of-a-fake-quotation/238257/) of [articles](https://www.nytimes.com/2017/02/15/opinion/can-you-spot-the-fake-quotation.html?_r=0) over the years (and centuries!) about misattributed quotations and where they come from, what their appeal is, and how to spot them. They may result from copy/paste manglings online, and, absent Jonah-Lehreresque patterns of intentional fabrication, a lot of them manage to simply slip into the ether of cliché, relatively unnoticed. I have been thinking about one of those: Bill Gates saying something to the effect that he would prefer to have a lazy person do a hard job because they'd find the easiest possible way to do it. The quote's a bit clunky (like many of these things), with a whiff of stern authority and self-hagiography. The sentiment makes sense, though.

Indeed one of the unexpected pleasures of moving through the Flatiron curriculum was finding out that Larry Wall, the creator and "benevolent dictator for life" of the Perl programming language, had expressed the point more concisely in a [published work](https://books.google.com/books?id=ezqe1hh91q4C&q=laziness#v=snippet&q=laziness&f=false).

My read of Wall is that good programming reflects an *earned* laziness: tasks that may at first glance require complex, clunky code are broken down into simple parts that easily work together. And learning how to create a particular breakdown makes it easier to replicate or build off of for the next program as needed. Like the correlation between an airplane's aerodynamics and its beauty, applying such principles making for code that itself looks cleaner and better for the next set of eyes. 

## Theory

I thought a lot about the above in building my CLI gem: as I worked to make the program work as intended, the code steadily became more concise.

The gem concerns blockers, the gloves ice hockey goalies wear on their stickhands that feature a large rectangle of padding to protect the hand and wrist. Some call them "waffleboards" because...well, take a look:
![](http://cdn.bleacherreport.net/images_root/slides/photos/000/164/655/astrom_display_image.jpg?1267150359)

The CLI scrapes information about the 20 cheapest blockers from Goaliemonkey.com and displays it on the command line, allowing a user to get more information (also scraped) about any of the 20.

## Practice

Because scraping was fresh in my mind, I focused on it from the start in a class called Blockers. I had recently completed exercises scraping items into an array of hashes. For the gem I needed a more object-oriented focus, but to get started and test my scraping more quickly I went back to the array of hashes approach at first. When I got the scraping behaving as I liked it, I started thinking of an object-oriented alternative to gathering the information and ended up using the scraper method to initialize an instance of a class with each of the variables thatwere assigned the scraped content.

From there, setting up the CLI was fairly straightforward: I built up a welcome page with the initial list (constructed by iterating through the scrapes) and plenty of monkey- and hockey-related emoji. I adjusted the list output and went back to the scraping method to make sure it came out as I wanted it: I needed the list to be numbered so the user could make a selection, for example. When the user makes a selection, the CLI class calls on the Blockers class to find the appropriate instance of itself from which to return data. For all points of the CLI requiring a certain type of user input I made sure to account for times when the user entered input other than what was called for.

At some point I realized that my repository was not set up in a Gem-friendly fashion and corrected for it, though it would have been much easier to have done so before plunging in. Lesson learned.

Debugging the program with Pry and by running the program itself were my favorite parts of the project. It allowed met to refine some customization of the program (scraped output formatting, for instance, and the aforementioned emojis) and to try to find ways to break or otherwise mess with my own program. I of course tried to write code that would work from the get-go, but inevitably had to make corrections. For example, I realized that if the user asked to see the list again after finding info about a particular blocker they would receive a forty-item list. Accordingly, I added a self-clearing method to the Blockers class and called it in the CLI class.

It was also rewarding to take the small step of publishing my first Ruby gem: there is a nice novelty to the fact that someone is only a "gem install blockers-cli-app" away from seeing my work. And some people--okay, some bots--have already done so! It may not have the gravitas of having fake quotes attributed to me online, but it's a start.

![](https://4.bp.blogspot.com/--pZD_9jGSKg/WC3Z7TErP7I/AAAAAAABK_k/8FwPECq1OpMwCtmn1v3yxzCIzwGtYFtFwCLcB/s1600/y-foil.jpg)



