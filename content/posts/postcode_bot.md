---
title: "Postcode bot. Freelance project."
date: 2022-09-17
slug: "postcode_bot"
description: "Telegram bot that used users' input of zip code to show social, demographic, and economic data about specific area."
keywords: ["python", "heroku", "bigquery", "sql"]
draft: false
tags: ["Python", "BigQuery", "SQL", "Heroku"]
summary: Telegram bot that used users’ input of zip code to show social, demographic, and economic data about specific area.
toc: true
---

## Summary:
This is a real-life case study about how I built a Telegram bot with Python for a startup, diving into asynchronous programming and learning a ton along the way. The bot was used as a prototype for validating a startup's hypothesis.

## Tools, I used:
BigQuery, Heroku, and the patience of my senior software-engineer mentor.
_____________________________________________________________________________________________
I have a passion for helping startups. Once, I worked with a company, that wanted to build a platform, that used users’ input of zip code to show social, demographic, and economic data about this area. Of course, there must be more interesting data and insights in the future, but this task was pretty interesting for me. After retrieving all the open data, and long hours of cleaning and transforming it, I finally had about 10 datasets that I put in the BigQuery.

The pivotal task was presenting this data to non-technical stakeholders. My solution: a Telegram bot as a functional prototype for the envisioned platform. This interactive bot allowed stakeholders to understand and explore our data, offering a hands-on approach to hypothesis testing.

The realization of this task was not so hard at first sight. First of all, I did some research about connecting the bot with BigQuery and learned how to create SQL queries inside the bot.

At this time I never used asynchronous programming and thought that it was something that was far away from my experience (you know, how it feels when you a junior in any field). And of course, this time I came to the understanding that I can’t build the working system without it.

So I found some examples of the async python bots on Github and started my journey. First of all, I made a simple user input + bot answer combination and when it worked, I started to add the queries in my main function that should run every query and return the information.

It leads me to this long read as an answer to user input of the zip code:

> postcode_bot:
</br></br>
Here are what I found for Garden Suburb and 2019:
</br></br>
Total population for 2019 is: 17132;
</br>
Predicted male population for 2019 is: 8098;
</br>
Predicted female population for 2019 is: 8673;
</br>
Predicted number of:
</br>
children (0-12): 15.51 %;
</br>teens (13-19): 7.24 %;
</br>adults (20-39): 26.28 %;
</br>mid-age adults (40-59): 27.49 %;
</br>seniors (60+): 23.48 %;
</br></br>The following data is for 2021-2022:
</br>The difference of house price in Barnet from may 2021 to may 2022: 11.4 % ;
</br>Median household income for your postcode: £44394.20;
</br></br>The number of households with:
</br>1 person : 27.71 %;
</br>2 people : 30.25 %;
</br>3 people : 15.97 %;
</br>4 people : 15.61 %;
</br>5 people : 7.26 %;
</br>6 people : 2.5 %;
</br>7 people : 0.54 %;
</br>8+ people : 0.18 %;
</br></br>Working households in Barnet:
</br>The number of the working households: 56.59 %;
</br>The number of the mixed (working+workless) households: 31.01 %;
</br>The number of the workless households: 12.4 %;
</br>1 Household includes at least one person aged 16 to 64
</br></br>The number of people, who lived in:
</br>owned outright residence: 42.4 %;
</br>owned with a mortgage or loan residence: 29.43 %;
</br>part owned and part rented residence: 0.2 %;
</br>social rented from local authority residence: 1.19 %;
</br>other social rented residence: 4.13 %;
</br>rented from private landlord or agency residence: 19.42 %;
</br>private rented as an employer of a household member residence: 0.14 %;
</br>private rented as a relative or friend of household member residence: 0.67 %;
</br>other private rented residence: 0.24 %;
</br>rent free residence: 2.18 %;
</br></br>Social grade for this area:
</br>Higher and intermidiate managerial, administrative personal: 22 %;
</br>Supervisory or clerical and junior managerial, administrative or professional: 44 %;
</br>Skilled manual workers: 14 %;
</br>Semi and unskilled manual workers, casual or lowest grade workers, pensioners and others who depend on the state for their income: 20 %;
</br></br>Social grade for this ward:
</br>Higher and intermidiate managerial, administrative personal: 53 %;
</br>Supervisory or clerical and junior managerial, administrative or professional: 30 %;
</br>Skilled manual workers: 7 %;
</br>Semi and unskilled manual workers, casual or lowest grade workers, pensioners and others who depend on the state for their income: 10 %;
</br></br>Qualification level in the area. Percentage of people with:
</br>Degree level or above: 48 %;
</br>2+ A-levels or equivalent: 13 %;
</br>Apprenticeship: Professional education: 2 %;
</br>5+ GCSEs or equivalent: 7 %;
</br>1-4 GCSEs or equivalent: 5 %;
</br>No academic or professional qualifications: 6 %;
</br>Other Qualification: Vocational / Work-related Qualifications, Foreign Qualifications / Qualifications gained outside the UK: 19 %;
</br></br>Qualification level in the ward. Percentage of people with:
</br>Degree level or above: 57 %;
</br>2+ A-levels or equivalent: 9 %;
</br>Apprenticeship: Professional education: 1 %;
</br>5+ GCSEs or equivalent: 10 %;
</br>1-4 GCSEs or equivalent: 5 %;
</br>No academic or professional qualifications: 8 %;
</br>Other Qualification: Vocational / Work-related Qualifications, Foreign Qualifications / Qualificationsgained outside the UK: 9 %;

As you can see, it is a wall of text. Useful, but not exactly user-friendly.

That’s when I decided to add some visuals. I had to untangle my code to make it happen and spent five days rewriting everything. Moving from Tableau to generating visuals directly in the bot was tough but super satisfying.

Here’s what the bot looks like now:
</br>
<video src="https://www.vysochinam.com/images/bot_flow.mp4" type="video/mp4" width="680" height="540" controls></video>
</br></br>
Currently the bot is not online, as it was deployed with Heroku and now this company doesn’t have a free plan. I already decided what I’ll use instead of it, but I didn’t redeploy the bot yet.

Looking back, this project had a steep learning curve for me. It pushed me to explore new areas of programming and think about data in a more user-friendly way. The best part? Seeing non-techy people interact with the bot and have those ‘aha’ moments.
In the future, I plan to redeploy the bot, further refining its functionality and exploring new data insights.
________________________________________________________________________________

## Aftermath
**In the end, this wasn't just a tech project. It was about making data approachable, learning loads, and helping a startup take its first steps.
And that's what I love about working in this space – it's always about more than just the code.**
