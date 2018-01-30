---
layout: post
title: "Symfony Tutorial: Building a Blog (Part 3)"
description: "Let's create and deploy a secure blog engine with Symfony."
longdescription: "Creating applications with Symfony is easy and can be scaled to be used in any requirement. The tools that it provides to create and maintain web applications is amazing and replaces repetitive tasks. Let's use Symfony to create a blog engine."
date: 2018-01-25 22:05
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
- 2018-01-04-creating-symfony-blog-part-2
- 2016-06-23-creating-your-first-laravel-app-and-adding-authentication
- 2017-05-08-the-ultimate-guide-to-deploying-php-applications
---

**TL;DR:** Symfony is a PHP framework as well as a set of reusable PHP components and libraries. It uses the Model-View-Controller design pattern and can be scaled to be used in any requirement. It aims to speed up the creation and maintenance of web applications, replacing repetitive code. In this part of the article, we will cover installing [Bootstrap, a UI framework for web applications](https://getbootstrap.com/), to make the blog engine look nicer visually. The final code can be found at this [repository](https://github.com/auth0-blog/symfony-blog-part-2).

---


## Symfony Tutorial: About Part 1 and Part 2

[In the first article](https://auth0.com/blog/symfony-tutorial-building-a-blog-part-1/), we:

* installed and configured a Symfony installation;
* created two new database tables: `author` and `blog_post`;
* allowed users to authenticate with [Auth0](https://auth0.com);
* and ensured that the authenticated users have `Author` instances associated with them before using the system.

[In the second article](https://auth0.com/blog/creating-symfony-blog-part-2/), we covered installing [Bootstrap, a UI framework for web applications](https://getbootstrap.com/), to make the blog engine look nicer visually. We also enhanced our blog engine to allow visitors to:

* see a list of blog posts;
* read a specific blog post;
* and find out more about authors.

Besides that, authenticated authors were able to:

* create a new blog post;
* see all of their own blog posts;
* and delete their own blog posts from the system.

In this third article, we will be covering deployments to two different environments (`staging` and `production`). We will be carrying this out by using two services, [Heroku](https://www.heroku.com) and [Travis-CI](https://travis-ci.com/)) allowing us to:

* make code changes to the blog;
* deploy these changes on committing them to GitHub
* almost instantly see their changes in production and/or staging

## About [Heroku](https://www.heroku.com)

[Heroku](https://www.heroku.com) is a container-based cloud Platform as a Service (PaaS). [Heroku](https://www.heroku.com) is used to deploy, manage and scale modern apps by developers. The service is very flexible, simple to use and quick to start allowing developers to focus more on the product rather than spending too much time on the technology. Added to this, [Heroku](https://www.heroku.com) is fully managed, this further reduces the time developers need to spend on maintaining servers and the hardware of the infrastructure.

## About Travis CI

[Travis CI](https://travis-ci.org/) is a hosted, distributed, continuous integration service. [Travis CI](https://travis-ci.org/) allows you to build and test your software hosted on GitHub, allowing you to publicise projects comfortable in knowing that project is fully functional for others to use.

With [Travis CI](https://travis-ci.org/), there are 2 different sites to use. [Travis-ci.org](https://travis-ci.org/) allows you test open source projects (public repositories). Whereas [Travis-ci.com](https://travis-ci.com/) allows you to test private projects but requires a fee dependant on the size of your project.

## Building the Blog Engine

### Before Starting

For this tutorial, you will need a GitHub account. So head over to [sign up at GitHub](https://github.com/join?source=header-home).

Once signed up, go to our example repository for the [Symfony blog part 2](https://github.com/auth0-blog/symfony-blog-part-2).

Click the "Fork" button, found in the top right-hand corner of the browser. As shown in the example below:

![Forking a repository on GitHub](https://cdn.auth0.com/blog/symfony-part-3/fork-repository.png)

Make sure you have followed all instructions in the first two parts.

You will find that you now have a repository of the same name but under your account. Click the "Clone or download" button and copy the URL found in there.
Next is to clone this repository for yourself, so run the following two commands. However, replace the URL with your own one:

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

Let's install the third party libraries used to make the blog look nicer visually with the following command:

```bash
yarn add @symfony/webpack-encore --dev
yarn run encore dev
```

## Install dependencies

### Installing Heroku CLI

In order to use Heroku, we need to have an account. So [sign up here](https://signup.heroku.com/) and follow all instructions provided on the website.

Once an account has been created, we're going to need to install the command line interface (CLI). Each operating system has a different method of installing Heroku CLI. So below is a list of the commands used to install this:

* on MacOS: `brew install heroku/brew/heroku`
* on Ubuntu Snap: `sudo snap install heroku --classic`
* on Debian / Ubuntu: `wget -qO- https://cli-assets.heroku.com/install-ubuntu.sh | sh`
* on Windows 64-bit: [Installer](https://cli-assets.heroku.com/heroku-cli/channels/stable/heroku-cli-x64.exe)
* on Windows 32-bit: [Installer](https://cli-assets.heroku.com/heroku-cli/channels/stable/heroku-cli-x86.exe)

Now we have an account and have the CLI installed on our system, we're going to need to create a space on Heroku. Running the following command will do this:

__NOTE__ Please change the `space-name-here` to be the space name you wish. Otherwise, you will find that the space name `space-name-here` is already in use by someone else.

```bash
heroku apps:create space-name-here
```

![Creating a Heroku space](https://cdn.auth0.com/blog/symfony-part-3/create-heroku-space.png)

Great, we can now access our website. If you take the URL that's given to you as a result of your command above, similar to the image above and put it into your browser, you should see the example page as shown in the image below:

![Heroku space created](https://cdn.auth0.com/blog/symfony-part-3/heroku-space-browser.png)

Now we have our space, we need to create our database. There are several different add-ons for Heroku in order to use a database in your package. However, although there are free plans, you still need to provide your card details. So please head over to [Heroku Verify](https://heroku.com/verify) to add your card details.

Once verified, we're going to add the ClearDb add-on to our space. So in the Terminal let's run the following command to add a database to our blog to access:

```bash
heroku addons:add cleardb:ignite
```

When the command has finished running, we should see something similar to the image below:

![Creating a ClearDB database](https://cdn.auth0.com/blog/symfony-part-3/create-heroku-database.png)

Heroku stores the environment files themselves. So in order for our blog to use keys such as the database table name, user, password and so on. We need to store them as environment keys. Previously we placed this in a `.env` file.

The following is the structure of keys we're going to need to use:

```bash
heroku config:set SYMFONY_ENV=prod
heroku config:set AUTH0_CLIENT_ID=(Your Auth0 Client ID)
heroku config:set AUTH0_CLIENT_SECRET=(Your Auth0 client secret)
heroku config:set AUTH0_DOMAIN=(Your Auth0 Domain)
heroku config:set DATABASE_NAME=(Your database name shown in the image above)
```

### Installing Travis-ci

[Travis-ci](https://travis-ci.org/) can be installed via Ruby. If you have Ruby installed, please make sure it is at least version 1.9.3 (2.0.0 recommended).
You can check this by typing in a Terminal:

```bash
ruby -v
```

If Ruby isn't installed, it can be installed by one of the following methods, depending on your operating system:

```bash
brew install ruby # Mac OS X
sudo apt-get install ruby-full # Debian / Ubuntu
pkg install ruby # Free bsd
sudo yum install ruby # CentOS, Fedora, or RHEL
```

Now that Ruby is installed or at the minimum required version, let's install [Travis CI](https://travis-ci.org/) CLI with the following command:

```bash
gem install travis -v 1.8.8 --no-rdoc --no-ri
```

Once complete the installation can be verified by checking the version with the following command:

```bash
travis version
```

Making use of [Travis CI](https://travis-ci.org/) requires a GitHub account. As an account was previously created one to fork the Symfony-blog tutorial, we don't need to create another. So let's head over to Travis-ci to [sign up](https://travis-ci.org/).

Once the account is created at [Travis CI](https://travis-ci.org/), we should see a profile page, that contains the list of our Github repositories. All we need to do is toggle our specific repository to on.

In order to run through the [Travis CI](https://travis-ci.org/) build, we need a `.env` file. So in the root of our project, let's create a `.env.travis.dist` file and paste the following in:

```yml
DATABASE_HOST=127.0.0.1
DATABASE_PORT=3306
DATABASE_NAME=travisbuild
DATABASE_USER=travis
DATABASE_PASSWORD=
CLEARDB_DATABASE_URL=
AUTH0_CLIENT_ID=
AUTH0_CLIENT_SECRET=
AUTH0_DOMAIN=
```

We've enabled our GitHub repository on [Travis CI](https://travis-ci.org/), but we now need to create a `.travis.yml` file so that [Travis CI](https://travis-ci.org/) knows what to do with this. So create the file and put the following in:

```yml
language: php
sudo: false
services:
- mysql
cache:
  directories:
  - "$HOME/.composer/cache/files"
matrix:
  fast_finish: true
  include:
  - php: 7.1
env:
- SYMFONY_VERSION="3.3.*" DB=mysql
before-install:
- composer self-update
install:
- cp .env.travis.dist .env
- composer install
- php bin/console doctrine:database:create --env=test
- php bin/console doctrine:schema:create --env=test
- php bin/console doctrine:fixtures:load -n --env=test
notifications:
email:
deploy:
  provider: heroku
  api_key:
    secure:
  app:
```

There are 3 parts of this file where we need to input unique details here:

* First is our e-mail, please type in your e-mail next to the line: `email: `
* Second is your secure api_key for [Heroku](https://www.heroku.com). If you run the following command, it'll put the encrypted [Heroku](https://www.heroku.com) API key for you:

```bash
travis encrypt $(heroku auth:token) --add deploy.api_key
```

* And finally is the app our [Heroku](https://www.heroku.com) refers to, this is your [Heroku](https://www.heroku.com) space id. So please type it in next to `app: ` which is at the bottom of the file.

## Configuring Symfony for production

Now we have set up and configured Heroku and [Travis CI](https://travis-ci.org/), we need to make some minor changes to our Symfony installation in order to be functional when hosted on [Heroku](https://www.heroku.com).

Let's make some changes to our `composer.json` file. Where it says:

```json
{
//...
  "minimum-stability": "dev",
}
```

Change it to:

```json
{
//...
  "minimum-stability": "stable",
}
```

Let's add a compile command, that Heroku will understand when being deployed to, to update the schema of the database. So find the following:

```json
"symfony-scripts": [
    "Incenteev\\ParameterHandler\\ScriptHandler::buildParameters",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::buildBootstrap",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::clearCache",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::installAssets",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::installRequirementsFile",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::prepareDeploymentTarget"
],
"post-install-cmd": [
    "@symfony-scripts"
],
"post-update-cmd": [
    "@symfony-scripts"
]
```

Add the compile entry at the bottom as shown below:

```json
"symfony-scripts": [
    "Incenteev\\ParameterHandler\\ScriptHandler::buildParameters",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::buildBootstrap",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::clearCache",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::installAssets",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::installRequirementsFile",
    "Sensio\\Bundle\\DistributionBundle\\Composer\\ScriptHandler::prepareDeploymentTarget"
],
"post-install-cmd": [
    "@symfony-scripts"
],
"post-update-cmd": [
    "@symfony-scripts"
],
"compile": [
    "php bin/console doctrine:schema:update --force"
]
```

Now let's update our composer lock file with the following command:

```bash
composer update
```

We now need to exclude our DataFixtures from the services autowiring. Open the file: `app/config/services.yml` and find:

```yml
AppBundle\:
    resource: '../../src/AppBundle/*'
    # you can exclude directories or files
    # but if a service is unused, it's removed anyway
    exclude: '../../src/AppBundle/{Entity,Repository,Tests}'
```

In exclude, where it shows `{Entity,Repository,Tests}` add `DataFixtures`. This will look like:

```yml
AppBundle\:
    resource: '../../src/AppBundle/*'
    # you can exclude directories or files
    # but if a service is unused, it's removed anyway
    exclude: '../../src/AppBundle/{Entity,Repository,Tests,DataFixtures}'
```

Our doctrine configuration will be different in production than it is in development. So in `app/config/config.yml` you should find something similar to:

```yml
# Doctrine Configuration
doctrine:
    dbal:
        driver: pdo_mysql
        host: '%env(DATABASE_HOST)%'
        port: '%env(DATABASE_PORT)%'
        dbname: '%env(DATABASE_NAME)%'
        user: '%env(DATABASE_USER)%'
        password: '%env(DATABASE_PASSWORD)%'
        charset: UTF8

    orm:
        auto_generate_proxy_classes: '%kernel.debug%'
        naming_strategy: doctrine.orm.naming_strategy.underscore
        auto_mapping: true
```

Move this to the dev config file: `app/config/config_dev.yml`.

In `app/config/config_prod.yml` we need the configuration to be slightly different. Paste the following into the bottom of that file:

```yml
# Doctrine Configuration
doctrine:
    dbal:
        driver: pdo_mysql
        dbname: '%env(DATABASE_NAME)%'
        url: '%env(CLEARDB_DATABASE_URL)%'
        charset: UTF8

    orm:
        auto_generate_proxy_classes: '%kernel.debug%'
        naming_strategy: doctrine.orm.naming_strategy.underscore
        auto_mapping: true
```

As you can see from the above, we are no longer using the database host, port, name, user and password. Instead we need only 2 variables, these are url, dbname.

Let's change the location of the log file found in `app/config/config_prod.yml`.
In this file, there will be something similar to:

```yml
monolog:
    handlers:
        main:
            type: fingers_crossed
            action_level: error
            handler: nested
        nested:
            type: stream
            path: '%kernel.logs_dir%/%kernel.environment%.log'
```

Replace the last line with:

```yml
path: 'php://stderr'
```

One final change is to remove the current PHPUnit test, we previously removed the DefaultController, there is still a test for this which will attempt to run (and fail) when building in Travis-CI.
So remove the following directory `tests/AppBundle/Controller`.

It's time to commit all of our changes to our forked repository. This can be done by the following commands:

```bash
git add .
git commit -m "Added file changes to prepare for pushing to production"
git push
```

## Instructions for staging

If we wish to make use of a staging environment for testing on, we're going to need to repeat some of our steps above. A staging environment allows you to make use of a direct replica of the production server. Providing an environment for those with access to test new features before they're ready to be published for public.

First, let's create our staging space on [Heroku](https://www.heroku.com) with the following command:

```bash
heroku apps:create space-name-here-staging
```

If previously you called your space "space-name-here" let's just append "-staging" on the end, so we know it's for staging, as shown in the example above.

Next, we're going to need to create a new database entry for this space. Let's do this by running the following command:

```bash
heroku addons:add cleardb:ignite
```

We now need to change the `travis.yml` file. Where we previously had:

```yml
  app: space-name-here
```

We want to replace this with branch specific configurations:

```yml
  app:
      master: space-name-here
      staging: space-name-here-staging
```

Next, we're going to need to create a staging area on Auth0, [log into your account here](https://manage.auth0.com/#/clients).
Click the button "Create Client", you can call it whatever you want. I would suggest call it the same as your current client, but append "-staging" to the end of it.

Once created, we need to make use of the Auth0 client Id, secret and domain. We also need to make use of the database name returned to us when we ran the cleardb:ignite command. Replace the contents of the brackets in the examples below with these details. And then run each of these commands:

```bash
heroku config:set SYMFONY_ENV=prod
heroku config:set AUTH0_CLIENT_ID=(Your Auth0 Client ID)
heroku config:set AUTH0_CLIENT_SECRET=(Your Auth0 client secret)
heroku config:set AUTH0_DOMAIN=(Your Auth0 Domain)
heroku config:set DATABASE_NAME=(Your database name shown in the image above)
```

Now that we have our environment set up, we need to run the following git commands to create and deploy the staging branch:

```bash
git checkout -b staging
git push --set-upstream origin staging
```

If we head over to our Heroku dashboard, we should see the build in progress or complete. If we now head to the URL for our staging environment we'll see an exact duplicate of the production environment.

## Conclusion

The deployment process is a very broad subject. It can also be very daunting for someone who hasn't had any exposure to this part of development.

Hopefully, this article will have given you a footstep into the world of deploying PHP applications.

With this article, you should have the foundation to creating, designing and deploying the blog you always wanted!
