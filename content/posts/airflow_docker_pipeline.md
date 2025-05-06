---
title: "Data Pipeline with Airflow and Docker. Work Project."
date: 2024-12-15
slug: "pipeline_airflow_docker"
description: "This capstone project was developed as part of the Data Engineering BootCamp I attended in summer 2024. It directly addresses a real-life challenge I encountered in my current role - automating the data pipeline to efficiently process and transform raw Chatfuel data."
keywords: ["python", "airflow", "dbt", "GCP"]
draft: false
tags: ["Python", "Airflow", "dbt", "Docker", "GCP"]
summary: This project directly addresses a real-life challenge I encountered in my current role - automating the data pipeline to efficiently process and transform raw Chatfuel data.
toc: true
---

## Summary:
This capstone project was developed as part of the Data Engineering BootCamp I attended in summer 2024. It directly addresses a real-life challenge I encountered in my current job - automating the data pipeline to efficiently process and transform raw Chatfuel data. By reverse-engineering a non-standard API, simulating event-based data, and implementing robust data transformations, I built a scalable solution that delivers actionable insights daily.

## Tools, I used:
Python, Airflow, Docker cantainer on a remote Ubuntu server, dbt, BigQuery, Looker.

_____________________________________________________________________________________________


During the summer 2024 Data Engineering BootCamp at DataExpert.io, I had the opportunity to tackle a challenging problem that I face in my current role. This project was not only a learning experience but also a practical solution to streamline the messy and manual process of handling raw Chatfuel data.
FYI: chatfuel is a no-code platform used for building Facebook Messenger bots to send weekly content and interact with subscribers.
Chatfuel provides a visual interface for filtering user base attributes, but it is very slow and makes thorough analysis difficult.


## Key parts of the project
<br>

1. **Advanced data extraction.**

    Chatfuel has a total of 3 public APIs:
   - GraphQL without an introspection possibility
   - JSON API without useful entry points for my task
   - another API is for the export of the userbase, but this request is very slow and doesn't send full data, so it is useless for the analysis.

    GraphQL API was made to support the visual interface of the platform and of course, there is no public documentation for it.
    Therefore, my only path was to reverse-engineer requests to their GraphQL database (thanks to Google Chrome for DevTools and Inspect!).

    But just making a request was not enough, you can read more about that adventure [here](https://github.com/mariavyso/dataexpert_capstone_project?tab=readme-ov-file#scope-of-the-project).

2. **Simulating event-based data.**

    Chatfuel doesn't share with us the event-based data (I know that they have it, though).
    To overcome this, I have decided to simulate event-based data myself. Because users interact with us only when we send them something, which happens 2-3 times per week, I can create an incremental table and write users’ attributes as an event, with the date of this event being the date when I pulled this data from Chatfuel. This approach wouldn’t allow me to use this event date in an incremental strategy, but it would create a historical data point that could be used in further analysis.

3. **Huge transformation pipeline with DBT and BigQuery.**

    Once the raw data was ingested into BigQuery and Gogle Cloud Storage, I used dbt to perform incremental transformations. You can read about it more [here](https://github.com/mariavyso/dataexpert_capstone_project?tab=readme-ov-file#data-transformation-with-dbt-in-bigquery).

    And I leveraged medallion architecture, so my end tables after all transformations are business-ready. Plus, I added the tests layer for all tables to keep the data consistent.

4. **Orchestration with Airflow.**

    I'm using Airflow in Docker container on Ubuntu server to orchestrate the entire pipeline. This automation covers data collection, transformation, and even error handling. The system is scheduled to run daily after midnight, and as a final step I set up automation to update the Looker dashboard with the fresh data, that visualizes key performance indicators and trends.

5. **I dived deeper into [possible scaling opportunities](https://github.com/mariavyso/dataexpert_capstone_project?tab=readme-ov-file#alternatives-and-scaling-opportunities)**, but in the end, I built a better solution for handling the data from Chatfuel, but it is for another story.

    *(Spoiler: I built my own FB App!)*


For a more detailed description of the project’s architecture, transformation steps, and the code itself, you can dive into the [repository on GitHub](https://github.com/mariavyso/dataexpert_capstone_project). Of course some parts of it were changed and I added more tasks into the Airflow, which weren't added to public repo, but it will give you a taste of it, at least.

_____________________________________________________________________________________________________________

## Aftermath
I really enjoyed working on this project. I gained deeper knowledge about tools and gained hands-on experience in delivering such a pipeline, being the only data engineer on the team in a fast-paced environment.
