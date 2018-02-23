---
layout: post
title:      "Just a moment"
date:       2018-02-23 17:05:29 +0000
permalink:  just_a_moment
---


Parsing and storing date and time information consistently is one of those small tasks that can have a big impact if not addressed properly. I found moment.js a helpful library to quickly improve my Rosewind app in this regard.

Previously in Rosewind, the client-side React app would get ActiveRecord timestamps from a Rails API and then parse them out into a more user-friendly format on the client-side. I did this with a custom method However, these stamps remained in UTC format without any local time adjustment. I looked into some ways to change that.

It's a best practice to keep backend timestamps standard and let users see the time in the zone they're currently in via client-side adjustments. To get the local browser's offset from UTC, I found I could assign a variable to "new Date().getTimezoneOffset()", which returns the offset value from UTC in minutes. From there I could add some logic to my parsing method to add or subtract the minutes and adjust hour and date values as necessary.

Interesting stuff, as these things go, but this seemed like a fairly common task so I decided to see if there were any libraries up to it. I came across [moment.js](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=0ahUKEwjzpMe-wbzZAhVqyVQKHWstAw0QFggpMAA&url=https%3A%2F%2Fmomentjs.com%2F&usg=AOvVaw2w0NKVDb3KVqyyecoj7yeL), which did the job cleanly and allows for quick formatting adjustments if needed. No need for extensive modifications to my parsing method if I wanted to, say, write out a month's name rather than number. 

Even though ActiveRecord's timestamps are in a different format than is typical in JavaScript, the library had no problem parsing through it and returning the time in a user-friendly format.

[Here](https://github.com/szeidman/rosewind/blob/local-time/client/src/components/FavoriteDetail.js) is the relevant code change for Rosewind on Github.
