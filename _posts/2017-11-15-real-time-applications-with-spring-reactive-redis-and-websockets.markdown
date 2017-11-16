---
layout: post
title: "Real-time app with Spring, Project Reactor, Redis, and Websockets"
description: "Let's use Spring and Project Reactor to create a real-time chat application."
date: 2017-11-15 10:03
category: Technical Guide, Java, Spring
author:
  name: "Bruno Krebs"
  url: "https://twitter.com/brunoskrebs"
  mail: "bruno.krebs@auth0.com"
  avatar: "https://www.gravatar.com/avatar/76ea40cbf67675babe924eecf167b9b8?s=60"
design:
  bg_color: "#1B1C20"
  image: https://cdn.auth0.com/blog/spring5-embedded/logo-spring-tomcat-gradle.png
tags:
- spring
- reactive
- redis
- websocket
- security
- real-time
- auth0
related:
- 2017-08-10-implementing-jwt-authentication-on-spring-boot
- 2017-04-28-incrementally-changing-your-database-with-java-and-flyway
---

**TL;DR:** In this article, we are going to use Spring, Project Reactor, Redis, and WebSocket to create a real-time chat application. Users are going to identify themselves through Auth0 and will be able to send messages to each other over a channel supported by Redis. You can find the complete app in [this GitHub repository](https://github.com/auth0-blog/spring-reactive-websocket).

## What is Project Reactor

As stated by [the official website](https://projectreactor.io/), Project Reactor is a fourth-generation Reactive library that helps developers build non-blocking apps on the Java Virtual Machine (JVM). Being a fourth-generation library means that Project Reactor has a fluent API that can interoperate with other Reactive libraries (such as [RxJava](https://github.com/ReactiveX/RxJava)) while handling backpressure and cancellation across the library boundaries. To learn more about the differences among all generations, take a look at [this article written by David Karnok](https://akarnokd.blogspot.com.br/2016/03/operator-fusion-part-1.html).

While non-blocking is a mainstream topic on other platforms like Node.js, Java developers are just starting to take this approach seriously. For example, only on its latest release, Spring (the most popular Java framework) [enabled developers to create for reactive HTTP  server](https://docs.spring.io/spring/docs/current/spring-framework-reference/web-reactive.html). However, as we will see, creating non-blocking code on Java is already easy.

## What is Redis

[Redis](https://redis.io/) is an open source project that can be used as a database, a cache engine, or as a message broker. In this article, we are going to use Redis to implement the [Publish/Subscribe messaging pattern](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern). To get messages in real-time, clients of our application will use a specific Redis channel to send and receive (publish and subscribe) messages. The process of exchanging messages will occur through WebSockets.

## What is WebSocket

[WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) is a protocol that provides two-way communication channels over TCP connections. Differently from HTTP, a WebSocket connection (or session) stays open as long as needed and accepts asynchronous messages from both ends. We will see this in action, but to better illustrate the concept, let's imagine that we have a web application that enables users to analyze logs from a server. If this application relied purely on HTTP connections, it would have to keep sending regular requests to get the latest logs. Besides not being a real-time communication, these requests could have no value at all, because they would end up as empty responses when no new logs were available.

However, if the web app was built upon the WebSocket protocol, there was no need to send any other request besides the [handshake](https://en.wikipedia.org/wiki/WebSocket#Protocol_handshake). Since the protocol leaves connections open as long as needed, the server could send new logs when they occur, in real-time.

Note that, although these protocols are different, [RFC 6455](https://tools.ietf.org/html/rfc6455) states that the WebSocket protocol is designed to work over HTTP ports 80 and 443 as well as to support HTTP proxies and intermediaries. That is, the WebSocket and the HTTP protocols are compatible.

## What Will We Build

![Real-time chat application written with Spring, Project Reactor, Redis, and WebSockets](https://cdn.auth0.com/blog/spring-reactive/real-time-chap.png)

### Creating an Auth0 Client

## Bootstrapping a Spring 5 App

### Adding the Dependencies

### Configuring Redis

### Mapping Messages

### Forwarding Messages

### Handling WebSockets Sessions

## Conclusion
