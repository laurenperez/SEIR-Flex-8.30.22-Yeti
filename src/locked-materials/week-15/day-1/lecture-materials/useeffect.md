---
track: "React Fundamentals"
title: "Component LifeCycle and UseEffect"
week: 15
day: 1
type: "lecture"
---


[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# The Component Life Cycle

So far, we've used react components to build simple applications. We've added
state and props and controlled data flow through them (using just the `render`
and `setState` methods). In order to do more complex things, we'll have to use
life cycle methods.

## Prerequisites

- React
- Components
- State and props

## Objectives

By the end of this, developers should be able to:

- Explain how to use React's life cycle methods
- Use asynchronous functions within react
- Retrieve data from an API inside of a component

## Introduction

How do we get data from an API? Well we could drop in an AJAX call to fetch some
data, but our component would likely render before the AJAX request finished.
Our component would see that our data is `undefined` and either render a
blank/empty component or throw an error.

How do we incorporate third party libraries like `fetch` or `axios` with React?
It sounds complicated... Do we put that in render?

This lesson will introduce the Component Life Cycle: hooks that are fired at
different states of a components "life" for solving the problems described
above, as well as many others.

So, what is the Component Life Cycle?

## The Component Life Cycle

### The Life Cycle Methods (10 min / 0:20)

When we create a react component we get a couple of lifecycle methods included
that we can use to add functionality to our components. These methods are
invoked at specific periods during the "life" of a component, like when it
mounts to the DOM or unmounts from the DOM. While there are a lot of lifecycle
methods, there are only a few that you will use regularly.

Here is a good diagram of the [Lifecycle Methods](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/)

There are three types of component lifecycle methods:

**Mounting:** called when a component is created and inserted into the DOM.

- **useState()**
- **`render()`**
- **useEffect(() => {}, [])**  / **componentDidMount()**

**Updating:** usually triggered by changes in props or state.

- **useState()**
- **`render()`**
- **useEffect(() => {})**  (No [] included) / **componentDidUpdate()** - always runs
- **useEffect(() => {}, [someStateValueToMonitor])** / **componentDidUpdate()** - runs only if the value has changed


**Unmounting:** called when a component is being removed from the DOM.

- **useEffect(() => {})**  (No [] included) / **componentDidUnmount()**

> the **bold** methods are the most commonly used ones and the ones we'll focus
> on for this lesson

### ComponentDidMount

Here useffect is used to run only once when the component mounts for the first time. It fetches the movieTitle that was stored in state when the app loaded for the first time.  In order for this to work properly we must make sure that useState does indeed have an initial value set. 

```js
import React, { useState, useEffect } from 'react';

export default function App() {

  const [movieData, setMovieData] = useState({});
  const [movieTitle, setMovieTitle] = useState('star wars')

  useEffect(() => {
    const movieUrl = `https://www.omdbapi.com/?t=${movieTitle}&apikey=98e3fb1f`;
    const makeApiCall = async () => {
      const res = await fetch(movieUrl)
      const json = await res.json()
      setMovieData(json)
    }
    makeApiCall()
  }, [])
  
 }
  
```

### ComponentDidUpdate

Here useffect will run anytime the movieTitle has been changed in state.  When movieTitle has been changed then the `makeApiCall()` function is exectued.  If fetching the data has been successful then it will update `setMovieData` to the returned data. 

```js
import React, { useState, useEffect } from 'react';

function Example() {
  const [movieData, setMovieData] = useState({});
  const [movieTitle, setMovieTitle] = useState('star wars')
  
  useEffect(() => {
    const movieUrl = `https://www.omdbapi.com/?t=${movieTitle}&apikey=98e3fb1f`;
    const makeApiCall = async () => {
      const res = await fetch(movieUrl)
      const json = await res.json()
      setMovieData(json)
    }
    makeApiCall()
  }, [movieTitle])
}

```

Review the documentation on
[The Component Life Cycle](https://reactjs.org/docs/react-component.html#the-component-lifecycle).
[The useEffect Hook](https://reactjs.org/docs/hooks-effect.html)