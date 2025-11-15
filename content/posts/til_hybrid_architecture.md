---
title: "TIL #3: The hybrid architecture"
date: 2025-11-16T01:10:03+06:00
showToc: true
TocOpen: false
draft: false
tags: ["TIL"]
---

Yesterday, one of my seniors, who is working as a senior ML engineer, approached me with a problem. He has started an e-commerce business where an AI model will help visitors make their decisions. As he has started the startup and is doing everything by himself, while hosting the project in a cloud server, he needs a cloud server with a GPU. The cost is the issue here. He approached me with the question: *Can we host an e-commerce application in a gaming laptop, with the AI model, while serving the users effectively?*

From the problem, I began to form a hypothesis:
1. As he mentioned about a *high-end gaming laptop*, he can use the GPU to run the model effectively.
2. We can host a web server on our computer and serve it to the open internet, but it needs some setup.

Serving a web server from a consumer computer for testing purposes is okay. But having 100 visitors visiting the website will have different outcomes. The problem he may run into:
- Consumer laptops aren't meant to be used 24/7. Laptops will have sleep/hibernate issues, sudden shutdown, etc.
- Thermal throttling = server load + background processes.
- Network reliability issues. A lot will depend on the ISP. As he is operating the whole process from his home, he will face network issues a lot.
- Storage reliability issues. Server-grade NVMes are totally different from consumer-grade SSDs. There are differences in write endurance and redundant backups. A laptop SSD will die faster in heavy writes.
- Security weakness, managing all of the firewall rules, and OS updates. Also, physical theft!
- A lot of heating issues.
- Last but not least, power outages.

I had a look at his website and noticed one crucial thing: the AI model helps users after they have selected a product. Based on that design, I came up with an idea I learned from backend coding: why not a pluggable architecture?

The AI model will run on the laptop.

The idea is simple. Let's host all the user-related things on the cloud server, as the cost is manageable. And, the AI model has some API endpoints, and he will run it from the laptop. If 100 requests hit the web server a day, only 10 will reach the AI model to try to sail the boat of e-commerce; that is enough for now.

This will cover:
- The storage of the most important data: users' data.
- The cloud server will handle all of the incoming requests. Only when the requests require the AI model will it hit the endpoints.
- A consumer-grade PC is capable enough to handle a low number of requests per day, which will reduce the heating and performance issues better compared to all of the requests hitting the laptop.
- To improve security, we can use tunnels. I mentioned using Cloudflare Tunnel, while he proposed Ngrok.
- He will have less worry by handling and monitoring the AI model only.
- As the project is in the MVP phase, some amount of downtime of the AI model can be accepted.
- Lastly, I suggested to him to move from Windows to Linux, which will give him less headache (*yeah, I know, it's a typical Linux enthusiast thing, but you can't think otherwise. Let's call our soldiers: oh my boy, Docker, oh my boy C++ libraries...*)

He liked the architecture idea and labeled it as efficient thinking. At first, I gave this a name: **Pluggable architecture**; later, I found out it has a name. Enter...

### Hybrid Architecture

**Hybrid architecture** is an engineering approach that combines two or more different types of systems or paradigms to create a more flexible, efficient, and scalable solution (*don't worry, I copied it from the internet*). A common example is one that I have already talked about: integrating on-premises architecture with public cloud services, allowing data and applications to move between them.

That's all. I am lazy. Read more about hybrid architecture on the internet. I will update it when I feel like it. Oka bye.
- - -
Are you still here? Well, okay, I haven't dived deep enough to explain in easy terms, nor do I have the articulation, aka technical terms. This post will be updated.

And if you are a senior system architect, thanks for reaching the end. I still have many problems to solve in this scenario, for example, I didn't raise some questions. For example, my proposal is based on assumptions; I need proper metrics to measure this solution, which I addressed to him. Also, I didn't deep dive enough for the request pattern; one of the questions in this category might be, what if 10 requests come within the same 5 seconds? And what to do if an actual burst of traffic happens? The solution of tunnels is questionable, too. What would the latency look like? And, I didn't think about the solution of network outages, which is important than ever.

So, I will update this post addressing all of the issues.