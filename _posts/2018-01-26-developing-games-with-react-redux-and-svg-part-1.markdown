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

Removing these files will probably generate an error if you try to execute you project. This is easily solved by removing two import statements from the `./src/App.js` file:

```js
// remove both lines from ./src/App.js
import logo from './logo.svg';
import './App.css';
```

And by refactoring the `render()` method to:

```js
// ... import statement and class definition
render() {
  return (
    <div className="App">
      <h1>We will create an awesome game with React, Redux, and SVG!</h1>
    </div>
  );
}

// ... closing bracket and export statement
```

Now is a good time to commit your files to Git or some other version control system (you do use a  version control system, right?).

## Installing Redux and PropTypes

After bootstrapping the React project and removing the useless files from it, you will want to install and configure [Redux](https://redux.js.org/) to be [the single source of truth on your application](https://redux.js.org/docs/introduction/ThreePrinciples.html#single-source-of-truth). You will also want to install [PropTypes](https://github.com/facebook/prop-types) as [this tool helps avoiding common mistakes](https://reactjs.org/docs/typechecking-with-proptypes.html). Both tools can be installed in a single command:

```bash
npm i redux react-redux proptypes
```

As you can see, the command above includes a third NPM package: `react-redux`. Although you could use Redux directly with React, this is not recommended. [The `react-redux` package does some performance optimizations](https://redux.js.org/docs/basics/UsageWithReact.html) that would be cumbersome to handle manually.

### Configuring Redux and Using PropTypes

With these packages in place, you can configure your app to use Redux. The process is simple, you will need to create a *container* component, a *presentational* component, and a *reducer*. The difference between container components and presentational components is that the first simply `connects` presentational components to Redux. The third element that you will create, a reducer, is the core component in a Redux store. This kind of component is responsible for getting *actions* triggered by events that occur in your application and applying functions to change state based on these actions.

> If you are not familiar with these concepts, you can read [this article to get a better explanation about presentational and container components](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0) and you can go through [this practical Redux tutorial to learn about *actions*, *reducers*, and the *store*](https://auth0.com/blog/redux-practical-tutorial/). Although learning about these concepts is highly recommended, you can still follow this series without reading about them.

You will be better off starting by creating the reducer, as this element does not depend on the others (actually, it's the other way around). To keep things organized, you can create a new directory called `reducers`, inside the `src` directory, and add to it a file called `index.js`. This file can contain the following source code:

```js
const initialState = {
  message: `It's easy to integrate React and Redux, isn't it?`,
};

function reducer(state = initialState) {
  return state;
}

export default reducer;
```

For now, your reducer will simply initialize the app's state with a `message` saying that it's easy to integrate React and Redux. Soon, you will start defining actions and handling them in this file.

Next, you can refactor the `App` component to show this message to users. As you installed `prop-types`, it's a good time to start using it as well. To achieve this, open the `./src/App.js` file and replace its contents with the following:

```js
import React, {Component} from 'react';
import PropTypes from 'proptypes';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h1>{this.props.message}</h1>
      </div>
    );
  }
}

App.propTypes = {
  message: PropTypes.string.isRequired,
};

export default App;
```

As you can see, defining what types your component is expecting is very easy with `prop-types`. You just have to define the `propTypes` property of the `App` component with the `props` that it needs. There are a few cheat sheets around the web (like [this one](https://lzone.de/cheat-sheet/React%20PropTypes), [this one](https://reactcheatsheet.com/), and [this one](https://devhints.io/react)) that summarize how to create basic and advanced `prop-types` definitions. If needed, refer to them.

Even though you have defined what the `App` component needs to render and what is the initial state of your Redux store, you still need a way to tie these elements together. That's exactly what *container* components do. To define a container in an organized fashion, you will want to create a directory called `containers` inside the `src` directory. Then, you can create a container called `Game` inside a file called `Game.js` in this new directory. This container will use the `connect` utility from `react-redux` to pass the `state.message` to the `message` props of the `App` component:

```js
import {connect} from 'react-redux';

import App from '../App';

const mapStateToProps = state => ({
  message: state.message,
});

const Game = connect(
  mapStateToProps,
)(App);

export default Game;
```

You are almost done now. The last step to integrate everything together is to refactor the `./src/index.js` file to initialize the Redux store and to pass it to the `Game` container (which will then fetch the `message` and pass to `App`). The following code shows how your `./src/index.js` file will look like after the refactoring:

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Provider } from 'react-redux';
import { createStore } from 'redux';
import './index.css';
import Game from './containers/Game';
import reducer from './reducers';
import registerServiceWorker from './registerServiceWorker';

/* eslint-disable no-underscore-dangle */
const store = createStore(
    reducer, /* preloadedState, */
    window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__(),
);
/* eslint-enable */

ReactDOM.render(
    <Provider store={store}>
        <Game />
    </Provider>,
    document.getElementById('root'),
);
registerServiceWorker();
```

You are done! To see everything working, you can head to the project root and run `npm start`. This will run your app in development mode and open it in your default browser.

{% include tweet_quote.html quote_text="It's easy to integrate React and Redux." %}

## Creating SVG Components with React

As you will see in this series, creating SVG components with React is quite easy. In reality, there is almost no difference between creating a React component with HTML and with SVG. Basically, the only differences are that SVG introduces new elements and that these elements are plotted in an SVG canvas.

Nevertheless, before creating your components with SVG and React, a quick review on SVG may be useful.

### Quick Review on SVG

SVG is one of the most cool and flexible web standards. SVG, which stands for Scalable Vector Graphics, is a markup language that allows developers to describe two dimensional based vector graphics. SVG is pretty similar to HTML. Both technologies are XML-based markup languages and work well with other web standards like CSS and the DOM. This means that you can apply CSS rules to SVG elements just like you would do with HTML elements, including animations.

Throughout this series, you will create more than a dozen SVG components with React. You will even compose (group) SVG elements to form your game elements (like the cannon that shoots cannon balls).

A thorough explanation about SVG is out of scope and would make this series too lengthy. So, if you are looking forward to learn the details of the SVG markup language, you can take a look at [the *SVG Tutorial* provided by Mozilla](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial) and at [this article about the SVG coordinate system](https://www.sarasoueidan.com/blog/svg-coordinate-systems/).

However, prior to start creating your components, there a few SVG characteristics that are important to understand. First, SVG and DOM enable developers to accomplish great things when combined. This makes using SVG with React very easy.

Second, the SVG coordinate system is similar to the Cartesian plane, but upside-down. This means that negative vertical values are, by default, shown above the X axis. The horizontal values, on the other hand, are just like the Cartesian plane (i.e. negative values are shown to the left of the Y axis). This behavior could be easily changed by [applying a transformation to the SVG canvas](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/transform). However, in order not to confuse any other developer, it's better to stick with the default. You will get used soon.

The third and last characteristic that you need to know is that SVG introduces a lot of new elements (e.g. `circle`, `rect`, and `path`). To use these elements, you cannot simply define them inside an HTML element. First, you must define an `svg` element (your canvas) where you will plot all your SVG components.

### SVG, Path Elements, and Cubic Bezier Curves

Drawing elements with SVG can be accomplished in three ways. First, you can use basic elements like `rect`, `circle`, and `line`. These elements are not very flexible, though. As their names state, they simply allow you to draw some simple shapes.

The second way is to combine these basic elements to form more complex shapes. For example, you could use a `rect` with equals sides (this would be a square) and two lines to form the shape of a  house. However, this approach is still limited.

The third and more flexible way is to use [`path` elements](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths). This kind of element allows developers to create fairly complex shapes. It does that by accepting a set of commands that instructs the browser how to draw a shape. For example, to draw an "L", you could create a `path` element that contains three commands:

1. `M 20 20`: this command instructs the browser to move its "pen" to the X and Y coordinates defined after `M` (i.e. `20, 20`);
2. `V 80`: this command instructs the browser to draw a line from the previous point to the position `80` in the Y axis;
3. `H 50`: this command instructs the browser to draw a line from the previous point to the position `50` in the X axis;

{% highlight html %}
{% raw %}
<svg>
  <path d="M 20 20 V 80 H 50" stroke="black" stroke-width="2" fill="transparent" />
</svg>
{% endraw %}
{% endhighlight %}

The `path` element accepts many other commands. Among of them, one of the most important is [the Cubic Bezier Curves command](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#Bezier_Curves). This command allows you to add some smooth curves in your path by taking two reference points and two control points.

From the Mozilla tutorial, this is how Cubic Bezier Curves work on SVG:

> _"Cubic Bezier curves take in two control points for each point. Therefore, to create a cubic Bezier curve, you need to specify three sets of coordinates. The last set of coordinates are where you want the line to end. The other two are control points. [...]. The control points essentially describe the slope of your line starting at each point. The Bezier function then creates a smooth curve that transfers you from the slope you established at the beginning of your line, to the slope at the other end."_ —[Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/SVG/Tutorial/Paths#Bezier_Curves)

For example, to draw an "U", you can proceed as follows:

{% highlight html %}
{% raw %}
<svg>
  <path d="M 20 20 C 20 110, 110 110, 110 20" stroke="black" fill="transparent"/>
</svg>
{% endraw %}
{% endhighlight %}

In this case, the commands passed to the `path` element tell the browser:

1. to start drawing on the point `20, 20`;
2. that the first control point lies on the point `20, 110`;
2. that the second control point lies on the point `110, 110`;
4. to finish the curve on the point `110 20`;

If you still don't understand exactly how Cubic Bezier curves work, don't worry. You will have the opportunity to practice during this series. Besides that, you can find a lot of tutorial around the web about this feature and you can always practice in tools like [JSFiddle](https://jsfiddle.net/) and [Codepen](https://codepen.io/).

### Creating the Canvas React Component

### Creating the Sky React Component

### Creating the Ground React Component

### Creating the Cannon React Component

### Making the Cannon Aim

## Next Steps
