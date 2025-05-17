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

### System Instructions
To be honest, i was afraid of this section. Because, making an LLM behave like an elite waiter while maintaining all the tool logic is hard, really hard. This was maybe the hardest task in my previous projects. But now I have an advantage that I didn't have before: `Google's AI Studio with Gemini 2.5 Pro`. With its massive context window and multimodal capabilites, `Gemini 2.5 Pro` can process very diverse set of data with very high intellect. It can even process Youtube videos. And the best part is, it's all free!

Firstly, i found a [good Youtube video](https://www.youtube.com/watch?v=S1CfItpKg7c). It was a training module for a restaurant service system called `ABCDXO`. I didn't even know this system existed! I just put most watched video on Youtube into `AI Studio`. Then i asked `Gemini 2.5 Pro` to extract breakdown of the key informations in that video. Here is the <ins>summary</ins> of `ABCDXO` system:

```
The ABCDXO System is a comprehensive restaurant service philosophy
designed to elevate servers from mere order-takers to proactive
"salespeople" by encouraging them to go
"Above and Beyond the Call of Duty" (ABCD) and add "Hugs and Kisses" (XO)
– those extra thoughtful touches. It utilizes a customized
XO Order Pad and descriptive inserts to guide servers through
a structured yet personalized service journey. This journey
emphasizes warmly greeting guests, building rapport, confidently
suggesting appealing non-alcoholic beverages, appetizers, entrees,
and desserts, and providing attentive ongoing care like quality checks
and "Clearing, Crumbing, & Replenishing" (CCR). The system's goal is
to create a "Total Guest Experience" that not only delights customers
and fosters loyalty but also increases sales, server tips, and overall
job satisfaction by ensuring every guest interaction
is positive and exceeds expectations.
```

Then i put key informations of `ABCDXO` system and `OpenWaiterAI` code into `AI Studio` and asked `Gemini 2.5 Pro` to create system message for me. After a few feedbacks, `Gemini 2.5 Pro` gave me the perfect system message!

If you want to change `OpenWaiterAI`'s behaviour, you can follow similar path: put current system message into `AI Studio` and ask `Gemini 2.5 Pro` to make the changes you want. You can even start from stracth. For example, if you have a book or another video, or both; just put them into `AI Studio` and use `Gemini 2.5 Pro` to create your own system message.

With these tools and system message `OpenWaiterAI` is now capable of serving customers. You can find repo in this [link](https://github.com/CumaOzavci/openwaiterai). In the next post, we will connect multiple `OpenWaiterAI` instances to database and create a simple manager for them.