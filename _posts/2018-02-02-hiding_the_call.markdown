---
layout: post
title:      "Hiding the Call"
date:       2018-02-02 16:55:04 +0000
permalink:  hiding_the_call
---


## Getting a single-page React app with a Rails API ready for deployment

As described in an earlier [post](http://www.samcode.org/rosewind_a_charity_navigator_navigator_with_react_redux_and_rails/), I had put together a React/Redux-driven front-end to an app to browse through and annotate Charity Navigator listings. (Here it is on [Github](http://github.com/szeidman/rosewind).) It works well running locally, but I'd like to have it up on Heroku as well. There are a couple  obstacles that must be overcome first, though, before the deployment is viable: 1) moving calls to Charity Navigator's API away from the client-side and 2) making React-Router, or something similar, play nice with Heroku. I'll address the second obstacle in a future post. For topic one, here is the approach I took:

## Identifying the problem
Guarding third-party API keys and secrets is a must: if they make it into the final product in a way that's accessbile to users, they can use and maybe abuse them for their own purposes. Charity Navigator is an organization I admire so I would like to avoid antagonizing them with excessive API calls if possible. But, even with my API keys ostensibly hidden away in my .env file, they still appeared plain as day to anyone who would run developer tools in their browser. This is because my fetch requests called the APIs directly, meaning even without revealing the key and secret in the code they would still be added in when the code executed.

## Moving to the back end
Since the client-side external API calls were problematic, I decided to leverage my own Rails API on the backend. The goal would be to have the frontend send the mutable parts of the API call (e.g., what U.S. state abbreviation to search in the API) to the backend, which would then add that parameter to a call  to the third-party API and then send the third-party API's response to the frontend. Here's what that involved:

### Rails-side: Adding a new controller, routes, and methods
To get the backend ready to receive requests from the frontend, I added a new controller called CharityNavController. I also added a couple new POST routes, search_state and search_ein, and queued up associated methods in the controller. I used the HTTParty gem to handle my requests, which was just a matter of calling HTTParty.get to the CN API uri, which I cut-and-pasted from the frontend and adjusted the syntax from JS to Ruby in order to make sure the CN API key and secret came over from their hiding place in .env (and, in deployment, from Heroku's env variable storage).

When the response came back, I found it was in a different format than what I was getting back in my direct calls to CN. To bring it back in line with what I was hoping for, I added ".parsed_response" to the response and sent it back to the frontend via "render json:". Success!

### JS/React: Making the call to the internal API instead of the third-party one
The action here happened in the actions. Instead of GET requests directly to CN's API, I now made POST requests to the internal API that would send the appropriate state code or charity employer ID number over to Rails as params. Because I made sure to have Rails send back the data from the third-party API as if the frontend had made the call directly, all it took was modifying the fetch requests to be POST with the appropriate param rather than GET and to update the URL from the CN API to my own.

The frontend now handles errors and parses the data as before, behaving just as it did earlier but without revealing the API key and secret to the user!




