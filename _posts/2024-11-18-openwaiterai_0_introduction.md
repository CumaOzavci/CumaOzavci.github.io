---
layout: post
title:  "OpenWaiterAI Devlog #0 Introduction"
date:   2024-11-18 12:00:00 +0300
categories: ai openwaiterai
excerpt_separator: <!--more-->
---

Recent advancements in Large Language Models (LLMs) have unlocked new opportunities across various fields. These AI systems are reshaping industries by improving efficiency, creativity, and problem-solving. In particular, the HoReCa sector stands to benefit greatly, as LLMs streamline customer service, optimize operations, and enhance personalized guest experiences.

LLMs have the potential to transform existing technologies in restaurants. Current self-service kiosks, which function as large tablets for placing orders, could evolve into interactive virtual cashiers powered by LLMs, offering conversational assistance and personalized recommendations. Similarly, service robots, which currently transport food and dirty dishes between the kitchen and tables, could take on more advanced roles. With LLM integration, these robots could greet customers, escort them to their tables, take orders, and even measure customer satisfaction through natural interactions. Lastly, LLMs could revolutionize phone systems, eliminating the need for human operators by seamlessly managing reservations and orders via human-like conversations.

<br/>
<blockquote class="reddit-embed-bq" style="height:500px" data-embed-height="740"><a href="https://www.reddit.com/r/shittyrobots/comments/18ike34/honestly_might_be_an_upgrade_nah/">Honestly, might be an upgrade? Nah!</a><br> by<a href="https://www.reddit.com/user/Myco_Cube/">u/Myco_Cube</a> in<a href="https://www.reddit.com/r/shittyrobots/">shittyrobots</a></blockquote><script async="" src="https://embed.reddit.com/widgets.js" charset="UTF-8"></script>
<br/>

<!--more-->

In their current state, service robots in restaurants are efficient but limited in capability. They can only transport food and dishes. As seen in the video, these robots cannot respond to simple customer requests, such as bringing a glass of water. While useful for basic tasks, they lack the adaptability and interactivity needed to provide a truly exceptional dining experience.

By integrating LLM technology, these robots could transcend their physical limitations. Although they still wouldn’t have arms, they could use advanced conversational AI to coordinate with human staff, ensuring tables are promptly cleaned and requests are addressed. **These robots could actively take orders directly from customers, recommending dishes based on their preferences or dietary restrictions, and even suggesting dessert options. Instead of merely delivering food, they could confirm orders, handle modifications, and seamlessly relay the information to <u>the kitchen</u>. With LLMs, service robots would not only engage with customers but also personalize the entire ordering process, creating a dining experience that feels both intuitive and futuristic.**

<br/>
![OpenWaiter](/pictures/open_waiter.png){:class="img-responsive"}
<br/><br/>
Now imagine a compact device, similar to Amazon Alexa, placed on every restaurant table, revolutionizing customer interaction. These table-top assistants, powered by LLM technology, could handle a wide range of tasks with ease. Diners could place orders directly through the device, inquire about the status of their meals, summon a server when needed, and even complete payments—all through natural, conversational interactions. Beyond practicality, these assistants could enhance the dining experience by providing personalized recommendations, answering questions about menu items, or even entertaining guests with trivia or stories about the dishes. By seamlessly integrating convenience and engagement, these devices could redefine the way customers interact with restaurant services.
<br/><br/>

The hospitality sector is ripe for transformation through AI-driven solutions. While many concepts and devices exist, such as kiosks, service robots, and table-top assistants, a unified platform to power them all is missing. This is where **OpenWaiterAI** comes in—a project I am actively working on to bridge this gap.

**OpenWaiterAI** is an open-source platform designed to integrate seamlessly with various restaurant technologies, turning them into intelligent, interactive tools. It will empower self-service kiosks to become virtual cashiers, enable robots to engage with customers conversationally, and enhance table assistants to handle tasks like order-taking and payment processing. Crucially, it will be legally open for commercial use, ensuring accessibility for businesses of all sizes. This project marks the beginning of a new era for smarter, AI-powered dining experiences.
<br/><br/>

---
<br/>

Now, let's put aside the introduction written by ChatGPT and talk about some technical stuff.

**OpenWaiterAI** is a wrapper for LLM APIs. It uses Langchain (for now). So that, it will be compatible with almost all LLMs.
With function calling, it can interact with restaurant pos systems. It can retrieve the menu, answer questions about items. It can enter orders and later check order status. It will try to sell more items (upsell&crosssell) etc. Furthermore, it will talk to customers like a **real waiters** not call center robots.

This devlog series, will focus on development of OpenWaiterAI. But can be used for other domain specific agentic llm workflows too. Firstly we will design a flexible but robust system. Then, we will code it. After we have a working demo, we will create a detailed benchmarking system. Once we have this benchmark system working, we will be able to close the development loop!

After releasing the first stable version of **OpenWaiterAI**, i plan to start an open hardware project for a table-top assistant called **OpenWaiter**!

**OpenWaiterAI** and **OpenWaiter** will be free for everyone. <u>The only exception to this is Saha Robotik, they are completely banned from using them.</u>