---
layout: post
title:  "Back to the Brewery"
date:   2017-09-07 15:17:17 +0000
---


I'm working on adding a jquery front end to the Brewery app described in the last couple of posts. In short, this will let items on the page render more quickly for users: for example, I'm hoping to have it up and running so that a user can fill out a new Ingredient, see that Ingredient, and fill out a form again all without reloading or clicking through pages.

For now, here is a list of things that I am working on: once I have them figured out, I'll address the solutions I came up with in another post.

## Re-rendering without using 'remote: true' / Knowing when to fold 'em

Given the amount of rails in the pages the app has, it is tempting to use a pattern that would let me preserve a lot of the rails language largely as is and have it converted to and from javascript strings. The specs prohibit doing so, though, which will require some improvisation. My guess is this will involve quite a bit of replicating logic currently implemented in rails into equivalent javascript logic.

Given the above, I am also considering whether to instead implement, say, a Comments function to the existing page rather than wrangling through the revised logic I would need to implement on the Beers and Ingredients page. This should still meet the requirements of the specs. I do like the challenge re-rendering presents, though, so I'll give that a shot first: hopefully a combination of js functions and handlebars templates will do the trick.

## Resetting the Ingredient form to allow for multiple submissions

This seems like something I will figure out in a blinding flash of the obvious, but for now after the submit button on the form is clicked it cannot be clicked again. I would like the whole form, including the button, to reset itself after a submission so the form can be used again.

## Again with the error handling

Because I want the form to work multiple times, I've started implementing error logic so that if a user fills out the form erroneously the form can be corrected and submitted again.

## Rendering the next Beer in the show page

I have a rudimentary Next Beer button that counts up Beer IDs to move to the next one, but it needs to account for deleted Beers. As it is, hitting the Next Beer button from Beer #3 will try to get Beer #4, but if Beer #4 had been deleted there is nowhere for it to go. Accordingly the method needs a couple of changes: the ability to skip invalid Beer IDs and to know when it's reached the end of the Beers. This will probably involve changing the logic to iterate through an array of all the Beer IDs, incrementing up by index number rather than ID number, and changing the behavior once the increment gets to the length of the array.


