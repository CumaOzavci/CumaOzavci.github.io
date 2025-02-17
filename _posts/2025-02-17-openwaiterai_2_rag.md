---
layout: post
title:  "OpenWaiterAI Devlog #2 Retrieval Augmented Generation"
date:   2025-02-17 12:00:00 +0300
categories: ai openwaiterai rag
excerpt_separator: <!--more-->
---
In this post, we will look at the retrieval augmented generation system of OpenWaiterAI. OpenWaiterAI should know its restaurant. It should know working hours, table counts etc. Also it should know items in the menu, their ingredients and possible allergens. It should be able to query erfficiently these datas. 

Since this is a standalone project, i created everything from stratch. I tried to choose best database and data structure for llm performance. After long discussions with ChatGPT O1, i decided to use PostgreSQL with simple table schemas. Thats because current llms are good at writing complex SQL queries. And by doing so, we will be able to reduce our tool count and increase llm performance on long conversations.

I did not use JSON in PostgreSQL for better querying performance. You can find current table schemas in below diagram:

![PostgreSQL Table Scemas](/pictures/sql_schema.png)