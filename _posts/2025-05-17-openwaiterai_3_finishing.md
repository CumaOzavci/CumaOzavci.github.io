---
layout: post
title:  "OpenWaiterAI Devlog #3 Finishing Tools and System Message"
date:   2025-05-17 12:00:00 +0300
categories: ai openwaiterai rag
excerpt_separator: <!--more-->
---
In this post, we will finish basic tool setup and system instructions of `OpenWaiterAI`.

### Tools
With below tools `OpenWaiterAI` will be able to:
1. Know its restaurant and menu
2. Query menu
3. Call waiter
4. Ask questions to restaurant management
5. Keep track of order slip

<br />

#### 1. SQLQueryTool
This tool and its logic was described in [previous post](https://cumaozavci.github.io/ai/openwaiterai/rag/2025/02/17/openwaiterai_2_rag.html). You can check it for more details. While i did not change this tool, i added two new tables to database: `WaiterCalls` and `CustomerManagementQueries`. `WaiterCalls` table is for calling waiters to the table. LLM can pass a call reason too. `CustomerManagementQueries` table is for asking questions for restaurant management. Due to its logic, i created another tool for this.
<br />

![Waiter Call SQL Schema](/pictures/waiter_call_sql_schema.png){:class="img-responsive" width="50%"}
<br />
<!--more-->

#### 2. CustomerQueryTool
This tool submits a question into below table and waits for an answer. It checks answer every second for 30 seconds. If there are no answer in this time, it raises error. With this tool LLM will be able to redirect customer queries and ask questions itself. This will be usefull especially when restaurant database is not filled correctly.
<br />

![Customer Queries SQL Schema](/pictures/customer_queries_sql_schema.png.png){:class="img-responsive" width="50%"}
<br />

#### 3. SetOrderSlipTool
This tool helps customer visualize his/her order. Imagine, the device running `OpenWaiterAI` has a screen; LLM will update customers order slip when customer makes a change and when customer approves slip, it will be submitted into database.