---
layout: post
title:  "GSoC 2017 Final Report"
date:   2017-08-28 11:28:00
categories: gsoc2017
comments: true
---

## Introduction
The last week of GSoC is approaching. We have managed to achieve a lot, but there is still room for improvements and new technologies that need a try. In this blog post, I will quickly summarize what we have done so far, what I particularly have and what can be improved/explored more.

## What was done
Overall, the biggest new is that we completely managed to remove JMS and use Apache Kafka instead of it. That was our main goal when we started the project. Goal set, goal met :smiley: If a push message is sent via UPS REST API, a Kafka producer handles it and adds it to a topic. A Kafka stream is initialized based on this topic. It processes the push messages to 6 different output topics based on its variant type (Android, iOS, Windows and etc.). These messages go through further complicated processing which includes different Kafka consumers and producers until final push message sending is triggered.

But not only push sending part can move to Kafka, collecting of push metrics is also meant to be implemented with Kafka. We have already started by having 
* a topic which stores invalid tokens and a consumer that reads from it and deletes them 
* a topic that stores if a push message was successfully sent
* a topic that stores if a push message for specific iOS token was accepted by APNS

As a continuation of GSoC, more metrics using Apache Kafka can be collected.
Hopefully, one day - sooner or later, our GSoC Kafka POC will be part of the master branch.

## UnifiedPush Server pull requests

* [AGPUSH-2102](https://issues.jboss.org/browse/AGPUSH-2102) First Kafka Consumer usage in UPS - added a consumer which reads from "installationMetrics" topic and updates metrics [#853](https://github.com/aerogear/aerogear-unifiedpush-server/pull/853)
* [AGPUSH-2105](https://issues.jboss.org/browse/AGPUSH-2105) Research Dependency Inject for Kafka Consumers and Producers 
* [AGPUSH-2120](https://issues.jboss.org/browse/AGPUSH-2120) Add Kafka CDI library dependency [#855](https://github.com/aerogear/aerogear-unifiedpush-server/pull/855) 
* [AGPUSH-2152](https://issues.jboss.org/browse/AGPUSH-2152) Add sonarqube properties file [#865](https://github.com/aerogear/aerogear-unifiedpush-server/pull/865) 
* [AGPUSH-2109](https://issues.jboss.org/browse/AGPUSH-2109) Research Kafka Security [AGPUSH-2144](https://issues.jboss.org/browse/AGPUSH-2144), [AGPUSH-2143](https://issues.jboss.org/browse/AGPUSH-2143), [AGPUSH-2142](https://issues.jboss.org/browse/AGPUSH-2142)
and [AGPUSH-2141](https://issues.jboss.org/browse/AGPUSH-2141)
* [AGPUSH-2154](https://issues.jboss.org/browse/AGPUSH-2154) Add instructions to Readme how to run Jacoco. [#866](https://github.com/aerogear/aerogear-unifiedpush-server/pull/866) 
* [AGPUSH-2125](https://issues.jboss.org/browse/AGPUSH-2125) Add Installation Metrics Consumer injection [#876](https://github.com/aerogear/aerogear-unifiedpush-server/pull/876) 
* [AGPUSH-2178](https://issues.jboss.org/browse/AGPUSH-2178) Remove @KafkaConfig annotation in the jaxrs module [#888](https://github.com/aerogear/aerogear-unifiedpush-server/pull/888) 
* [AGPUSH-2176](https://issues.jboss.org/browse/AGPUSH-2176) Test environment setup for Kafka consumers/producers [#895](https://github.com/aerogear/aerogear-unifiedpush-server/pull/895) 
* [AGPUSH-2159](https://issues.jboss.org/browse/AGPUSH-2159) Create Push Notification Sender Kafka Producer [#896](https://github.com/aerogear/aerogear-unifiedpush-server/pull/896) 
* Remove *.orig files [#883](https://github.com/aerogear/aerogear-unifiedpush-server/pull/883)
* [AGPUSH-2155](https://issues.jboss.org/browse/AGPUSH-2155) Check id before parsing the page size [#867](https://github.com/aerogear/aerogear-unifiedpush-server/pull/867) 
* [AGPUSH-2190](https://issues.jboss.org/browse/AGPUSH-2190) Remove Kafka module [commit](https://github.com/polinankoleva/aerogear-unifiedpush-server/commit/f1a18abf7bde7c0f9194a45e0978bdc2f2162b5d) (decided not to be merged)
* [AGPUSH-2167](https://issues.jboss.org/browse/AGPUSH-2167) NotificationSenderCallback onSuccess/onError Topic [#903](https://github.com/aerogear/aerogear-unifiedpush-server/pull/903) 
* [AGPUSH-2186](https://issues.jboss.org/browse/AGPUSH-2186) Invalid Token Topic [#904](https://github.com/aerogear/aerogear-unifiedpush-server/pull/904) 
* [AGPUSH-2202](https://issues.jboss.org/browse/AGPUSH-2202) Update ReadMe after integration of Kafka [#913](https://github.com/aerogear/aerogear-unifiedpush-server/pull/913)
* [AGPUSH-2200](https://issues.jboss.org/browse/AGPUSH-2200) Generic pushMessage "success/failure" count with Kafka Streams [In Progress]


## Kafka CDI library pull requests
* Fix typos in the readme [#11](https://github.com/matzew/kafka-cdi/pull/11)
* Adding shutdown method to a consumer [#12](https://github.com/matzew/kafka-cdi/pull/12)
* Adding myself as a developer [#24](https://github.com/matzew/kafka-cdi/pull/24)

## Problems 
* [AGPUSH-2169](https://issues.jboss.org/browse/AGPUSH-2169) Producer null when @Producer detected before @KafkaConfig 
* [AGPUSH-2174](https://issues.jboss.org/browse/AGPUSH-2174) Scanning in Kafka Module doesn't work 
* [AGPUSH-2115](https://issues.jboss.org/browse/AGPUSH-2115) Resolve 'auto.commit.interval.ms' warning for Kafka producer configurations 
* [AGPUSH-2139](https://issues.jboss.org/browse/AGPUSH-2139) llegalArgumentException when using Kafka CDI library 
* [AGPUSH-2140](https://issues.jboss.org/browse/AGPUSH-2140) InstanceAlreadyExistsException for a Kafka consumer during redeployment 
* [AGPUSH-2145](https://issues.jboss.org/browse/AGPUSH-2145) ComponentIsStoppedException after redeployment 

## Improvements 
* [AGPUSH-2156](https://issues.jboss.org/browse/AGPUSH-2156) Rename HttpRequestUtil.extractSortingQueryParamValue method 
* [AGPUSH-2158](https://issues.jboss.org/browse/AGPUSH-2158) Update comments of findAllForPushApplication method 
* [AGPUSH-2161](https://issues.jboss.org/browse/AGPUSH-2161) Duplication of code in variantEndpoint classes 
* [AGPUSH-2162](https://issues.jboss.org/browse/AGPUSH-2162) Update link comment for "Message Format Specification" 
* [AGPUSH-2128](https://issues.jboss.org/browse/AGPUSH-2128) Return response even if an installation metrics producer hasn't finished 
* [AGPUSH-2151](https://issues.jboss.org/browse/AGPUSH-2151) MPNSPushNotificationSender - Use isEmpty() to check whether the collection is empty or not
* [AGPUSH-2185](https://issues.jboss.org/browse/AGPUSH-2185) Replace String.format in the log statements
* [AGPUSH-2195](https://issues.jboss.org/browse/AGPUSH-2195) Explore AdmPushNotificationSender errors per token
* [AGPUSH-2196](https://issues.jboss.org/browse/AGPUSH-2196) Catch NetworkIOException when sending push messages to Windows

## Other assignees' tasks 

* [AGPUSH-2163](https://issues.jboss.org/browse/AGPUSH-2163) Use Kafka Streams for processing of push messages 
* [AGPUSH-2164](https://issues.jboss.org/browse/AGPUSH-2164) Commit template not working 
* [AGPUSH-2165](https://issues.jboss.org/browse/AGPUSH-2165) TokenLoader replacement with Kafka 
* [AGPUSH-2166](https://issues.jboss.org/browse/AGPUSH-2166) Create consumer that will replace sendMessagesToPushNetwork method 
* [AGPUSH-2114](https://issues.jboss.org/browse/AGPUSH-2114) Set up test environment for Kafka 
* [AGPUSH-2131](https://issues.jboss.org/browse/AGPUSH-2131) Improve @KafkaConfig annotation
* [AGPUSH-2188](https://issues.jboss.org/browse/AGPUSH-2188) IOS Response Kafka Topic
* [AGPUSH-2189](https://issues.jboss.org/browse/AGPUSH-2189) Kafka Consumer that deletes invalid tokens
* [AGPUSH-2197](https://issues.jboss.org/browse/AGPUSH-2197) Check if wildfly full profile is needed
* [AGPUSH-2200](https://issues.jboss.org/browse/AGPUSH-2200) Generic pushMessage "success/failure" count with Kafka Streams

## Overall Stats
* We have 88 Jira Tasks created. 25 assigned to me, 25 to [Dimitra](https://github.com/dimitraz) and 11 to [Matthias](https://github.com/matzew).
* 49 of Jira Tasks were created by me, 23 by Dimitra and 13 by Matthias. 
* We did 37 pull requests to the UPS, 14 - mine, and 23 - Dimitra. 
* The GSoC branch is 60 commits ahead of the master
* We have 7 pull requests to [Kafka CDI](https://github.com/matzew/kafka-cdi), 3 - mine, 4 - Dimitra's 

## TODO List

There are three major topics that we only started but there are still things which can be added or improved. One of them is a stress/performance test of UPS after we replaced JMS with Kafka. In other words how the usage of Kafka improves the throughput. The second is the collection of push metrics - we did some initial tasks but we believe that more is yet to come. And the last one is Kafka Security. Its research was done and concrete Jira tasks were created. Though it was not the highest priority for us, there is no way GSoC POC project to be merged to master and used if security is not implemented.

Jira epics for these topics can be found here:
* [AGPUSH-2157](https://issues.jboss.org/browse/AGPUSH-2157) Kafka performance metrics
* [AGPUSH-2111](https://issues.jboss.org/browse/AGPUSH-2111) Push messages metrics
* [AGPUSH-2109](https://issues.jboss.org/browse/AGPUSH-2109) Kafka Security

## Future Work
Initially, when I applied for GSoC, integration of HBase was part of the plan. Unfortunately, we did not have the time to do it. HBase is the Hadoop database, a distributed, scalable, big data
store. It provides linear and modular scalability; strictly consistent reads and writes (no "eventually
consistent"); automatic and configurable sharding of tables; automatic failover support between RegionServers and easy-to-use Java API for client access. HBase can improve read/write access in the UPS, so we will leave it as an idea for GSoC Aerogear UnifiedPush Server 2018 :wink:

## Useful Links
* [GSoc 2017 Branch](https://github.com/aerogear/aerogear-unifiedpush-server/tree/GSOC_2017_kafka) and [commits](https://github.com/aerogear/aerogear-unifiedpush-server/commits/GSOC_2017_kafka)
* All [UPS PRs](https://github.com/aerogear/aerogear-unifiedpush-server/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Apolinankoleva%20) and [Kafka CDI PRs](https://github.com/matzew/kafka-cdi/pulls?utf8=%E2%9C%93&q=is%3Apr%20author%3Apolinankoleva%20) by me
* [GSoC 2017 Jira board](https://issues.jboss.org/browse/AGPUSH-2187?jql=labels%20%3D%20gsoc_2017)
* All mailing list updates: [1](http://lists.jboss.org/pipermail/aerogear-dev/2017-June/012887.html), [2](http://lists.jboss.org/pipermail/aerogear-dev/2017-June/012869.html), [3](http://lists.jboss.org/pipermail/aerogear-dev/2017-July/012894.html), [4](http://lists.jboss.org/pipermail/aerogear-dev/2017-July/012899.html), [5](http://lists.jboss.org/pipermail/aerogear-dev/2017-July/012904.html), [6](http://lists.jboss.org/pipermail/aerogear-dev/2017-July/012914.html), [7](http://lists.jboss.org/pipermail/aerogear-dev/2017-August/012934.html), [8](http://lists.jboss.org/pipermail/aerogear-dev/2017-August/012949.html) and [9](http://lists.jboss.org/pipermail/aerogear-dev/2017-August/012955.html)
* All my blog posts [1](https://polinankoleva.github.io/personal/2017/06/12/me-the-intro.html), [2](https://polinankoleva.github.io/education/2017/07/13/install-sonar-qube.html), [3](https://polinankoleva.github.io/gsoc2017/2017/07/16/GSOC-or-UPS.html) and [4](https://polinankoleva.github.io/gsoc2017/2017/07/17/SonarQube-And-UPS.html)
* [All Jira Tasks](https://issues.jboss.org/browse/AGPUSH-2197?jql=creator%20in%20(currentUser())%20AND%20labels%20%3D%20gsoc_2017) created by me
* [All Jira Tasks](https://issues.jboss.org/browse/AGPUSH-2194?jql=labels%20%3D%20gsoc_2017%20AND%20assignee%20in%20(currentUser())) assigned to me

Share your feedback and stay tuned for my next post. <br/>
**Do not overthink, be happy and just keep smiling...**
<br /> Polina
