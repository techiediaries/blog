## Aside: Secure a Vuejs 2 App with Auth0

Securing applications with Auth0 is very easy and brings a lot of great features to the table. With Auth0, we only have to write a few lines of code to get solid [identity management solution](https://auth0.com/user-management),
[single sign-on](https://auth0.com/docs/sso/single-sign-on), support for [social identity providers (like Facebook, GitHub, Twitter, etc.)](https://auth0.com/docs/identityproviders), and support for [enterprise identity providers (Active Directory, LDAP, SAML, custom, etc.)](https://auth0.com/enterprise).

In the following sections, we are going to learn how to use Auth0 to secure Vuejs 2 apps. As we will see, the process is simple and fast.

### Setting Up a Client App

The first thing we need to secure our app is to register it as a [Client](https://auth0.com/docs/clients) on our Auth0 account. By the way, we have to create an account on Auth0. Luckily, <a href="https://auth0.com/signup" data-amp-replace="CLIENT_ID" data-amp-addparams="anonId=CLIENT_ID(cid-scope-cookie-fallback-name)">Auth0 has a free that supports 7,000 free active users & unlimited logins</a>!

After creating our free account, let's proceed as follows:

1. Let's go to our [**Auth0 Dashboard**](https://manage.auth0.com/#/) and click the "[create a new client](https://manage.auth0.com/#/clients/create)" button.
2. Let's call our app as "Vuejs Demo" and select "Single Page Web Applications".
3. In the **Settings** for our new Auth0 client app, let's add `http://localhost:8080/callback` to the **Allowed Callback URLs**.
4. If desired, we can [set up some social connections](https://manage.auth0.com/#/connections/social). We can then enable them for our app in the **Client** options under the **Connections** tab. The example shown in the screenshot above utilizes username/password database, Facebook, Google, and Twitter. For production, let's make sure to set up the correct social keys and do not leave social connections set to use Auth0 dev keys.

### Dependencies and Setup

The only dependency that we need is the [`auth0.js` package: a client side JavaScript toolkit for Auth0 API](https://github.com/auth0/auth0.js). To install it, let's issue `npm install --save auth0-js` in the project root.

After installing it, we can create an authentication service to interface with the `auth0.js` script. Let's call this service `Auth` and create it in the `src/Auth/` directory with the following code:

```javascript
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
