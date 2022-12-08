---
track: "React Fundamentals"
title: "Styling React Components"
week: 12
day: 1
type: "lecture"
---


# Intro to Styling React Components

<br>
<br>

## Learning Objectives

| Students Will Be Able To: |
|---|
| Include external fonts |
| Style components using imported CSS files |
| Style components using inline CSS in JSX |
| Style components using CSS in JS |
| Style components using Styled Components |
| Style components using CSS Modules |
| Style components using SASS |

<br>
<br>

## Roadmap

- Set Up
- Overview of Styling options in React
- The Starter Code
- Adding External Fonts to a React App
- Importing CSS Normal Stylesheets
- Inline Styling With JavaScript
- Using CSS in JS
- Using Styled Components
- Importing CSS Modules
- The world of SASS

- Essential Questions

<br>
<br>

## Overview of Styling in React

Like many things React, styling is done a little differently than what we've become accustomed to.

For example, when learning CSS we were told that inline styling should be avoided. Well, React actually encourages inline styling!

There are several popular methods for styling react components, what you choose to use in your deliverables is up to you. There is no true "industry standard" and exposure to multiple styles and examples is ideal. In a real job setting, you will just adopt whatever librarys or conventions are used in the existing codebase.
<br>
<br>

### TL:DR Some Options for Styling React Components

Today, we will look at a few of the more popular ways to style the components that comprise a React UI:

1. **CSS Stylesheets**: This is the approach that `create-react-app` uses with its `<App>` component by default.

2. **Inline Styling**: This approach uses the `style` prop.

3. **CSS in JS**: Requires a package: `npm install react-jss`
[React-JSS](https://cssinjs.org/react-jss/?v=v10.9.0) integrates JSS with React using the new Hooks API

4. **Styled Components**: Requires a package: `npm install styled-components`
[Styled-Components](https://www.npmjs.com/package/styled-components):  _"Utilising tagged template literals (a recent addition to JavaScript) and the power of CSS, styled-components allows you to write actual CSS code to style your components... Styled-components is compatible with both React (for web) and React Native – meaning it's the perfect choice even for truly universal apps!"_

5. **CSS Modules**: Similar to importing regular stylesheets, but different in a significant way - scoping. CSS files are included the same way but with the following file extension format `filename.module.css`. CSS written in this module will be scoped locally to the component that is importing it. This can help avoid class name collisions in large applications.

6. **SASS**: Requires a package: `npm install sass`
[Sass](https://sass-lang.com/): Sass is the most mature, stable, and powerful professional grade CSS extension language in the world.


<br>
<br>
<br>

#### Set Up - 5 min 

Create a new react app for todays codealong:

`npx create-react-app@latest react-css`

1. Add a directory called `components` inside `src`

2. Add the following directories inside of `components`:

  - example-1-normal-css
  - example-2-inline-css
  - example-3-css-in-js
  - example-4-styled-components
  - example-5-css-modules
  - example-6-sass


<br><br>

#### Creating the Button Components

We will be re-creating the same component file inside each of these example component folders. This will act as our "starter code" for each of today's examples. 

_Note:_ Typically purpose of components is reusability, however, because they all have different methods of incorporating thier styles and css - we will be recreating the same component for each example.

#### More Set Up - 2 min

Create a Button.jsx file in each of the example folders ( 6 total ).

Include the following code in your Button files:

```javascript
export default function Button({ buttonText }) {
  return (
    <button>{ buttonText }</button>
  );
}
```

Now lets clean up App.js and get it ready for our examples: 

`App.js`

```javascript
import "./App.css";
// import button components here

function App() {
  return (
    <div className="App">
      {/* Add example components here */}
    </div>
  );
}

export default App;
```

Lets also wipe out App.css and add some of our own default styles:

`App.css`



```css
/* import fonts  */ 


* {
  /* add custom fonts here */
}

.App {
  display: flex;
  flex-direction: column;
  gap: 30px;
  margin: 0 auto;
  width: 300px;
}
```

Lets go get a cool Roboto font from [google fonts](https://fonts.google.com/specimen/Roboto+Flex).

Import your fonts in `App.css` and add your font family styles to '*'.

<br>
<br> 

With each example we'll import a new button component to our `App.js` like this:

 ```javascript
 <ButtonOne text="Example 1" />
 ``` 

<br>
<br> 


#### Lets get styling...

<br>
<br>

For the following examples we'll be using the same set of base styles ( with minor variations ) in our button components. 

You'll want to refer back to these: 

<br>

```css
.default-button {
  background-color: white;
  border-radius: 8px;
  color: blue;
  font-size: 24pt;
  padding: 15px 50px;
  width: 300px;
}
```

<br>
<br>

#### Example 1 : Normal CSS Stylesheets ( This should be review )

<br>

1. Create a regular css style sheet in the example component folder
2. Import into your component file
3. Add classes in JSX using the JSX attribute `className`

<br>
<br>

✨ Let's give it a fancy background-color: #2465f1

✨ Bonus: Create a second class reversing the text color and background-color ( think themes! )

✨ Bonus: Refactor your component so that you can pass a class as props and change the look of your button from '<App>'

<br>
<br>

#### Review Questions

**❓ What attribute do we use in JSX to apply css classes to components?**

**❓ Can classes be passed down to components in props?**

<br>
<br>
<br>
<br>


#### Example 2 : Inline CSS

<br>

About Inline Styling

Contrary to what we've been told about avoiding styling elements inline using the `style` attribute in HTML, with React, inline styling is a common technique for **dynamic** styling.

Inline styling in React uses the `style` property, however, unlike the `style` attribute in HTML, we assign a JS object instead of a string.

1. Create a `buttonStyle` variable and set it equal to an obj. Add your default button styles as key value pairs to this object. 

_Note:_ Unlike normal CSS, property key names will need to be camelCased and their values wrapped in a string. Dont forget your commas!

2. Set the `style` attribute in your component jsx equal to your `buttonStyle` object.


```jsx

export default function Button({ text }) {

  const buttonStyle = {
    // add styles here
  }

  return (
    <button style={buttonStyle}>{ text }</button>
  );
}
```




<br>
<br>

✨  Let's give it a fancy background-color: #b974b6

✨  Bonus: Let's change one of the style property values to a variable

✨  Bonus: Refactor your component to accept a props value that will set the style property in your css

<br>
<br>

#### Review Questions

**❓ What attribute do we use in JSX to apply css styles directly to components?**

**❓ What data type does the style attribute require?**

<br>
<br>
<br>
<br>

#### Example 3 : CSS in JS

<br>

To use CSS in JS we will need to install the package: [react-jss](https://cssinjs.org/react-jss/?v=v10.9.0).

`npm install react-jss `

This library also uses javascript objects to describe styles in a declarative way. Its custom method ‘createUseStyles’ then incorporates those styles into functional components using the JSX className attribute.

```jsx
import { createUseStyles } from "react-jss";

export default function Button({ text }) {

  const styles = createUseStyles({
    button: {
      // add your styles as JS key : value pairs
    }
  });

  const classes = styles()

  return (
    <button className={ classes.button }>{ text }</button>
  );
}
```

<br>
<br>

✨  Let's give it a fancy background-color: #24292e  &  text color: #f7df1c

✨  Bonus: refector to accept props and add a prop style

<br>
<br>

#### Review Questions

**❓ What method does react-jss use to apply style objects?**

**❓ What jsx attribute is used to apply the style object created by react-jss?**

<br>
<br>
<br>
<br>

#### Example 4 : Styled Components

<br>

To use Styled Components we will need to install the package: [styled-components](https://www.npmjs.com/package/styled-components)

`npm install styled-components`

Utilising [tagged template literals](https://wesbos.com/tagged-template-literals) (a recent addition to JavaScript) and the power of CSS, styled-components allows you to write actual CSS code to style your components. It also removes the mapping between components and styles. [source](https://styled-components.com/docs)

```jsx
import styled from "styled-components";

export default function Button({ text }) {
  
  const StyledButton = styled.button`
    add styles here using css syntax
  `;

  return (
    <StyledButton>{ text }</StyledButton>
  );
}
```

<br>
<br>

✨  Let's give it a fancy background-color: #86ba8a  &  text color: #3d3d3d

✨  Bonus: refector to accept props and add a prop style

<br>
<br>

#### Review Questions

**❓ What syntax is used inside the tagged template literal?**

**❓ What is one benefit of using styled components?**

<br>
<br>
<br>
<br>

#### Example 5 : CSS Modules

<br>

CSS Modules became available with the release of **create-react-app v2.0**, which improved the configuration of Webpack.

With **CSS Modules**, a CSS file's **class names** will be made unique by the tooling and will be dedicated to the component that imports the CSS Module. 

No more worrying about class name collisions! 😄

Using a CSS Module differs from using a CSS stylesheet in three ways:

1. The filename ends with `module.css`, e.g., `button.module.css` instead of `button.css`.
2. The CSS Module is imported with the `from` syntax.
3. **Class selectors** are unique to the component.  Other selectors however become global CSS rules just like with CSS stylesheets.

Create a button.module.css file and add the default button styles from above.
Import your CSS module and reference its classes in your jsx as you would a normal stylesheet. 

```jsx

import styles from './button.module.css'

export default function Button({ text }) {
  return (
    <button className={ styles.cta }>{ text }</button>
  );
}

```

**Note that the class names become keys on the `styles` object - let's console.log it to check it out:**

<br>
<br>

✨  Let's give it a fancy background-color: #ffa500

✨  Bonus: refector to accept props and add a prop style

<br>
<br>

#### Review Questions

**❓ What's the difference when naming the files for CSS stylesheets vs. CSS Modules?**

**❓ True or False: Importing CSS Modules results in an object where the keys are the names of the classes we defined in the module and the values are the unique class names generated by the tooling.**

**❓ What's wrong with the following code:**

```jsx
import styles from './SmallComponent.module.css';

function SmallComponent(props) {
	return <div className="styles.small">I am small</div>;
}
```

<br>
<br>
<br>
<br>

#### Example 6 : Sass

<br>

*CSS on its own can be fun, but stylesheets are getting larger, more complex, and harder to maintain. This is where a preprocessor can help. Sass has features that don't exist in CSS yet like nesting, mixins, inheritance, and other nifty goodies that help you write robust, maintainable CSS.*

*Once you start tinkering with Sass, it will take your preprocessed Sass file and save it as a normal CSS file that you can use in your website...* [source](https://sass-lang.com/guide)

![sass](https://sass-lang.com/assets/img/styleguide/color-1c4aab2b.png)

We'll be using [this package](https://www.npmjs.com/package/sass) to install Sass.


This package is a distribution of Dart Sass, compiled to pure JavaScript with no native code or external dependencies. It provides a command-line sass executable and a Node.js API.

You can install Sass globally using `npm install -g sass` which will provide access to the sass executable. You can also add it to your project using `npm install --save-dev sass`

We'll be using the second option.

Create a new file in your component example folder called `button.sass`

Add your default button styles using the [sass syntax documentation](https://sass-lang.com/documentation/style-rules).

Import your sass file in your button component just like a normal css file. 

```jsx

import "./button.sass"

export default function Button({ text }) {
  return <button className="sassy-button">{ text }</button>;
}

```

Some fun things to explore in sass -

  -Variables
  -Nesting
  -Mixins
  -Functions
  -Partials & Importing
  -Inheritance
  -The '&' Operator
  -Control Directives (if else)
  -Interpolation
  -Placeholders

✨  Let's give it a fancy background-color: #ce649a

✨  Bonus: refector to accept props and add a prop style

<br>
<br>

#### Review Questions

**❓ What is sass?**

**❓ Are there extra tools required when using a CSS preprocessor like Sass?**

**❓ Why is sass so popular?**


<br>
<br>

**You now know 6 different ways to style react components!**

#### Which was your favorite?