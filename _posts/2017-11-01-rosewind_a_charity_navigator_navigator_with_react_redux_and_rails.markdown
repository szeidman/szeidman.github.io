---
layout: post
title:      "Rosewind: A Charity Navigator Navigator with React, Redux, and Rails"
date:       2017-11-01 18:18:22 +0000
permalink:  rosewind_a_charity_navigator_navigator_with_react_redux_and_rails
---

![](https://www.charitynavigator.org/_gfx_/promo/new/logo125x125.gif)
*Charity Navigator, whose informative API serves as a key source of data for Rosewind*




Rosewind takes data from CharityNavigator, an organization that, in a nutshell, analyzes the effectiveness and efficiency of charities and nonprofits, and allows the user to add particular Charities to their Favorites along with their own notes. A lot has gone into it, my final project for Flatiron (for now!), and it is ripe for even more features. I will continue adding to and restyling the existing app, but here is an overview of what it has so far. 

## APIs
### Linked Rails API

Using create-react-app, I was able to start with an API-only version of the Rails architecture and then add in a React front end which in turn can send and receive data into the API. The API and frontend run simultaneously on different ports to enable the flow of data between the two.

### External API from CharityNavigator

I signed up for API access to CharityNavigator so I could add actual charity data to Rosewind. The free tier of the API has its quirks, so I used a couple different Fetch requests to generate a list of Charity search results and another one to generate the data for an individual Charity.

## The Front End
Learning React and Redux felt a bit like early grammar lessons in middle school French, where there is a very specific structure to things and how they agree and connect with each other that can seem clear from the initial examples you're given but still requires a deeper understanding to create things of your own that actually make sense.

The basic interplay of setting up a store to set the state of the page, using different actions and reducers to manipulate that state, and passing the state to properties of different components seems straightforward at first, but in practice it requires a lot of thought and adaptation: should this be a container, or stateless? Maybe it be both, or maybe there's a way to split up the data manipulation and rendering without breaking everything? Because of all the room for error in the JS, Ruby, and React/Redux processes, debugging becomes a bit more difficult than usual given all the hiding places for errors.

After lots of trial and error the page came together, though. React-router is there to provide RESTful routes for Charities and Favorites. Reducer actions, along with the state, can be passed up to props as needed. Asynchronous requests to and from the APIs work as they should and the page indicates to a user that the are in progress. Material-UI provides some nice styling elements, which I will continue to refine.

## Next Steps
The app as it stands is designed to meet the specs for the project, but along the way I learned of some added features I will continue to work on.

For one, I would like to link Users to the front end to let each individual User sign in via Auth0 so they can manage their own individual Favorites and not see those of others. I have already used Auth0 to use a secure token to protect the API from undue manipulation, but it makes more sense to assign an access token to an authenticated User after they have received an authentication token. JSON Web Tokens should be able to facilitate all this.

On the API side this will also entail using UserCharities (i.e., Favorites) to keep track of which Users have favorited which Charities; accordingly, the UserCharities class will hold onto the Notes and any other data about a Charity specific to a User. (For now, the Notes attribute is just attached to a given Charity in the API.)

Secondly, in its current form I have the Charity search pre-set to return Charities from a given U.S. state, but it should be fairly straightforward to allow a user to toggle the search results by state and/or some of the other possibilities the CharityNavigator API allows.

Getting the site working in its current form was a great way to dive into React/Redux and get familiar with making an app this way. Even after learning from examples, it took building something from scratch to really appreciate how they work and what they offer for users.
