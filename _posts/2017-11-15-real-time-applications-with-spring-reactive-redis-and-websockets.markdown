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

## What is Redis

## What is a WebSocket

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
