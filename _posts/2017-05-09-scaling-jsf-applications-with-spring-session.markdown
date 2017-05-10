---
layout: post
title: "Scaling JSF Applications with Spring Session"
description: "JSF is heavily dependent on HTTP sessions, which is hard to scale it horizontally. Today we will see how can we do it with the help of Spring Session."
date: 2017-05-09 08:00
category: Technical Guide, Java, Spring Boot
author:
  name: "Bruno Krebs"
  url: "https://twitter.com/brunoskrebs"
  mail: "bruno.krebs@auth0.com"
  avatar: "https://www.gravatar.com/avatar/76ea40cbf67675babe924eecf167b9b8?s=60"
design:
  bg_color: "#2E668D"
  image: "https://cdn2.auth0.com/blog/boot-faces/jsf-logo.png"
tags:
- spring-boot
- jsf
- java
related:
- 2017-05-09-developing-jsf-applications-with-spring-boot
- 2017-04-28-incrementally-changing-your-database-with-java-and-flyway
- 2016-09-20-securing-spring-boot-with-jwts
---

**TL;DR** Spring is one of the most popular and reliable frameworks available for Java developers. Upon its core, many modules were implemented, like Spring MVC, Spring Security, Spring Data, and so on. In this article we will see how to use Spring Session (one of these Spring modules) to scale JavaServer Faces (JSF) applications horizontally.

## Horizontal vs Vertical Scaling

Before diving into how we use Spring Session to scale JavaServer Faces (JSF) applications, let's do a quick review on the differences between scaling applications vertically and horizontally.

Scaling an application vertically simply means adding more power to the machine that hosts the application. For example, let's say that we have an application running on a `t2.medium` instance on AWS (which has 2 virtual cores and 4GiBs of RAM) that is having a hard time to process all requests. To scale this application vertically would mean choosing a more powerful instance, with more resources, to host it (e.g. using a `t2.xlarge` instance that has 4 virtual cores and 16GiBs of RAM).

This approach has one good advantage but three extremely important disadvantages. The upside is that it is *very* easy to scale it. It's just a matter of deploying the application to the new instance and we are good to go. The downside list is:

1. It is quite expensive to keep upgrading the instance type, no matter where we are hosting it.
2. There is a limit on how much power you can get from a single host.
3. The host that supports our application is a single point of failure. This means that if it goes down our users won't have access to our application.

Horizontal scalability, on the other hand, means adding more hosts to support an application. In the example above, instead of choosing a new instance type, we could create a second `t2.medium` instance and [load balance requests](https://en.wikipedia.org/wiki/Load_balancing_(computing)) between them.

The biggest disadvantage of this approach is that the complexity of scaling an application is higher, when compared to simply deploying an application on a more powerful machine. But, the advantages are:

1. We can scale it infinitely.
2. It is cheaper to add a second machine rather than replacing for one with twice the power.
3. We have no single point of failure.

As we could see, although scaling applications vertically can be easier, scaling them horizontally is much more interesting and improves the reliability of our system. That's why in this article we will see how we can use Spring Session to help us scaling JSF applications horizontally.

## What is Spring Session

Spring Session is a module of the Spring Framework that aims on providing a common infrastructure for managing clustered sessions. This means that Spring Session abstracts the complexity of clustering HTTP sessions, making it easy to host applications scaled horizontally. Basically, what this modules does it to offload the storage of the session state into  external session stores (e.g. Redis or a RDBM such as PostgreSQL).

## Scaling JSF with Spring Session

To see Spring Session in action, we are going to start two instances of a specific JSF application. These two instances will run on different ports (i.e. one will run on port 8081 and the other on port 8082) and the session on them will be tied together with Spring Session. To orchestrate the requests between both instances, we are going to use the [round robin algorithm](https://en.wikipedia.org/wiki/Round-robin_scheduling) on NGINX.

## JSF Application

To speed up the process, let's use this [JSF + Spring Boot application](https://auth0.com/blog/developing-jsf-applications-with-spring-boot/) as a starting point. To do so, we will clone the [GitHub repository available here](https://github.com/auth0-blog/spring-boot-faces/tree/complete), and checkout the `complete` branch.

```bash
git clone https://github.com/auth0-blog/spring-boot-faces.git
cd spring-boot-faces
git checkout complete
```

If we ran the application, by issuing `mvn spring-boot:run`, we will see that this is a simple application that has three features: it shows a list of products; it allows users to add new products; and it allows users to delete products. This application already, as most JSF applications, already uses HTTP session. But, before proceeding with the configuration of Spring Session, we will make two adjustments to this application:

1. We will add a feature that will make easier to see the that the session is indeed being handled by Spring Session.
2. We will replace HSQLDB (an in memory database) by PostgreSQL. We will use the latter as the session store of our application.

### Enabling Product's List Reordering

The product list shown in our application is loaded from the database the first time the user access the application and whenever they add or delete a product. We will add to the product's list a feature that enables user to reorder it. This reordering won't be persisted to the database, it will be a feature that resides on 
