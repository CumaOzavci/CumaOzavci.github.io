---
layout: post
title:  "OpenWaiterAI Devlog #2 Retrieval Augmented Generation"
date:   2025-02-17 12:00:00 +0300
categories: ai openwaiterai rag
excerpt_separator: <!--more-->
---
In this post, we will look at the retrieval augmented generation system of `OpenWaiterAI`. `OpenWaiterAI` should know its restaurant. It should know working hours, table counts etc. Also it should know items in the menu, their ingredients and possible allergens. It should be able to query erfficiently these datas.

Since this is a standalone project, i created everything from stratch. I tried to choose best database and data structure for llm performance. After long discussions with ChatGPT O1, i decided to use PostgreSQL with simple table schemas. Thats because current llms are good at writing complex SQL queries. And by doing so, we will be able to reduce our tool count and increase llm performance on long conversations.

I did not use JSON in PostgreSQL for better querying performance. You can find current table schemas in below diagram:

![PostgreSQL Table Schemas](/pictures/sql_schema.png)
<!--more-->

With above table structure, `OpenWaiterAI` should be able to answer complex queries like `I'm trying to build muscle these days. I need to eat something high in protein but low in carbohydrates. I also have a gluten allergy. What do you recommend from your menu?`. After receiving request, it should write an SQL query to filter foods that do not have gluten and sort them with respect to their nutritional values. Finally, give a good answer to customer.

I used [Tavuk Dünyası](https://www.tavukdunyasi.com/tr-en/) menu in this project. That is because i am familiar with their menu and their official website has very detailed information about their products. I used ChatGPT 4o with search capabilities to scrape their website and filled database with them.

You can find a working PostgreSQL database in [OpenWaiterAI DB repo](https://github.com/CumaOzavci/openwaiterai_db). Just follow instructions. Also you can find parsed Tavuk Dünyası items and some helper python scripts in this repo. You can use them anywhere you want.

#### Caching
Giving `OpenWaiterAI` a tool to query PostgreSQL database might sound enough but this approach is prone to errors because it does not have any idea of database structure, schema or any relations beforehand. When it receives a query, it will try to get table names first. Then it will get schemas of tables that it found relevant and finally write correct SQL query. This will result in multiple tool calls and noticable latency.

I have found that by giving a carefully prepared summary of database, we can increase llm performance a lot. Although this approach lengthens the system message, llm will make fewer API calls and give a more stable latency because it simplifies function calls.

![System Message Structure](/pictures/system_message_structure.png)

As seen above, i added restaurant, menu and schema descriptions to system message. The restaurant description includes the contents of the `summary` category in the `RestaurantInfo` table and the names of other restaurant info categories. The menu description includes the categories in the menu, the products in these categories and their IDs. The schema description includes the tables in the database and their schemas. In this way, `OpenWaiterAI` can reduce function calls and provide better answers to the user without increasing the number of tokens too much.
