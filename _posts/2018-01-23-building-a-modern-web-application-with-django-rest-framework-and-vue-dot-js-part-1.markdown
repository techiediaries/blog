---
layout: post
title: "Building Modern Applications with Django and Vue.js: Part 1"
description: "In this series, you will create, step by step, a full-stack application with Django and Vue.js."
longdescription: "Throughout this series, you'll be using Django, Django REST framework, and Vue.js to develop an application with a REST API back-end and a Vue.js front-end. The API will be consumed by the Vue.js front-end with the help of the Axios client library and JWT authentication will be handled by Auth0."
date: 2018-01-23 17:28
category: Technical Guide, Python, Django
author:
  name: "Ahmed Bouchefra"
  url: "https://www.techiediaries.com"
  mail: "techiediaries9@gmail.com"
  avatar: "https://twitter.com/ahmedbouchefra/profile_image?size=original"
design:
  bg_color: "#4A4A4A"
  image: https://cdn.auth0.com/blog/python-restful/logo.png
tags:
- django
- vue.js
- python
- javascript
- rest
- restful
- api
- auth0
- identity
related:
- 2017-11-09-sqlalchemy-orm-tutorial-for-python-developers
- 2017-09-28-developing-restful-apis-with-python-and-flask
---

**TL;DR:** Throughout this series, you'll be using Django, Django REST framework, and Vue.js to develop an application with a REST API back-end and a Vue.js front-end. The API will be consumed by the Vue.js front-end with the help of the Axios client library and JWT authentication will be handled by Auth0.

You are going to start by installing all the project's requirements, then you will bootstrap the Django and the Vue.js sub-projects.

You can find the final source code of the demo project that you will create in this [GitHub repository](https://github.com/techiediaries/django-auth0-vue).

---

## Summary

This article is composed of the following sections:

* Introduction to Python and Django
* Introduction to Vue.js and Vue.js Features
* Bootstrapping the Back-End Project
* Bootstrapping the Front-End Project
* Introduction to JSON Web Tokens
* Integrating Django with Auth0
* Integrating Auth0 with Vue.js
* Conclusion and Next Steps

## Introduction to Python and Django

[Python](https://www.python.org/) is a general purpose programming language and it's among the most popular programming languages in the world. It's readable, efficient, and [easy to learn](http://lifehacker.com/five-best-programming-languages-for-first-time-learners-1494256243/1497409477). Python is also a portable language available for major operating systems such as Linux, Windows, and Mac.

For web developers, Python has many great tools and frameworks that make developers more productive and able to build prototypes in no time. The most popular framework for web development in the Python landscape is [Django](https://www.djangoproject.com/). Django is advertised as *the framework for perfectionists with tight deadlines* because of its ability to allow developers to quickly build prototypes.

Django uses the power of Python to offer developers a great set of features such as class based views and a powerful ORM that you can use to model your database requirements without writing any single line of SQL. The Django ORM abstracts away all the complexities of working with databases and, most importantly, doesn't intimidate you when your client has not decided yet on the database system to use. You can start developing using a SQLite database (which doesn't need any special installation) then you can switch to the right database system later on when your client has settled on the right RDBMS (Relational Database Management System) to use.

Django also comes with a powerful migration system that allows you to migrate your database safely and without losing your data when you make changes to the database structure later on.

Another advantage of using Django is that it has a big and an evolving community. This community has created open source packages for common web development problems, so you don't need to reinvent the wheel when building your project.

Django has an excellent and complete documentation for all the features of the framework supplemented by a set of tutorials created by the community.

Django is also suitable for beginners and you don't have to be an expert with every feature of the framework to start building your web application.

{% include tweet_quote.html quote_text="Django is also suitable for beginners and you don't have to be an expert with every feature of the framework to start building your web application." %}

## Introduction to Vue.js and Vue.js Features

[Vue.js is a progressive framework for building user interfaces with JavaScript](https://vuejs.org/). You can use Vue.js in the view layer of your application or you can use it to build Single Page Applications (SPAs) by combining it with some other front-end [tools](https://vuejs.org/v2/guide/single-file-components.html).

Vue.js took the best of both Angular.js and React into one library. For example, just like React, Vue.js uses [a virtual DOM](https://vuejsdevelopers.com/2017/02/21/vue-js-virtual-dom/) and a component-based approach. It also uses similar syntax to the Angular.js directives (e.g. `v-if` and `v-for`)

Vue.js has many features, such as:

* virtual DOM;
* reactive and composable view components;
* great performance;
* and native rendering on iOS and Android thanks to [Weex](https://weex.incubator.apache.org/) and [NativeScript](https://github.com/rigor789/nativescript-vue);

## Bootstrapping the Back-End Project

Before you can create the back-end project, you need to install some requirements in your development machine. For Django, you need to have [Python 3](https://www.python.org/download/releases/3.0/), [PIP](https://pypi.python.org/pypi/pip), and [`venv`](https://docs.python.org/3/library/venv.html) installed.

Please note that you don't need to install a full fledged database management system to develop with Django. You can use a SQLite database which allows you to have a file-based database that doesn't require any special installation.

### Installing the Requirements

Let's start with Python 3. Chances are that you already have Python 3 installed on your machine. If not, then the process is simple. You just need to head over to the [official  downloads page](https://www.python.org/downloads/) and pick the installer for your operating system.

You can check if you have Python 3 installed by running the following command from your terminal or command prompt:

```bash
python3 --version
```

The `venv` module (part of the Python 3 standard library) allows you to create lightweight virtual environments for your projects. This allows you to have an isolated environments for each Python project (i.e. dependencies are isolated for each project and from the system wide packages).

Setting up an isolated environment provides you with more control over the installed Python packages. You can have different versions for the same package without having to worry about any conflicts, which allows you to work with different Python projects with different package versions. Also, since each environment can have its own Python binary, this allows you to create various environments with different Python versions.

Once you create a virtual environment, `venv` will take care of installing the latest Python binary.

Next, you need to install [`pip`](https://packaging.python.org/key_projects/#pip), a package manager for Python that, by default, installs packages from the Python Package Index ([PyPI](https://pypi.python.org/pypi)).

You can verify if `pip` is installed in your development machine by running the following command:

```bash
# pip or pip3, depending on the environment
pip --version
```

You should have `pip` installed if you have installed Python using the official [python.org](https://www.python.org/downloads) installer (or via Homebrew in MAC). Also, if you are inside a `venv` environment, pip is already installed. For Linux, you may need to install it separately. If that's your environment, take a look at this [guide](https://packaging.python.org/guides/installing-using-linux-tools/) and [this page](https://pip.pypa.io/en/stable/installing/).

### Creating a Virtual Environment

Once you have all the back-end requirements installed, you will have to create a virtual environment for installing your project's dependencies. Head back to your terminal and run the following commands:

```bash
# create a directory to your project
mkdir djangovuejsproject

# move the cursor to it
cd djangovuejsproject

# initialize a virtual env on it
python3 -m venv ./
```

Next you need to activate the virtual environment using `source`:

```bash
source ./bin/activate
```

### Bootstrapping Django

You can now install Django using `pip`:

```bash
pip install django
```

The next step is to create the Django project using this command:

```bash
django-admin startproject djangovuejsproject .
```

Now, execute the following command to organize your Django project with [`catalog`](https://github.com/redsolution/django-catalog):

```bash
python manage.py startapp catalog
```

You also need to add this app to the list of installed apps. Open your project `settings.py` file then add the app:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'catalog'
]
```

You can then migrate your database and start the development server:

```bash
python manage.py migrate
python manage.py runserver
```

If you navigate to [http://127.0.0.1:8000](http://127.0.0.1:8000) in a web browser, you will see the following homepage:

![Django starter page](https://cdn.auth0.com/blog/django-vuejs/django-basic-app.png)

## Bootstrapping the Front-end Project

Vue.js has a [CLI utility](https://github.com/vuejs/vue-cli) that allows developers to quickly generate SPAs. The CLI offers pre-configured build setups for a modern frontend workflow and takes only a few minutes to scaffold a basic project boilerplate. This boilerplate is shipped with features such as hot-reloading, lint-on-save, and production-ready builds.

Before you can install the Vue.js CLI, you need to have a development environment with Node.js 6.x or newer and NPM version 3 or newer. You can install both of them by heading to the [official Node.js website](https://nodejs.org/en/download/) and download the right installer for your operating system. For development machines, it's recommended to use [NVM](https://github.com/creationix/nvm).

Next, head over to your terminal or command prompt and run the following command:

```bash
npm install -g vue-cli
```

> You may need to add `sudo` to the NPM command to install commands globally depending on your [NPM configuration](https://docs.npmjs.com/getting-started/fixing-npm-permissions).

After installing the Vue.js CLI, you will use it to generate a new Vue.js application based on the [Webpack template](https://github.com/vuejs-templates/webpack). Inside the Django project's root, run the following command:

```bash
# initialize Vue.js with the Webpack template
vue init webpack frontend
```

Vue.js will ask a bunch of questions while creating your project. You can answer them as follows:

* *Project name:* frontend
* *Project description:* A Vue.js project
* *Author:* Your Name <your.name@somewhere.com>
* *Vue build:* Runtime + Compiler: recommended for most users
* *Install vue-router?* Yes
* *Use ESLint to lint your code?* Yes
* *Pick an ESLint preset:* Standard
* *Set up unit tests:* No
* *Setup e2e tests with Nightwatch?* No
* *Should we run `npm install` for you after the project has been created? (recommended)* Yes, I use NPM

Then, to run the initial Vue.js project, you can run the following code:

```bash
# change working directory to the front-end
cd frontend

# run the NPM dev script
npm run dev
```

After that, you should be able to visit the Vue.js application in your browser by navigating to [http://127.0.0.1:8080](http://127.0.0.1:8080).

![Vue.js basic application](https://cdn.auth0.com/blog/django-vuejs/vuejs-basic-app.png)

## Introduction to JWT

[JWT](https://jwt.io/) stands for JSON Web Token and it's simply a JSON (JavaScript Object Notation) object that contains claims. Here it is the official definition in the [RFC 7519](https://tools.ietf.org/html/rfc7519):

>JSON Web Token (JWT) is a compact, URL-safe means of representing claims to be transferred between two parties.  The claims in a JWT are encoded as a JSON object that is used as the payload of a JSON Web Signature (JWS) structure or as the plaintext of a JSON Web Encryption (JWE) structure, enabling the claims to be digitally signed or integrity protected with a Message Authentication Code (MAC) and/or encrypted.

JWTs are composed of three parts: a header, a payload, and a signature. The header contains informations such as the algorithm used while signing the payload and the header (e.g `RS256`) and the payload holds the claims. To learn more about JWTs, [take a look into this reference](https://auth0.com/docs/jwt).

## Creating an Auth0 Resource/API

Before you can use Auth0 authentication with your application, you first need to create an Auth0 account. If you haven't so yet, <a href="https://auth0.com/signup" data-amp-replace="CLIENT_ID" data-amp-addparams="anonId=CLIENT_ID(cid-scope-cookie-fallback-name)">sign up for a free account now</a>. After that, head over to the [API section](https://manage.auth0.com/#/apis) of the Auth0 dashboard and click on the *CREATE API* button.

You'll be presented with a form to fill in your API details: the name, the identifier, and the signing algorithm.

![Auth0 Create API Form](https://cdn.auth0.com/blog/django-vuejs/auth0-api-form.png)

You can use the following properties while filling this form:

* *Name:* Django Vue.js API
* *Identifier:* http://djangovuejsapi.digituz.com.br
* *Signing Algorithm:* RS256

After that, click on the *Create* button. You'll be taken to a page where you can further customize your API settings.

That's it! You are now ready to integrate your Django application with Auth0

## Integrating Django with Auth0

In this section, you will see how to secure the Django REST API with Auth0.

In the next part, you will build the API, but before that you will add JWT authentication to your back-end using Auth0.

For this reason, you will need to install the [Django REST framework](http://www.django-rest-framework.org/). You will also need to install the `djangorestframework-jwt` package for handling JWT authentication. You will setup `djangorestframework-jwt` to use Auth0 for signing the JWT tokens.

To do that, head back to your terminal, make sure the virtual environment we previously created is activated, and then run the following commands to install both packages using `pip`:

```bash
pip install djangorestframework
pip install djangorestframework-jwt
pip install cryptography
pip install python-jose
```

> You may need to install `python3-dev` if you get problems while installing the cryptography package.

Next, you'll need to add these packages to the list of installed apps in `settings.py`

```python
INSTALLED_APPS = [
    #...
    'rest_framework',
    'rest_framework_jwt'
]
```

You will also need to the `REST_FRAMEWORK` application definition to `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
       'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
    ),
}
```

Next, make sure to import the following dependencies in your `settings.py` file:

```python
import json
from six.moves.urllib import request
from cryptography.x509 import load_pem_x509_certificate
from cryptography.hazmat.backends import default_backend
```

Then, add the following code in the end of this file:

```python
AUTH0_DOMAIN = '<YOUR_AUTH0_DOMAIN>'
API_IDENTIFIER = '<YOUR_API_IDENTIFIER>'
PUBLIC_KEY = None
JWT_ISSUER = None
if AUTH0_DOMAIN:
    jsonurl = request.urlopen('https://' + AUTH0_DOMAIN + '/.well-known/jwks.json')
    jwks = json.loads(jsonurl.read().decode('utf-8'))
    cert = '-----BEGIN CERTIFICATE-----\n' + jwks['keys'][0]['x5c'][0] + '\n-----END CERTIFICATE-----'
    certificate = load_pem_x509_certificate(cert.encode('utf-8'), default_backend())
    PUBLIC_KEY = certificate.public_key()
    JWT_ISSUER = 'https://' + AUTH0_DOMAIN + '/'
def jwt_get_username_from_payload_handler(payload):
    return 'auth0user'
JWT_AUTH = {
    'JWT_PAYLOAD_GET_USERNAME_HANDLER': jwt_get_username_from_payload_handler,
    'JWT_PUBLIC_KEY': PUBLIC_KEY,
    'JWT_ALGORITHM': 'RS256',
    'JWT_AUDIENCE': API_IDENTIFIER,
    'JWT_ISSUER': JWT_ISSUER,
    'JWT_AUTH_HEADER_PREFIX': 'Bearer',
}
```

Make sure to replace `<YOUR_AUTH0_DOMAIN>` with your own Auth0 domain and `<YOUR_API_IDENTIFIER>` with the identifier used while creating the Auth0 API.

The public key is retrieved from `https://YOUR_AUTH0_DOMAIN/.well-known/jwks.json` using the `request.urlopen()` method. This key is then assigned to `PUBLIC_KEY` in `JWT_AUTH` (the settings object for `djangorestframework-jwt`).

`JWT_ISSUER` is your Auth0 domain name prefixed by https.

`JWT_ALGORITHM` needs to be set to **RS256**. That means that you also need to make sure your Auth0 API is using **RS256**.

The `JWT_AUTH_HEADER_PREFIX` is set to **Bearer**. This is the default prefix used by most frameworks (`djangorestframework-jwt` uses a **JWT** prefix by default).

You also have set `JWT_PAYLOAD_GET_USERNAME_HANDLER` to a custom method. This tells `djangorestframework-jwt` to use your custom method in order to map the username from the **access_token** payload to the Django user.

For most cases, you don't need to store users in your database since Auth0 handles all of that for you including advanced features such as profiles. You can use a custom function that checks if a general (can be fake) user that you create exists and map it to all Auth0 users.

To do that, head over to your application and create a user with any valid username such as *auth0user*. You can do that through Django admin interface or in a migration by adding the following code:

```bash
# create an empty data migration
python manage.py makemigrations --empty catalog
```

This will generate the `catalog/migrations/0001_initial.py` file. In this file, you can add the following code:

```python
from django.db import migrations
from django.conf import settings

def create_data(apps, schema_editor):
    User = apps.get_model(settings.AUTH_USER_MODEL)
    user = User(pk=1, username="auth0user", is_active=True , email="admin@techiediaries.com")
    user.save()

class Migration(migrations.Migration):
    dependencies = [
    ]
    operations = [
        migrations.RunPython(create_data),
    ]
```

> For more information about migrations, see [the official docs](https://docs.djangoproject.com/en/2.0/topics/migrations/#data-migrations).

Next, just run migrate to create your initial data:

```bash
python manage.py migrate
```

This is the custom method that maps the JWT payload with the *auth0user* user:

```python
def jwt_get_username_from_payload_handler(payload):
    return 'auth0user'
```

Please, note that you can return the username of any existing user in the database because you are not going to use this for anything besides the sole purpose of letting `djangorestframework-jwt` successfully authenticate the user.

### Adding Django Views

To make sure the JWT authentication via Auth0 is working, you can add these two view functions in `catalog/views.py`:  

```python
from rest_framework.decorators import api_view
from django.http import HttpResponse

def public(request):
    return HttpResponse("You don't need to be authenticated to see this")


@api_view(['GET'])
def private(request):
    return HttpResponse("You should not see this message if not authenticated!");
```

Also, make sure to create the corresponding URLs in `urls.py`. You can replace the whole contents of this file with this:

```python
from django.conf.urls import url
from catalog import views


urlpatterns = [
    url(r'^api/public/', views.public),
    url(r'^api/private/', views.private)
]
```

If you rerun your Django project now (`python manage.py runserver`), you will have access only to the [http://127.0.0.1:8000/api/public/](http://127.0.0.1:8000/api/public/) URL. The other one, the `/api/private/`, will need a JWT to be accessed.

## Integrating Auth0 with The Vue.js Front-end

In this section, you will see how you can add Auth0 authentication to your front-end Vue.js application. You will also add a button to use the access token, retrieved from Auth0, to allow users to fetch the message from the `/api/private/` endpoint.

### Creating an Auth0 Client

To represent your Vue.js front-end application on Auth0, you will need to create an Auth0 client. To do that, go to your [Auth0 dashboard](https://manage.auth0.com/) and click on the *New Client* button.

You will be presented with a page where you can enter the name of the client and the type of the client. You can fill the name of the application as *Django Vue.js App* and choose the *Single Page Web Applications* type.

![Creating an Auth0 Client](https://cdn.auth0.com/blog/django-vuejs/auth0-client.png)

After filling this form, click on the *Create* button. This will redirect you to a page where you can find the *Settings* tab.

### Configuring the Callback URL

You will need to whitelist the callback URL for your app (`http://localhost:8000/`) in the *Allowed Callback URLs* field of your new Auth0 client. This can be done in the *Settings* tab. In this tab, find the field mentioned and add `http://localhost:8000/` to it. Then click on the *Save* button.

### Creating the Authentication Service

Now. you will focus on integrating Auth0 in your Vue.js application. To do that, you will have to install the [`auth0.js`](https://github.com/auth0/auth0.js) and the `eventemitter3` libraries:

```bash
# change directory to frontend
cd frontend

# install dependencies
npm install --save auth0-js
npm install --save eventemitter3
```

You also need to install Axios to send AJAX requests to the Django back-end:

```bash
npm install --save axios
```

After installing these three dependencies, create a new file under `./frontend/src/auth/`. You will call this file as `AuthService.js` and add the following code:

```js
import auth0 from 'auth0-js'
import EventEmitter from 'eventemitter3'
import router from './../router'

export default class AuthService {
  authenticated = this.isAuthenticated();
  authNotifier = new EventEmitter();

  constructor() {
    this.login = this.login.bind(this);
    this.setSession = this.setSession.bind(this);
    this.logout = this.logout.bind(this);
    this.isAuthenticated = this.isAuthenticated.bind(this);
    this.handleAuthentication = this.handleAuthentication.bind(this);
  }

  // create an instance of auth0.WebAuth with your
  // API and Client credentials
  auth0 = new auth0.WebAuth({
    domain: '<YOUR_AUTH0_DOMAIN>',
    clientID: '<YOUR_CLIENT_ID>',
    redirectUri: '<YOUR_CALLBACK_URL>',
    audience: '<YOUR_AUDIENCE>',
    responseType: 'token id_token',
    scope: 'openid profile'
  });

  // this method calls the authorize() method
  // which triggers the Auth0 login page
  login() {
    this.auth0.authorize();
  }

  // this method calls the parseHash() method of Auth0
  // to get authentication information from the callback URL
  handleAuthentication() {
    this.auth0.parseHash((err, authResult) => {
      if (authResult && authResult.accessToken && authResult.idToken) {
        this.setSession(authResult);
      } else if (err) {
        console.log(err);
        alert(`Error: ${err.error}. Check the console for further details.`);
      }
      router.replace('/');
    })
  }

  // stores the user's access_token, id_token, and a time at
  // which the access_token will expire in the local storage
  setSession(authResult) {
    // Set the time that the access token will expire at
    let expiresAt = JSON.stringify(
      authResult.expiresIn * 1000 + new Date().getTime()
    );
    localStorage.setItem('access_token', authResult.accessToken);
    localStorage.setItem('id_token', authResult.idToken);
    localStorage.setItem('expires_at', expiresAt);
    this.authNotifier.emit('authChange', {authenticated: true})
  }

  // remove the access and ID tokens from the
  // local storage and emits the authChange event
  logout() {
    localStorage.removeItem('access_token');
    localStorage.removeItem('id_token');
    localStorage.removeItem('expires_at');
    this.authNotifier.emit('authChange', false);
    // navigate to the home route
    router.replace('/')
  }

  // checks if the user is authenticated
  isAuthenticated() {
    // Check whether the current time is past the
    // access token's expiry time
    let expiresAt = JSON.parse(localStorage.getItem('expires_at'));
    return new Date().getTime() < expiresAt;
  }

  // a static method to get the access token
  static getAuthToken() {
    return localStorage.getItem('access_token');
  }

  //a method to get the User profile
  getUserProfile(cb) {
    const accessToken = localStorage.getItem('access_token');
    if (accessToken) return this.auth0.client.userInfo(accessToken, cb);
    else return null;
  }
}
```

You will need to replace `<YOUR_AUTH0_DOMAIN>`, `<YOUR_CLIENT_ID>`, `<YOUR_CALLBACK_URL>`, and `<YOUR_AUDIENCE>` with the values from your client and API settings. The audience property refers to the identifier of your Auth0 API (i.e. if you followed the instructions, it will be `http://djangovuejsapi.digituz.com.br`).

### Creating the Homepage

Now, you are going to refactor your Vue.js homepage to allow users to authenticate via Auth0 and to fetch public and private messages. To do that, open `./frontend/src/App.vue` and replace the `<template>` code with this:

```html
<template>
  <div>
    <button
      class="btn btn-primary btn-margin"
      v-if="!authenticated"
      @click="login()">
      Log In
    </button>

    <button
      class="btn btn-primary btn-margin"
      v-if="authenticated"
      @click="private()">
      Call Private
    </button>

    <button
      class="btn btn-primary btn-margin"
      v-if="authenticated"
      @click="logout()">
      Log Out
    </button>
    {{message}}
    <br>
  </div>
</template>
```

### Creating the Methods

Still in your `./frontend/src/App.vue`, replace the `<script>` tag with this:

```html
<script>
  import AuthService from './auth/AuthService';
  import axios from 'axios';

  const API_URL = 'http://localhost:8000';
</script>
```

This imports the `AuthService` from `./auth/AuthService` and the `axios` library. After that, it declares the *API_URL* constant which holds the URL of the back-end server.

After the `API_URL` constant, create an instance of `AuthService`:

```js
const auth = new AuthService();
```

Then, below the `auth` definition, start defining the app component:

```js
export default {
  name: 'app',
  data() {
    this.handleAuthentication();
    this.authenticated = false;

    auth.authNotifier.on('authChange', authState => {
      this.authenticated = authState.authenticated
    });

    return {
      authenticated: false,
      message: ''
    }
  },
}
```

In this code, you are listening for the `authChange` event emitted by `AuthService` when the authentication state changes. You are then assign the result to the `authenticated` variable. You also set the `message` variable to be empty. This variable will hold the response from your protected endpoint.

The last thing you need to do is to define the `methods` property of the `app` component:

```js
export default {
    // name: 'app',
    // data() { ... },
    methods: {
      // this method calls the AuthService login() method
      login() {
        auth.login();
      },
      handleAuthentication() {
        auth.handleAuthentication();
      },
      logout() {
        auth.logout();
      },
      private() {
        const url = `${API_URL}/api/private/`;
        return axios.get(url, {headers: {Authorization: `Bearer ${AuthService.getAuthToken()}`}}).then((response) => {
          console.log(response.data);
          this.message = response.data || '';
        });
      }
    }
  }
```

The `login()`, `handleAuthentication()`, and `logout()` methods are simply wrappers for the corresponding methods in AuthService.

In the `private()` method, you are now using the `axios.get()` method to send a GET request to `http://localhost:8000/api/private/`. Since this endpoint is protected by Auth0, you also added an `Authorization` header. The access token is retrieved from the local storage using `AuthService.getAuthToken()` method.  

## Testing the Django, Vue.js, and Auth0 Integration

You'll be redirected to Auth0 central login page

![Auth0 auth page](https://screenshotscdn.firefoxusercontent.com/images/b59a1a2e-70a6-460a-8626-e628ed0fd02b.png)


After authentication you'll be able to send a request to the protected endpoint

![page after authentication](https://screenshotscdn.firefoxusercontent.com/images/61fa4449-c16a-4c2c-9a8e-7fc40f135251.png)


If the request is successfully sent you should get this massage `You should not see this message if not authenticated!`

![private message](https://screenshotscdn.firefoxusercontent.com/images/1a9c8aef-ce66-46c8-85cd-26042f47244f.png)


## Conclusion and Next Steps


In this article we have bootstrapped both the back-end project with Django and the front-end application using the Vue CLI. We have also added JWT authentication to our back-end using Auth0. In the next part we will see how to create the REST API using Django REST framework and then how to consume it from the Vue.js front-end using Axios. We'll also see how to create our project front-end views so stay tuned!

These are some screenshots from the demo project we are going to continue building in the next parts:

![demo app](https://camo.githubusercontent.com/108cac1baf92e9ea68ec1326fd2cb2292266a718/68747470733a2f2f73637265656e73686f74732e66697265666f7875736572636f6e74656e742e636f6d2f696d616765732f36323663303262302d616363622d343561362d623430622d6565633465613331333337342e706e67)

![demo app](https://camo.githubusercontent.com/7085b3824c7ab564a09e761f0e5cf58f05123f25/68747470733a2f2f73637265656e73686f74732e66697265666f7875736572636f6e74656e742e636f6d2f696d616765732f37386131393135322d306565302d346462632d383636622d3930393864346533626534342e706e67)
