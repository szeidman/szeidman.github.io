---
layout: post
title:      "Back to the Front End"
date:       2017-10-10 18:31:04 +0000
permalink:  back_to_the_front_end
---

![](https://i.imgur.com/tz7MnB7.jpg)

At long last, the Brewery App is up to spec for the [Rails App with a jQuery Front End](https://github.com/szeidman/brewery) project! What does that mean, and how did I get there?

## Initial Attempts

I first tackled this project a while ago, knowing I would be going through a brief stretch when I simply would not be able to spend much time coding. This put a bit of pressure on me, as did the fact that I was delving back into JS after a long stretch in Ruby.

The app itself, sans jQuery, allows Users to create Ingredients and add them to Beers. I had quite a bit of custom error handling and class methods to handle the quirks of doing so. For example, I required each beer to have exactly one of each of four kinds of ingredients. As such, I was a bit wary of translating everything into JavaScript and spent quite a bit of time trying to figure out where I would be adding the jQuery front end: should it be there when you create new Beers? For new ingredients?

I paid careful attention to the Rails/JS materials at my disposal, but some things did not seem to be clicking. Still, going along with the theme of many of my projects thus far, I found some interesting ideas about error handling from a [blog post](http://tomdallimore.com/blog/ajax-and-json-error-handling-on-rails/) and started to run with it. (Nevermind that JS error handling, like my four-ingredients-per-beer requirement, was *not* part of the specs!) I also managed to get a sort-of workable show page rendering, but had some work to do in order to implement a "next" button on the page to cycle through the other items' show pages.

## Another Round
Eventually the coding hiatus hit. When it was over, this project still loomed. I decided to go back through the JavaScript materials. By happy coincidence Flatiron had just revised its JS curriculum, so there were also some new concepts included in my refresher and I felt much more confident for going through it again.

I was less enthused about my code from before, which I had been committing locally but had not even had the confidence to push to my GitHub remote. (In hindsight, this is of course a terrible reason not to push commits and fortunately my computer did not suffer any damage between then and now.) I decided to copy my local repo in case anything I had done proved useful but then forked a new branch from right where I had finished the earlier, non-jQuery iteration of the app.

### Rendering an index with jQuery

The new start proved beneficial: I had been stuck on error handling for form submission before, and had also hit a wall in rendering a show page, so I decided to forego working on that aspect until I had tackled a few of the other specs. First up: render an index with jQuery.

This proved to be a relatively easy task: some of the gremlins that kept me from making progress earlier had to be Googled and resolved, such as foregoing the use of TurboLinks for now, but once I could focus on the jQuery itself I ended up being able to display each ingredient in a beer by rendering from JSON once a "See the Ingredients" link was clicked. I used a data-id to pass the right Beer's ID to the JSON GET request in order to find the right list of ingredients.

Handlebars templates had given me some confusion when I first encountered them, but after some more Googling I found a straightforward approach to implementing them that I used throughout the app. Sorting through these issues with the most straightforward part of the juery implementation made life easier later on.

### Rendering a show page with jQuery

I realized earlier on that the Beers show page would not be a good candidate for re-rendering because of all the class methods I would have to adapt, such as one that evaluates the color of a given Beer. And the more I thought about it, the more I liked the idea of keeping the jQuery focus on Ingredients: they are more likely to need access all at once, or to need quick creation, since they make up each Beer.

I had a look back at my previous code and, armed with some newfound confidence in Handlebars, got a show page to render relatively easily. The trick would be implementing a button to cycle to the next Ingredient. I had tried a rudimentary way of doing so earlier that would simply increment an Ingredient ID number, but it could not handle deleted Ingredients: if Ingredient #5 were deleted, the next button on Ingredient #4 would search in vain for it.

As such, I rigged up some logic to gather an array of all Ingredients, find the index for the current Ingredient, and increment through the array indices rather than the Ingredients' IDs. I had the function keep track of the index's length so that it would cycle back to the beginning of the array after reaching the end.

This worked fine for viewing all Ingredients, but I also wanted to adapt the logic for cycling through a Beer's Ingredients. I did this by using multiple data-ids in order to track both Beer and Ingredient. I also modified the JSON request for instances where users are on the Beer Ingredients page so that the request fires specifically to a Beer's Ingredients. Finally, I used the BeerIngredients join table, which ActiveModel brought over to the API, to also capture the amount of each Ingredient in a Beer.

My Rails logic for showing Ingredients from a particular Beer included some custom logic to display different messages depending on whether an Ingredient was used in other Beers, which I hope to add to JS as well at some point! In the meantime I did replicate the Rails logic for Ingredient pages viewed on their own, which show all the Beers in which they can be found.


### Using the Rails API and a Form to Create a Resource and Render a Response 

Next I was back to what had slowed my progress in the first place: handling a form submission. Looking back at my code from when I first began, I was further along than I thought: I had an Ajax POST request that serialized the entries in a form upon submit, and had replicated a good amount of the error logic from Rails/ActiveRecord.

This time, though, I decided to focus on successes before the errors, and went about making sure I had functions that could handle the form response data and throw it to the DOM. The formSubmit function I used was pretty agnostic as to whether the form was a New Ingredient or one of a Beer's New Ingredients, but I again focused first on the simpler case: New Ingredients without an associated Beer on creation.

Once that was in place, I found that the main difference between creating a New Ingredient and a New Beer Ingredient was whether the posted JSON was an Ingredient or an array of each of the Beer's Ingredients. I also found that the Beer's Ingredients array would be in numerical order by ID, meaning that the newest ingredient would always have an index of 3. That meant I could check to see if json[3] existed and, if so, pull info from json[3] to populate a new Ingredient object; if it did not, I could just use json:

```
   let last = json[3];
   let forNew = (last) ? last : json;
   let ingredient = new Ingredient(forNew)
```

With a few more modifications, including figuring out how to change the submit button back to enabled to enable reusing the form without a refresh, I had it working as I wanted. Back to the errors!

The blog post mentioned above helped out nicely for error handling. Ordinarily, a form submission with errors would throw an error in JS as well, but the post suggested going to controller logic and adding the errors to be rendered into json. I played around with this a bit and ended up adding the below to the create method on my Ingredients controller, to execute whenever an Ingredient fails to save.

```
            respond_to do |f|
              f.html {render :new}
              f.json {render json: {error_messages: @ingredient.errors.full_messages, error_type: @ingredient.errors}}
            end
```	

Because this JSON successfully renders when a form fails to submit, this error handling would still be passed to the ".success" part of the POST request. (A nice part of coding is changing "errors" to successes; that is, accounting for user actions that would ordinarily result in errors and finding ways for users to resolve them instead.) ".success", therefore, was where I placed the code to replicate the appearance and text that would have been rendered by HTML had I left the form well enough alone.

I ended up not using the "error_type" in the code above at all: I thought it would help match up which field should be rendered as a "field with errors," but it was easier to take the first word of the full_message (e.g. "Name can't be blank") and see if it matched with the field's name. With error handling in JS, the New Ingredient form could now handle both valid and invalid form submissions without a refresh.

From there, since refreshes were no longer necessary, it was just a matter of refining the info appended to the DOM about the Ingredient that was just created. I again used a Handlebars template and got it to my liking. I also got the submit button to change to "Create Another Ingredient" after the first successful submission on the page.

## Onward

This project took longer than I would have liked, but I am happy with the ultimate result. My leeriness about revisiting a complex Rails app was not altogether unjustified, but it was a good challenge to build upon a developer's (that is, my) quirky initial code rather than starting from scratch. The front-end branch of the app ended up with more commits than the initial site. In part, this may reflect better Git habits and my worries that even a subtle change in JS would break everything, but it also shows that improving a site's front end can be at least as involved as sorting out the back end logic.
