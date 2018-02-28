---
layout: post
title:      "Deployment debugging: odd behavior on Heroku for a React/Rails API app"
date:       2018-02-28 15:35:18 +0000
permalink:  deployment_debugging_odd_behavior_on_heroku_for_a_react_rails_api_app
---


The [Rosewind](http://rosewind.herokuapp.com) app is, at long last, up on Heroku. I will still work on expanding and improving some of its features, but whenever I worked on it I was confronted with bizarre behavior on deployment that I could not replicate locally. I decided to drop everything else for a bit and focus on getting it to deployment.

Locally, everything was working well: I had my internal and external API calls running smoothly to access Charities and Favorites. I followed online tutorials to deploy to Heroku. There were some snags with making sure routing worked properly, but everything else looked good in produciton. Then I tested deleting a Favorite and was greeted with my custom error message. Same thing if I tried to edit a Favorite.

I went back to the local build. No problems. I used the Heroku CLI to run the Heroku app locally; same result. I switched from SQLite to Postgres in the dev and test environments. Same thing. More than a bit confused, I went back to the deployed site and looked at what was happening to trigger the errors: the requests to the API were supposed to have a URI of ~/api/v1/charities/[Charity ID], but on deployment there was "undefined" in lieu of the Charity ID number.

Perhaps Heroku's database somehow was not storing IDs or timestamps by default? I accessed it from the command line, though, and there they were.

After some unsuccessful Google searches about the problem, I retraced where that Charity ID should be coming from: the React/Redux state, where it would be part of the information about a given Favorite. Favorites were added to the state by iterating through the json returned by fetching from ~/api/v1/charities. And there I saw the problem: the state did receive data back from that fetch request, and included all the Favorites, but it did not include several fields: id numbers and the created_at/updated_at timestamps. Sure enough, these fields *were* included in the state when running locally.

So the problem was likely with the ~/api/v1/charities json payload. I ran curl from the command line for both the local and deployed website to confirm: locally, all fields appeared, but not in deployment, and it had to do with the backend, not the frontend. I needed to make the json payload from Rails include all fields.

Accordingly I went all the way down the stack to the API controller. In the index method, I had "render json: @charities". Something about that rendering was not being consistent in Heroku. To prevent whatever mechanism was stripping the id and timestamp fields away from the JSON, I simply added ".to_json" to the line so it read  "render json: @charities.to_json".

It worked! Only 7 characters of code to fix what had appeared to be an incredibly vexing problem.

Getting to those 7 characters was a reminder of the discipline that can be needed when debugging fullstack React projects, though: in hindsight, it may look like a quick fix, but in reality tracing your way down from the frontend to the backend, not knowing exactly where the problem lies, means you may encounter a lot of false starts along the way.

