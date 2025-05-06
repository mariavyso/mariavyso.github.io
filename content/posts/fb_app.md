---
title: "Server-to-server FB App for data pipeline. Work Project."
date: 2025-05-01
slug: "fb_app_pipeline"
description: "This pipeline eliminates a task that previously took 24 hours per project, saving ~15% of total work hours monthly during our last campaign. It’s scalable, already in production, and lays the groundwork for better analytics, automation, and real-time capabilities."
keywords: ["FB Messenger API", "python", "shell", "GCP", "ubuntu", "flask"]
draft: false
tags: ["Python", "FB Messenger API", "Shell", "flask", "GCP"]
summary: I built a new pipeline with FB App that replaces Chatfuel's export workaround and simulation of the events on our side, delivering reliable, event-based Messenger data to our Data Warehouse.
toc: true
---

## Summary:
I built a custom Facebook App and a small pipeline that replaces our reliance on Chatfuel’s export process, that we had earlier (you can dive deeper into it [here](https://www.vysochinam.com/posts/pipeline_airflow_docker/)).
<br>
This new solution gives us reliable, event-based Messenger data with timestamps for each user interaction with our bot. This system eliminated a task that previously took 24 hours per project, saving ~15% of total work hours per month during our last campaign. It's scalable, already in use internally, and lays the foundation for cleaner analytics, automation, and future real-time capabilities. I am now pushing our umbrella org to adopt it, teaming up with product, analytics, and ops to measure their monthly time‑savings and drive change across teams.

## Tools, I used:
FB Messenger API, Python, Shell, Linux, GCP — Storage and BigQuery.

_____________________________________________________________________________________________


I was tired of fighting with CF to access data I knew was there. I even tried to simulate events in our [current pipeline](https://www.vysochinam.com/posts/pipeline_airflow_docker/). But CF makes undocumented changes often, and one tweak can break the entire flow. I wanted a stable, scalable foundation that we own end-to-end.

Having this App and pipeline already reduces ~15% of my total work hours per month, just by automating one task and I’m pushing our umbrella org to adopt it to have a truly transformative results.
<br>
<br>

## The problems we had with CF:
<br>

- lack of the API endpoints to export the database, which makes me reverse engineer GraphQL requests that are sent from the frontend, only to be able to receive full user data.

- lack of event-based data. Technically, CF has event-based tracking, but they don’t expose it to us. That’s why bot builders end up adding JSON API calls to every single button or action—just to collect basic engagement data.

- cutting the attribute list. CF silently stops supporting attributes after a certain threshold, even if you didn’t delete them. We needed unique attributes per broadcast, so our attribute list kept growing. At some point, old attributes just stopped being queryable, even though users still had them. You can’t filter by them in the people tab, and you can’t get fast counts.

- overwriting attributes. We heavily rely on free-form user input. But users don’t always respond in one message. They pause, edit, correct, and send you their thoughts in 3 separate messages. That’s natural UX. But CF overwrites previous inputs, and there is no solution that I’m aware of for this. Even the “Save to spreadsheet” feature only saves the last message.

In our last experiment, 48% of people shared their thoughts *without* clicking the “Share Thoughts” button. So if you’re only capturing based on that action, you’re missing half the insights.
<br>
<br>

## How building the FB App solves these problems:
<br>

The FB App that I built connects to your Messenger, then you subscribe it to specific Messenger webhooks. After this, you start receiving on your server everything that happens in your bot with timestamps for every action in real-time.

Here is what you can receive from the “messages” webhook:

- user id (that conveniently matches with Chatfuel’s one),
- texts that you send to the user, with read event as a separate webhook
- messages sent by the user,
- flag if this is the new user with ad_title and ad_id
- button names clicked by the user.


<figure>

![alt](/images/fb_app_raw_data.png)

<figcaption><h4>an example of the raw data with a dummy user</h4></figcaption></figure>

Here is the screenshot of the table in bigquery, which was optimized for our specific use case - to show all messages that we are receiving with timestamps and user ID.

<figure>

![alt](/images/fb_app_bq_data.png)

<figcaption><h4>an example of the BigQuery data</h4></figcaption></figure>
<br>
<br>

## App architecture:
<br>
<figure>

![alt](/images/fb_app_architecture.png)

</figure>

At a glance, this schema shows that we’ve fully eliminated our dependency on Chatfuel data for tracking user interactions. That said, it doesn’t mean we’ll stop storing or pulling some attributes from CF. We’ll still use it to filter users for broadcasts, but the number of attributes we rely on will drop significantly.

The remaining CF attributes can be pulled easily through our existing Airflow pipeline. We just need a few small tweaks in the data pulling logic and a new table at the dbt level.

The total cost of infrastructure is $32 per month, including server for $24 and GCP for $8
<br>
<br>

## The impact on the workflow for the whole company:
<br>

1. <b>We have a reliable data source.</b>

CF can change their database schema anytime without notice; they can stop us from querying your userbase by old attributes because we had too many attributes. I’m assuming that FB can change the schema as well, but they usually provide changelogs and developer notices.

2. <b>No more JSON calls for every action.</b>

I know that some parts of our umbrella org using JSON API call from CF to simulate user actions, but with this pipeline and app in place, bot builders don’t need to do this, because now you have the event data in the most reliable way.

3. <b>Data engineers are happy.</b>

Because they don’t need to fight with CF for the user data anymore. I mean, we still need to pull the attributes for the new user from Chatfuel if we label them in specific way based on the ad, but it is a simpler problem comparing to not having the events at all.

4. <b>Analysts working with real data.</b>

Set up BigQuery mart tables based on the raw event stream. Analysts get clean, structured data and build the dashboards faster.

5. <b>Real-time-ish insights for stakeholders.</b>

I didn’t implement it, but at a high level we can send the data from the FB App to Kafka topics, process it in real time with Spark with some transformation and cleaning, and write the data to the Data Warehouse, add a layer of simple queries for your main KPIs and voila, now we can make informed decision.

6. <b>Enables ML in the future.</b>

We could layer sentiment, clustering, or behavior prediction models on top of this data.

_____________________________________________________________________________________________________________

## Aftermath
I like how my solution to the real-world problem evolved from a Chrome extension to a data pipeline with simulation of the event-based data, and finally to a data pipeline with an FB App and real events. This project dramatically improved my resilience, because the main problem was to go through the process of the app review 7 times before I finally received approval. And this could be a skill issue, but you can search "how to send server to server FB app for a review" on Reddit and dive deeper into this painful world.
