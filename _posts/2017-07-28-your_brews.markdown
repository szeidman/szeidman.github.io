---
layout: post
title:  "Your Brews"
date:   2017-07-28 14:03:22 +0000
---

![Torgo](http://i.imgur.com/KbB81qW.png)

*Pictured: Torgo*

## Using Rails to Create a Brewery Recipe Manager

The specs for the recipe manager seemed straightforward, but I ran into some wrinkles.

I created a hypothetical brewery and named it after Torgo, who is apparently in the public domain. (Link tk after I deploy to Heroku; watch this space.)

The site allows all users to have a look at the beers and ingredients in the site's database. You can see the attributes of the beers and ingredients, how much of each ingredient is in a given beer, and break the ingredients down into categories (that is, whether they are hops, malt, yeast, or water).

### Seeding

An early challenge was making sure I had seeded the database in a useful way: even though the relationships between each of the user, beer, and beer ingredients classes were not difficult to determine, it was important to test that they were in fact working properly and to create a variety of sample data to do so.

Because I wanted to ensure throughout that each beer had each kind of ingredient, I used a counter to make sure the ingredient seeds were divvied up accordingly. Though I did not realize it going in, it turns out that the Faker gem had dummy seed data for most of the categories I needed, including the beer-related stuff, a nice surprise. Faker also has Twin Peeks characters and locations, so naturally I used those where I could as well.

### Custom Attribute Writer

From there most of the challenge was related to implementing a custom attribute writer. Rails has a few shortcuts that make it simple to assign the attributes of a nested class (say, the Ingredients of a Beer), but I needed to also include the amount of Ingredients in a given Beer. This amount attribute would be stored in the join table that kept track of which ingredients were in which beers.

To add to the challenge, I had my self-imposed rule that each beer needed to have exactly four ingredients, one of each kind. For new and edited Beers, this meant a lot of legwork in the "ingredient_attributes=" method: it took in an Ingredient of each kind and an amount, then assigned the Beer id and Ingredient id to the beer_ingredients join table with the corresponding amount. This required careful crafting of the Beer form to ensure each of its fields posted to the right place, and some custom error validations to make clear exactly which fields errors pertained to (for example, which Ingredients were filled in with an invalid amount).

I also needed to create a form for to add a new Ingredient directly to a particular Beer. At first, I accomplished this with a Beer form, but the redirects after submitting the form were cumbersome. Instead, I engineered a way for the Ingredients form to take in the attributes. Unlike the Beer form, for this Ingredients form I used the Ingredients controller for much of the attribute assigning legwork. I believe this is probably the better approach.

### Creation, Editing, and Deletion Permissions

Once I had the custom attribute writer working as expected, with the forms redirecting where they needed to, I added some finishing touches to give users a good amount of control over beers they created. The Beers index page flags which ones belong to the current user. Only the user who created a beer can edit or delete it. No one may Delete ingredients when they are contained in a Beer, and editing an Ingredient's kind is not permitted after creation. This ensures that Beers continue to have one of each of the four types of ingredients.

### Next
After refactoring the code a bit in the coming days, I will get the project deployed to Heroku, making it the first of my projects that I can easily share with people without making them install a bunch of things in the command line. Another small milestone.
