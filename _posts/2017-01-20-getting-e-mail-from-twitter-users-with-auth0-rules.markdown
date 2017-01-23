---
layout: post
title: "Getting e-mail from Twitter users with Auth0 rules"
description: "Get "
date: 2017-01-20 22:50
author:
  name: <YOUR NAME>
  url: <YOUR URL>
  mail: <YOUR MAIL>
design:
  bg_color: <A HEX BACKGROUND COLOR>
  image: <A PATH TO A 200x200 IMAGE>
tags:
- foo
---

**TL;DR**

*Getting e-mail from Twitter users with Auth0 rules is as simple as clicking a button on the dashboard. Just add the predefined rule entitled 'Get email address from Twitter', configure your client codes and you are good to go.*

## Twitter and the Email Address Tale

First Twitter didn't share users e-mail addresses with applications that were integrating to its OAuth implementation. Then it shared only to previously whitelisted applications. Now it shares with all the applications that asks for it, but through a step that it is not part of the default authentication and authorization flow.

If things keep moving as they are, soon Twitter will make our lives easier and just share the email address as everyone else does. But, in the mean time, lets take advantage of Auth0 rules to take this information.

## Auth0 Rules to the Rescue

Since Twitter does not handle this information on the normal workflow, we need to issue a second request to its API to gather the information. Issuing a request to Twitter on behalf of a user is a quite cumbersome task. If you are not interested on the details involved, just head to the last section were I show you how to add and configure the template rule to your Auth0 account. For the brave and curious, keep up.

>I assume you won't have trouble reading JavaScript, as this is the de facto write once run everywhere language that everybody is talking about nowadays. Also, it is important to notice that as Auth0 rules are run as the last step of the sign-in process, the user's `access token` and `access token secret` is available us. If you are trying to get this information by other means, you will have to figure it out how to get both tokens before going ahead.
