---
layout: post
title:      "Tech & the City [ed. note: sorry]"
date:       2018-01-19 11:16:13 -0500
permalink:  tech_and_the_city_ed_note_sorry
---

## BetaNYC's "Unlocking Open Data for Community Boards"

Last night I went to an event at the WeWork space in Harlem that, while largely targeted for community board members in New York City, also provided some inspiration for harnessing the plethora of publicly available data the city has had to offer since 2012 legislation went into effect.

## The Event

BetaNYC, a meetup-turned-nonprofit, were the main facilitators of the event. The target audience was community board members, so the focus would be on accessing data from the various visualization tools available online.

After a brief introduction to their organization and the other event sponsors and hosts, we began with an exercise in live data manipulation. "Live" meaning each participant had a paper with data fields from a 311 request, to which we added our names (with complimentary pens from Gale Brewer's office, no less) and sorted, filtered, and pivoted ourselves around the room.

Afterward we received a demo of some of the tools available to wade through some of the city's open data, again beginning with 311 requests on a tool called [Boardstat](http://manhattanbp.nyc.gov/html/community-boards/boardstat.shtml/).

![](http://manhattanbp.nyc.gov/images/photos/boardstat.png)

Boardstat is based on Microsoft's Power BI, which the organizers conceded (and needed to reiterate throughout the event as participants gave feedback) is a bit hobbled for the job: the GUI is quite clunky and browser compatibility is not the greatest (and apparently not present at all on mobile browsers). They discussed some common problems with the 311 dataset: so far, Manhattan is the only borough that has opened the data to Boardstat; duplicate callers (e.g., the same person repeatedly calling with a noise complaint) may distort the numbers; and inputted addresses are far from uniform.

The demo also touched upon [NYCityMap](http://gis.nyc.gov/doitt/nycitymap/), which provides access to multiple layers of datasets; [NYC Crime Map](https://maps.nyc.gov/crime/), which is similar but, as the name suggests, is focused on crime and is intended to link to the NYPD's CompStat data (though that integration needs updating); and the [Liquor Authority Map Project](http://lamp.sla.ny.gov/nysla/index.htm), which is built on (alas) Flash. The presenters showed how a community board could use the various data sources to make better informed decisions: quickly seeing if certain residential complaints, like lack of heat/hot water, were indicative of a larger pattern of violations by a specific building owner, or weighing different neighborhood factors in deciding whether to approve a liquor license application.

After the demo, they provided worksheets with "data journeys" that provided guided navigation through the various sites, along with further information about the datasets.

## Takeaways

The event was frustrating and heartening.

Frustrating, because of the old architecture a lot of the tools need to rely on, largely due to budgetary constraints. Also, the aforementioned bureaucratic thickets suggest that collaborating and sharing info and tech between the various government agencies is probably trickier than it should be. I recently read the New Yorker's piece on [Estonia](https://www.newyorker.com/magazine/2017/12/18/estonia-the-digital-republic), which has made a concerted effort to streamlining access to data and services, and the contrast is notable.

Heartening, because of all the grassroots work that had clearly gone into bringing the city to where it is today: not long ago, getting the same data that we can now quickly pull up on a browser required navigating bureaucratic thickets and steep access charges (upwards of $500 per request, per the organizers). It was also uplifting to see the diverse coterie that turned up for the event, with a wide range of tech familiarity, that were now familiar or more familiar with tools that are useful for all New Yorkers.

The frustration was also heartening in its way: people are more than aware of the technical limitations and are continually working their way around them. Those introduced to the datasets for the first time quickly realized their potential, and growing awareness of it should, with more effort and as word spreads, help push the state and local governments to take steps to broaden and improve access. The fact that a lot of these problems have been solved elsewhere can provide fodder for griping but is also motivating and encouraging: this stuff can be done, because it has been.

For me the event was a great introduction to some datasets with which I had been unfamiliar. I'd used some of the NYCityMap layers before, but the organizers also pointed out to some of the developers in the audience that [NYCOpenData](https://opendata.cityofnewyork.us) has gathered much of the underlying data we saw during the presentations. The developer community has already ran with many of them to create some novel applications. I checked out some of the APIs during the event and began thinking about what I could build or help contribute to by using them.

