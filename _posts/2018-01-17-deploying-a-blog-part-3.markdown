---
layout: post
title: "Symfony Tutorial: Deploying a blog (Part 3)"
description: "Let's create and deploy a secure blog engine with Symfony."
longdescription: "Creating applications with Symfony is easy and can be scaled to be used in any requirement. The tools that it provides to create and maintain web applications is amazing and replaces repetitive tasks. Let's use Symfony to create a blog engine."
date: 2018-01-17 11:00
category: Technical Guide, PHP, Symfony
author:
  name: Greg Holmes
  url: https://github.com/GregHolmes
  mail: iam@gregholmes.co.uk
  avatar: "https://avatars0.githubusercontent.com/u/2411269?s=460&v=4"
design:
  bg_color: "#000000"
  image: https://cdn.auth0.com/blog/symfony-blog/logo.png
tags:
- symfony
- php
- auth0
- bootstrap
- authentication
- heroku
- travis-ci
- web-app
related:
- 2017-12-28-symfony-tutorial-building-a-blog-part-1
- 2018-01-04creating-symfony-blog-part-2
- 2016-06-23-creating-your-first-laravel-app-and-adding-authentication
---

**TL;DR:** Symfony is a PHP framework as well as a set of reusable PHP components and libraries. It uses the Model-View-Controller design pattern and can be scaled to be used in any requirement. It aims to speed up the creation and maintenance of web applications, replacing repetitive code. In this part of the article, we will cover installing [Bootstrap, a UI framework for web applications](https://getbootstrap.com/), to make the blog engine look nicer visually. The final code can be found at this [repository](https://github.com/auth0-blog/symfony-blog-part-2).

---


## Symfony Tutorial: About Part 1 and Part 2

[In the first article](https://auth0.com/blog/symfony-tutorial-building-a-blog-part-1/), we:

* installed and configured a Symfony installation;
* created two new database tables: `author` and `blog_post`;
* allowed users to authenticate with [Auth0](https://auth0.com);
* and ensured that the authenticated users have `Author` instances associated before using the system.

[In the second article](https://auth0.com/blog/creating-symfony-blog-part-2/), we covered installing [Bootstrap, a UI framework for web applications](https://getbootstrap.com/), to make the blog engine look nicer visually. We also enhanced our blog engine to allow visitors to:

* see a list of blog posts;
* read a specific blog post;
* and find out more about authors.

Besides that, authenticated authors were able to:

* create a new blog post;
* see all of their own blog posts;
* and delete their own blog posts from the system.

In this third article, we will be covering deployments to two different environments (`staging` and `production`). We will be carrying this out by using two services, [Heroku](https://www.heroku.com) and Travis CI) allowing us to:

* make code changes to the blog;
* deploy these changes on committing them to github
* almost instantly see their changes in production and/or staging

## About [Heroku](https://www.heroku.com)

[Heroku](https://www.heroku.com) is a container-based cloud Platform as a Service (PaaS). [Heroku](https://www.heroku.com) is used to deploy, manager and scale modern apps by developers. The service is a very flexible, simple to use and quick to start service allowing developers to focus more on the product than spending too much time in the technology. Added to this, [Heroku](https://www.heroku.com) is fully managed, this further reduces the time developers need to spend on maintaining servers, hardware of the infrastructure.

## About Travis CI

Travis CI is a hosted, distributed, continuous integration service used to build and test software projects hosted at GitHub.
 
Open source projects may be tested at no charge via travis-ci.org. Private projects may be tested at travis-ci.com on a fee basis. TravisPro provides custom deployments of a proprietary version on the customer's own hardware.
 
Although the source is technically free software and available piecemeal on GitHub under permissive licenses, the company notes that it is unlikely that casual users could successfully integrate it on their own platforms.[4]

## Building the Blog Engine

### Before Starting

For this tutorial, you will need a GitHub account. So head over to [sign up at GitHub](https://github.com/join?source=header-home).

Once signed up, go to our example repository for the [Symfony blog part 2](https://github.com/auth0-blog/symfony-blog-part-2).
Click the "Fork" button, found in the top right hand corner of the browser. As shown in the example below:

#todo: include fork-repository.png image

Make sure you have followed all instructions in the first two parts. 

You will find that you now have a repository of the same name, but under your account. Click the "Clone or download" button and copy the URL found in there.
Next is to clone this repository for yourself, so run the following two commands. However replace the URL with your own one:

```bash
git clone https://github.com/auth0-blog/symfony-blog-part-2
cd symfony-blog-part-2
```

After that, you will have to create a file called `.env` in the project root and paste the following into it:

```yml
DATABASE_HOST={DATABASE_HOST}
DATABASE_PORT={DATABASE_PORT}
DATABASE_NAME={DATABASE_NAME}
DATABASE_USER={DATABASE_USER}
DATABASE_PASSWORD={DATABASE_PASSWORD}
AUTH0_CLIENT_ID={AUTH0_CLIENT_ID}
AUTH0_CLIENT_SECRET={AUTH0_CLIENT_SECRET}
AUTH0_DOMAIN={AUTH0_DOMAIN}
```

Note that you will have to replace the values above. [Check the first part to understand how to replace them](https://auth0.com/blog/symfony-tutorial-building-a-blog-part-1/).

__Pro Tip!__ If you do not have a MySQL database available, an easy way to bootstrap one is with Docker:

```bash
docker run --name symfony-blog-mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=myextremellysecretpassword \
    -e MYSQL_DATABASE=symfony-blog \
    -e MYSQL_USER=symfony-blog-user \
    -e MYSQL_PASSWORD=mysecretpassword \
    -d mysql:5.7
```

The last thing you will need to do is to use composer to install the dependencies:

```bash
composer install
```

This will trigger a series of questions that you can answer as follows:

```bash
database_host (127.0.0.1): 127.0.0.1
database_port (null): 3306
database_name (symfony): symfony-blog
database_user (root): symfony-blog-user
database_password (null): mysecretpassword
```

For the questions related to `mailer_transport` and `secret`, you can simply press `Enter` to accept the default values.

If you haven't followed the first two parts of this series, you might need to issue the following commands to create and populate the database tables:

```bash
php bin/console doctrine:migrations:migrate
php bin/console doctrine:fixtures:load
```

__NOTE__ If you do not have [Yarn](https://yarnpkg.com), a Javascript package manager, installed, you will need to install and configure this. So go to their [Installation](https://yarnpkg.com/lang/en/docs/install/) page and follow the instructions for installing and configuring Yarn first.

You can then install the third party libraries used to make the blog look nicer visually with the following command:

```bash
yarn add @symfony/webpack-encore --dev
yarn run encore dev --watch
```

## Install dependencies

### Installing Heroku CLI

In order to use Heroku, you need to have an account. So [sign up here](https://signup.heroku.com/) and follow all instructions provided on the website.

Once you have created an account, we're going to need to install the command line interface (CLI). Each operating system has a different method of installing Heroku CLI. So below is a list of the commands used to install this:

* on MacOS: `brew install heroku/brew/heroku`
* on Ubuntu Snap: `sudo snap install heroku --classic`
* on Debian / Ubuntu: `wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh`
* on Windows 64-bit: [Installer](https://cli-assets.heroku.com/heroku-cli/channels/stable/heroku-cli-x64.exe)
* on Windows 32-bit: [Installer](https://cli-assets.heroku.com/heroku-cli/channels/stable/heroku-cli-x86.exe)

- Create space

Now we have an account and have the CLI installed on our system, we're going to need to create a space on Heroku. Running the following command will do this:

__NOTE__ Please change the `space-name-here` to be the space name you wish. Otherwise you will find that the space `space-name-here` is already in use by someone else.

```bash
heroku apps:create space-name-here
```

#todo: include create-heroku-space.png image

Great, you can now access your website. Head over to the url shown after you created your space, as shown in the example above. 

Now we have our space, we need to create our database. There are several different add-ons for Heroku in order to use a database in your package. However, although there are free plans, you still need to provide your card details. So please head over to: [Heroku Verify](https://heroku.com/verify) to add your card details.

Once verified, we're going to add the ClearDb add-on to our space. So in your Terminal you can run the following command to add a database for your blog to access:

```bash
heroku addons:add cleardb:ignite
```

#todo: include create-heroku-database.png image

Heroku stores the environment files themselves. So in order for our blog to use keys such as the database table name, user, password and so on. We need to store them as environment keys. Previously we placed this in a `.env` file.

The following is the structure of keys we're going to need to use:

```bash
heroku config:set SYMFONY_ENV=prod
heroku config:set AUTH0_CLIENT_ID=(Your Auth0 Client ID)
heroku config:set AUTH0_CLIENT_SECRET=(Your Auth0 client secret)
heroku config:set AUTH0_DOMAIN=(Your Auth0 Domain)
heroku config:set DATABASE_NAME=(Your database name shown in the image above)
```

### Installing Travis-ci CLI


## Configuring Symfony for production
    - Configure Composer requirement changes
    - Move doctrine config to config_dev and config_prod
    - Move logs config to config_dev and config_prod

## Instructions for staging

## Conclusion

Congratulations, you have built yourself a functional blog engine from scratch with Symfony. This blog engine even enables visitors to sign up to become authors. This allows them to also contribute to your blog by posting articles of their own! Although this is just the basics of a blog, it is a strong stepping stone into making it as custom and feature filled as you wish.
