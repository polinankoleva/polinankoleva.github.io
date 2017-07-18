---
layout: post
title:  "GSoC or Aerogear UPS project"
date:   2017-07-16 11:27:00
categories: gsoc2017
comments: true
---

It is a little bit late for this article, but I like saying "It is better late than never". So trying to have a productive Sunday, after a complete change of the blog's UI 

> I am not created for front-end development as you can see :laughing:

I have found time to return to the beginning and explain about the project I am working on. Finally, girl!!! :fireworks: :fireworks: :fireworks:

## Aerogear Unified Push Server
[Aerogear](https://aerogear.org/) provides flexible, extensible libraries and server side components that will simplify your mobile development and infrastructure setup across platforms. It provides different modules, but what concerns me the most is [AerogearPush](https://aerogear.org/push/) module. <br/>
This module provides the functionality for sending push notifications to any device, regardless of platform or network. A part of it is [AeroGear UnifiedPush Server (UPS)](https://aerogear.org/push/#unifiedpush) which sends native push notifications to different mobile operating systems such as Android, iOS and others. I and [Dimitra](https://dimitraz.github.io/blog/) as participants in GSoC are working on this server. 

## GSoC Goal

The AeroGear GSoC project is about extending the capabilities of AeroGear UnifiedPush Server by improvement of its scalability and performance. This will make processing of data and sending of push notifications fast regardless of the data volume the server operates over. Two technologies are considered as the most valuable in terms of the business logic and needs of AeroGear UnifiedPush Server. The first is the usage of distributing streaming platform Apache Kafka which can be seen as  “replacement” of JMS subsystem in the server. The second is a replacement of MySQL with a distributed, scalable, big data store HBase. For now, we are concentrated more on Apache Kafka. If the time is enough when Apache Kafka is integrated and tested, we can start researching HBase.

The “sender” module of UPS is based on message queues. For example, when a request for sending push notifications comes, `NotificationRouter` fires an event. This event is handled by `MessageHolderWithVariantProducer` which depending on the `variantType` uses one  `<Network>PushMessageQueue`. On the other side, we have `MessageHolderWithVariantsConsumers` which consume each `MessageHolderWithVariants` placed in a queue and fire another event. In such manner, all processes for sending of push notifications and collecting of push metrics are executed. Unfortunately, under high load and lots of metrics, the current implementation is not best for scaling. Therefore, as a possible solution, a replacement of JMS with Apache Kafka is considered.

Apache Kafka is a distributed streaming platform. It has three key capabilities:
* lets you publish and subscribe to streams of records (similar to a message queue) - exactly what we are looking for
* stores streams of records in a fault-tolerant way (replication of the data)
* processes streams of records as they occur

Basic concept of the platform is that it is run as a cluster on one or more server. The terminology in Kafka differs from JMS but the ideas are identical. Instead of a queue, Kafka has an abstraction called **“topic”**. A topic is a category to which records are published. Each record in a topic has a key, a value and a timestamp. Each topic has a partitioned log. This leads to increase of scalability in terms of topics - each topic can go beyond the size that fits on a single machine. The partitions for a topic are not only distributed, but also replicated. For each partition, Kafka has a server which acts as a leader and zero or more followers which passively replicate the leader. If the leader fails, one of the followers will automatically become a new leader for this partition. So it guarantees that for a topic with replication factor N, it will tolerate up to N-1 fails without losing any committed data. 

In terms of implementation, Kafka provides four different APIs. 
* **Producer API** which allows to public records into one or more topics
* **Consumer API** which allows  subscribing to a topic and process records published to it
* **Streams API** which allows to an application to run as stream processor
* **Connector API** that serves as a factory of consumers and producers and connects topics to different applications

<p>In comparison to traditional messaging systems, Kafka has a lot of advantages. Traditional messaging systems have two models - publish/subscribe and queueing. Kafka manages to generalize and combine them by unique concepts of a consumer group. Therefore, the advantage of Kafka's model is that every topic implements both models - it can scale processing and it is also multi-subscriber. 
Additionally, Kafka is a very good storage system -  you can think of it as kind of special purpose distributed filesystem dedicated to high-performance, low-latency commit log storage, replication, and propagation.</p>
<p> To conclude, in comparison to most messaging systems Kafka has better throughput, built-in partitioning, replication, and fault-tolerance which makes it a good solution for large scale message processing applications.  </p>

## Progress so far
I will try to keep it short. If you want to find out more, you can read [Dimitra's blog post](https://dimitraz.github.io/blog/post/welcome/).
<p>ALREADY DONE:</p>
* we both created blogs and we try to share our experience
* first Kafka producer and consumer are already integrated and working
* documentation how to use UPS with Kafka or Kafka on docker is ready
* a library for [Kafka CDI](https://github.com/matzew/kafka-cdi) is chosen 
* first injected producer and consumer are ready
* some fixes to the Kafka CDI library are committed [consumer shutdown](https://github.com/matzew/kafka-cdi/pull/12) 

<p>IN PROGRESS</p>
* Kafka Security is in progress - the topic is divided into smaller tasks so that we have better understanding what has to be done and how much time it will take approximately. But it has  a minor priority at the moment
* Kafka on OpenShift 
* problems with the lifecycle of Kafka components when Kafka CDI library is used have arisen. So this fix is the highest priority right now. More about them can be found [here](http://aerogear-dev.1069024.n5.nabble.com/aerogear-dev-Problems-when-injecting-a-Kafka-Consumer-td13050.html). 
* Aerogear UPS metrics before and after - mainly two tools are considered [SonarQube](https://www.sonarqube.org/)
and [Structure 101](http://structure101.com/). The idea is to generate reports about UPS before and after the integration of Kafka.

 <p>WHAT'S NEXT:</p>
 * create a kafka module in the project
 * design the sender/jaxr module with Kafka - architectural level
 * start slowly replacing JMS with Kafka in the sender module
 * Kafka Streams - explore we can use it in the UPS

If you want to explore the code of the project - check out [UPS kafka branch](https://github.com/aerogear/aerogear-unifiedpush-server/tree/GSOC_2017_kafka). <br/>
If you want to learn more about our Jira board - check out [GSoC Board](https://issues.jboss.org/secure/RapidBoard.jspa?rapidView=4022&view=planning.nodetail&selectedIssue=AGPUSH-2103&versions=visible).

## Try Open Source

I know that usually when you have a 5-to-9 job, it is really hard to find spare time to contribute to an open source project. Because you are tired, you have friends, you have family and a list of TODO things that you are always postponing, but just try once. I hope I will find time even after the program finishes. Such experience can for sure makes you a better developer. None the less, it can motivate and challenge you if you are bored at work (sometimes it happens)


Share your feedback and stay tuned for my next post. <br/>
**Do not overthink, be happy and just keep smiling...**
<br /> Polina
