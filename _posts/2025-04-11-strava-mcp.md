---
layout: post
title: Claude, what do you think to my running?
date: 2025-04-1
description: Creating an MCP server so Claude can roast my Strava entries
tags: generative-ai, mcp, strava
categories: generative-ai
thumbnail: assets/img/posts/strava-mcp/strava-mcp-thumbnail.jpeg
---
<div class="row justify-content-center mt-2">
    <div class="col-10 mt-2 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/strava-mcp-thumbnail.jpeg" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Anyone that knows me, knows that I enjoy getting out into the hills and running, and if you let me, it's all I'll talk about.

That passion for running combines nicely with another passion of mine: data. If you log your runs to a platform like Strava, then you'll know that there's a lot of information for you to pore over. I find myself doing this more than I care to admit!

I enjoy digging into the specifics of a run or workout, seeing how my perceived effort maps onto real, hard data, and figuring out where I could improve. But it got me thinking: what would Claude or ChatGPT make of my running activities? Could they suggest improvements? Could they understand trends?

Recently, Anthropic (the people behind Claude) released something called Model Context Protocol (MCP, for short). Large language models like Claude need context to understand how best to answer your question and often, they have learnt enough context to answer your question without you needing to provide it. But there are times when you might want Claude to use up-to-date information from the internet. This is where MCP comes in - it's a standardised way to connect models to different data sources, like the internet.

It's also a method to make models like Claude more agentic, i.e., they can figure out that they need to fetch some information from the internet without you having to ask them. MCP is cool and for a more in-depth understanding, Anthropic have written a [guide](https://modelcontextprotocol.io/introduction).

## Connecting Strava to Claude

By default, Claude doesn't have access to my Strava data. If you ask it, it'll tell you:

<div class="row justify-content-center mt-3">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/most_recent_run.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

To connect Claude to my Strava data, I built a simple MCP server. The server runs locally on my laptop and interacts with the [Strava API](https://developers.strava.com/) and the [Meteostat](https://dev.meteostat.net/) platform for weather data. 

The server exposes two tools to Claude, one to fetch a collection of activities and another to fetch a specific activity. Both tools have a set of parameters that Claude can configure, for example, when fetching a collection of activities, the number of activities and start and end dates can be specified.

Those dates enable Claude to ask for a collection of activities between two dates, e.g., you could ask "Tell me about my activities over the past five days":

<div class="row justify-content-center mt-3">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/activities_over_five_days.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

The other tool is to fetch information about a specific activity. Strava's API provides an endpoint to get a collection of activities, but the information in each of those is limited. To get more information, e.g., segments, splits, and efforts, you have to query the API for a specific activity (more information [here](https://developers.strava.com/docs/reference/)).

I did a run a couple of days ago that I called "Lemon Squeezy" because it was an easy run, naturally. Here's Claude telling me about it:

<div class="row justify-content-center mt-3">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/querying_an_activity.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

If we put it all together, first asking Claude about activities over the past week and then using its output to ask a follow up question, this is the result:

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/posts/strava-mcp/putting-it-together.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>


## Claude, what do you think to my running?

Claude can now fetch information about activities from Strava, knows how to use date to narrow down the search, and can provide an analysis of the data. But, does Claude have any more in-depth thoughts about my running? Are there any improvements I could make?

<div class="row justify-content-center mt-3">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/claudes-thoughts.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

There are some good suggestions in there, particularly around recovery (even though I do have dedicated rests, honest) and adding in more flat speedwork (I'm more of a trail runner than road runner). 

## Roast me Claude

So, we've shown that Claude can work with Strava data and has some good suggestions on improvements. But, we could make it more fun. Let's see what Claude really thinks. 

<div class="row justify-content-center mt-3">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/posts/strava-mcp/roast.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>

Ouch. My girlfriend absoultely deserves a medal, and I live for those single thumbs up on my runs - you know who you are, thank you :pray:

Thanks Claude.

## General Thoughts

Getting an MCP up and running is impressively simple, and Claude is pretty good at understanding what tools it needs to use to generate output. For example, when playing around with the Strava MCP, I found that Claude would fail to get the information it needed and then attempt to use the other tools available to get it, pretty cool. 

The MCP does require good docstrings for Claude to understand what tools to use, what the parameters mean, and what format they should be in. If you're a good engineer, this is standard practise. 

It's not really agentic. The agentic part is within Claude, with it's ability to rationalise that it needs to use tools to complete it's task. These MCP servers provide a collection of tools to enable Claude to be more agentic.

If you fancy seeing what Claude has to say about your Strava activities, you can find the Strava MCP server on my Github: [https://github.com/JonoCX/strava-mcp](https://github.com/JonoCX/strava-mcp)