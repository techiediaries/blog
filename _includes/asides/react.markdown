## Aside: Authenticate a React App with Auth0

We can protect our applications and APIs so that only authenticated users can access them. Let's explore how to do this with a React application using [Auth0](https://auth0.com).

![Auth0 centralized login screen](https://cdn.auth0.com/blog/resources/auth0-centralized-login.jpg)

We'll need an [Auth0](https://auth0.com) account to manage authentication. [To sign up for a free account, we can follow this link](https://auth0.com/signup). Next, let's set up an Auth0 client app and API so Auth0 can interface with a React App.

### Setting Up a Client App

1. Let's go to our [**Auth0 Dashboard**](https://manage.auth0.com/#/) and click the "[create a new client](https://manage.auth0.com/#/clients/create)" button.
2. Let's call our app as "React Demo" and select "Single Page Web Applications".
3. In the **Settings** for our new Auth0 client app, let's add `http://localhost:3000/callback` to the **Allowed Callback URLs**.
4. If desired, we can [set up some social connections](https://manage.auth0.com/#/connections/social). We can then enable them for our app in the **Client** options under the **Connections** tab. The example shown in the screenshot above utilizes username/password database, Facebook, Google, and Twitter. For production, make sure to set up the correct social keys and do not leave social connections set to use Auth0 dev keys.

### Dependencies and Setup

The only dependency that we need is the [`auth0.js` package: a client side JavaScript toolkit for Auth0 API](https://github.com/auth0/auth0.js). To install it, let's issue `npm install --save auth0-js` in the project root.

After installing it, we can create an authentication service to interface with the `auth0.js` script. Let's call this service `Auth` and create it in the `src/Auth/` directory with the following code:

```js
import history from '../history';
import auth0 from 'auth0-js';

export default class Auth {
  auth0 = new auth0.WebAuth({
    // the following three lines MUST be updated
    domain: 'bkrebs.auth0.com',
    audience: 'https://bkrebs.auth0.com/userinfo',
    clientID: '3co4Cdt3h3x8En7Cj0s7Zg5FxhKOjeeK',
    redirectUri: 'http://localhost:3000/callback',
    responseType: 'token id_token',
    scope: 'openid'
  });

  constructor() {
    this.login = this.login.bind(this);
    this.logout = this.logout.bind(this);
    this.handleAuthentication = this.handleAuthentication.bind(this);
    this.isAuthenticated = this.isAuthenticated.bind(this);
  }

  handleAuthentication() {
    this.auth0.parseHash((err, authResult) => {
      if (authResult && authResult.accessToken && authResult.idToken) {
        this.setSession(authResult);
        history.replace('/home');
      } else if (err) {
        history.replace('/home');
        console.log(err);
      }
    });
  }

  setSession(authResult) {
    // Set the time that the access token will expire at
    let expiresAt = JSON.stringify((authResult.expiresIn * 1000) + new Date().getTime());
    localStorage.setItem('access_token', authResult.accessToken);
    localStorage.setItem('id_token', authResult.idToken);
    localStorage.setItem('expires_at', expiresAt);
    // navigate to the home route
    history.replace('/home');
  }

  login() {
    this.auth0.authorize();
  }

  logout() {
    // Clear access token and ID token from local storage
    localStorage.removeItem('access_token');
    localStorage.removeItem('id_token');
    localStorage.removeItem('expires_at');
    // navigate to the home route
    history.replace('/home');
  }

  isAuthenticated() {
    // Check whether the current time is past the
    // access token's expiry time
    let expiresAt = JSON.parse(localStorage.getItem('expires_at'));
    return new Date().getTime() < expiresAt;
  }
}
```

The `Auth` service just created contains functions to deal with various steps of the sign in/sign up process. The following list briefly summarizes these functions and their descriptions:

- `handleAuthentication`: looks for the result of the authentication in the URL hash. Then, process the result with the `parseHash` method from `auth0-js`;
- `setSession`: sets the user's access token, ID token, and the access token's expiry time;
- `login`: initiates the login process, redirecting users to the Centralized Login page;
- `logout`: removes the user's tokens and expiry time from browser storage;
- `isAuthenticated`: checks whether the expiry time for the user's access token has passed;

Besides these functions, the class contains a field called `auth0` that is initialized with values extracted from the Auth0 client. Let's keep in mind that we need to update them accordingly before proceeding.

Attentive readers probably noticed that the `Auth` service also imports a module called `history` that we haven't talked about. We can define this module in only two lines, but let's define it in a file to provide reusability. Let's call this file `./src/history/history.js` and add the following code:

```js
import createHistory from 'history/createBrowserHistory'

export default createHistory()
```

After creating both elements, we can refactor our `App` component to make use of the `Auth` service.

```jsx
import React, { Component } from 'react';
import { Navbar, Button } from 'react-bootstrap';
import './App.css';

class App extends Component {
  goTo(route) {
    this.props.history.replace(`/${route}`)
  }

  login() {
    this.props.auth.login();
  }

  logout() {
    this.props.auth.logout();
  }

  render() {
    const { isAuthenticated } = this.props.auth;

    // ... render the view
  }
}

export default App;
```

Note that we are passing this service through `props`. Therefore, when including the `App` component, we need to inject `Auth` into it: `<App auth={auth} />`.

Considering that we are using the Auth0 Centralized Login page, our users are taken away from the application. However, after they authenticate, users automatically return to the callback URL that we set up previously (`http://localhost:3000/callback`). This means that we need to create a component responsible for this URL:

```jsx
import React, { Component } from 'react';
import loading from './loading.svg';

class Callback extends Component {
  render() {
    const style = //...

    return (
      <div style={style}>
        <img src={loading} alt="loading"/>
      </div>
    );
  }
}

export default Callback;
```

This component can be just a loading indicator that keeps spinning while the application sets up a client-side session for the users. After the session is set up, we can redirect users to another route.

Please, refer to [the official Quick Start Guide to see, step by step, how to properly secure a React application](https://auth0.com/docs/quickstart/spa/react/01-login). Besides the steps shown in this section, the guide also shows:

- [How to manage profile information of authenticated users](https://auth0.com/docs/quickstart/spa/react/02-user-profile).
- [How to properly call an API](https://auth0.com/docs/quickstart/spa/react/03-calling-an-api).
- [How to control which routes users can see/interact with](https://auth0.com/docs/quickstart/spa/react/04-authorization).
- [How to deal with expiry time of users' access token](https://auth0.com/docs/quickstart/spa/react/05-token-renewal).
