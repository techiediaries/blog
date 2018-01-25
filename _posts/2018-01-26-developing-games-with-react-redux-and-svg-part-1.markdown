---
layout: post
title: "Developing Games with React, Redux, and SVG - Part 1"
description: "Learn how to make React and Redux control a bunch of SVG elements to create a game."
longdescription: "In this series, you will learn how to make React and Redux control a bunch of SVG elements to create a game. The knowledge acquired throughout this series will also allow you to create other types of animations that are orchestrated by React and Redux, not only games."
date: 2018-01-26 15:42
category: Technical Guide
author:
  name: "Bruno Krebs"
  url: "https://twitter.com/brunoskrebs"
  mail: "bruno.krebs@gmail.com"
  avatar: "https://twitter.com/brunoskrebs/profile_image?size=original"
design:
  image: https://cdn.auth0.com/blog/sso-b2c/logo.png
  bg_color: "#002C5F"
tags:
- react
- redux
- svg
- game
- auth0
- animation
- state
related:
- 2018-01-10-implementing-single-sign-on-in-b2c-applications
---

**TL;DR:** A brief synopsis that includes link to a [github repo](http://www.github.com/).

---

## The React Game: Aliens, Go Home!

## Prerequisites

## Bootstrapping a React Project with Create-React-App

The very first thing you will do to create a game with React, Redux, and SVG is to use `create-react-app` to bootstrap your project. As you probably know (it doesn't matter if you don't), [`create-react-app` is an open-source tool, maintained by Facebook, that helps developers to start developing in React in no time](https://github.com/facebookincubator/create-react-app). Having Node.js and NPM installed locally (the latter has to be 5.2 and higher), you can use `create-react-app` without even installing it:

```bash
# using npx will download (if needed)
# create-react-app and execute it
npx create-react-app aliens-go-home

# change directory to the new project
cd aliens-go-home
```

This tool will create a structure similar to the following one:

```bash
|- node_modules
|- public
  |- favicon.ico
  |- index.html
  |- manifest.json
|- src
  |- App.css
  |- App.js
  |- App.test.js
  |- index.css
  |- index.js
  |- logo.svg
  |- registerServiceWorker.js
|- .gitignore
|- package.json
|- package-lock.json
|- README.md
```

The `create-react-app` tool is popular, well documented, and well supported by the community. As such, if you are interested in learning its details, you can check the [official `create-react-app` GitHub repository](https://github.com/facebook/create-react-app) and [its user guides](https://github.com/facebook/create-react-app#user-guide).

Right now, what you will want to do is to remove some stuff that you won't need. For example, you can get rid of the following files:

- `App.css`: the `App` component is important but the styles definitions will be delegated to other components;
- `App.test.js`: tests might be addressed in another article, but you won't use it for now;
- `logo.svg`: you won't use React's logo in this game;

## Installing Redux and PropTypes

## Creating the Basic SVG Elements

### Quick Review on SVG

### SVG and Bezier Curves

### Creating the Canvas React Component

### Creating the Sky React Component

### Creating the Ground React Component

### Creating the Cannon React Component

### Making the Cannon Aim

## Next Steps
