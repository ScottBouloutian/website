---
layout: post
title: Starbucks Coffee from the Terminal
author: scott_bouloutian
excerpt: I created a script to automate my daily intake of caffeine
modified: 2017-12-10T1:48:00-05:00
categories: projects
tags: []
image:
  feature: coffee-terminal.jpg
comments: true
share: true
---

## The Idea
Well met! As a developer, I get coffee on a regular basis to fuel those long stretches of concentration on coding. It may come as no surprise that I turn to Starbucks as a provider for my often daily small coffee. This habit got me thinking if there was a way to automate the process. After learning that Starbucks has an Alexa skill to reorder your favorites, I had an idea. The problem could be reduced by automating an Alexa speech query.

## Process Diagram
The idea here is to use the Alexa Voice Service API to interact with the Starbucks Alexa skill. However, the input and output of this API is speech. In order to create a "coffee CLI," I would need a way of inputting a speech query as text and receiving some output text as a response. To achieve text to speech I used AWS Polly, and for the reverse, I used Google Speech API. As of writing this, I do not believe AWS has a speech to text service.

![Process Diagram]({{ site.url }}/images/koffee-diagram.png)

## Results
I was really pleased with the results of this endeavor. Not only had I created a script to automate my daily intake of caffeine, but I had created a generalized way of interacting with the Alexa API over the command line using text. Ten minutes after typing the command `koffee order`, I walked over to Starbucks and enjoyed a nice tall coffee!

![Koffee Program](https://raw.githubusercontent.com/ScottBouloutian/koffee/master/screenshots/koffee.png)

<div markdown="0"><a href="https://github.com/ScottBouloutian/koffee" class="btn">Source Code</a></div>
