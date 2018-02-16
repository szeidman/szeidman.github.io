---
layout: post
title:      "Tiny Steps: Getting Started with Open Source"
date:       2018-02-16 23:52:16 +0000
permalink:  tiny_steps_getting_started_with_open_source
---


Earlier this week I went to an event where I contributed my first pull request for an open source project, which was subsequently merged into the app. The contribution itself was incredibly modest, but participating was very instructive: most importantly, it has given me more confidence in jumping into projects knowing that I have something to offer even for apps I previously knew nothing about.

## Picking a project

The event had speakers from a few organizations, each with interesting projects at hand. After each organization gave an overview of what they were working on and what kinds of frameworks and languages they were using participants broke off into working groups with each of them. I decided to join with an organization with which I had some subject matter expertise from my previous career, and also some coding expertise: they had a backlog of issues for a Rails app. Serendipitous.

## Paired programming

I tentatively asked another participant about pairing up for some of the issues, since I wanted to make sure I was following best practices with my contributions. Thankfully I was able to partner up, and as with most paired programming experiences I found it beneficial to learn from my partner and help work our way through the required troubleshooting.

## Patiently progressing

A typical part of open-source contributing consists of making sure the end product will run on one's own machine locally. Indeed, this was by far the most time-consuming part of the process! Once the errors were dispatched, we had the site up and running locally with seed data and could get to work. We identified what seemed to be an easy fix for the issue and went through the code to make sure it seemed promising. It was a very concise addition to the code, but on saving it we held our breath to see if, on rebundling and restarting Rails, there would be any unintended consequences. Everything seemed okay! We ran the test suite to be sure: there were a few failures, but they were not related to our change. All set.

On to Git. We doublechecked how the org wanted us to submit pull requests and followed their guidelines. Git add, commit, and push. On to the next issue, a bit more invoved now. Time was running out on the event and we did not have a proposed solution. It was a bit disappointing to have only taken care of a small, low-hanging fruit of an issue, but the organizers were grateful and pointed out that even the smallest fixes add up. Even though I'd gotten accustomed to coding feats of increasing complexity and derring-do, making that first small contribution to an open source project felt like a significant step, and will hopefully be the first of many.
