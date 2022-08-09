---
title: "What is a Message Queue and Where is it used?"
categories:
  - Tech
tags:
  - System-design
  - message queue
---
![image-center]({{ site.url }}{{ site.baseurl }}/assets/images/message-queue/20210711203343.png){: .align-center}

Intro: there are shop 1,2,3,4 and each shop has ongoing orders. Shop number 4 is have power outage. In image there are service who ask (hearbeat) to 4 shop each 15 sec if there are alive/not

#### What
Messaging Queues provide useful features such as persistence, routing and task management. 

### When
A system having a message queue can move to higher level requirements while abstracting implementation details of message delivery and event handling to the messaging queue.
