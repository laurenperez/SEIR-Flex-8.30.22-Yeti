# React Cities

## Intro To State Exercise

So far you've learned the following about React:

- Creating and nesting Components
- Passing props and how to using them in JSX
- Importing and setting up state
- Updating state and re-rendering the Component
- Adding and calling event listeners

Now it's time to put it all together. At a high level you will do the following:

> Using only a single App Component you will implement the logic that allows a user to click on one of 4 small images and then update the DOM to display that image as the large image.

<img src="https://i.imgur.com/RVEofv5.jpg" width=200/>

## Working Version
Here is a [working version](https://codepen.io/jkeohan/live/850f8454693590e9772f8d0f6c2f44c8) of the app so you have a reference of the base functionality that you are being asked to implement. 

The solution above was implemented using jQuery so there is no React code to inspect via DevTools.  It is meant to provide a working example of the apps functionality. 

## Starter Code

[Download Starter Code]()

The starter code is in the ```react-cities-starter``` folder


## Instructions
For this exercise you will do the following:

#### App Component
- Examine the working live solution and determine the functionality needed
- Examine the HTML provided in `src/index.html` as this contains the HTML elements needed for the design
- Determine how best to organize the data needed to render the images
- Create a file called imageData.js that contains an array of image objects that are assigned keys of your own choosing, but must include the image url and alt values. 
- Using Array.map() loop over the data to create the small images based on the structure you decided
- Render the array of small image elements 
- Import `useState` into App
- Use one instance of `useState` to implement the logic.  
- Work out the remaining logic needed to implement the design

**Note:** All functionality must be placed within the App Component and no additional Components should be created for this solution to work. 

**Hint:** Try setting up state first and rendering the big image based on the value in state.  

**Hint:** Since you will be looping over an array of data, creating an image for each element and passing it the properties it needs, consider assigning the handleClick within the loop

**Hint:** Since you already have the value of the image src inside the loop perhaps you could pass the handleClick function the image url.  


### Bonus - Green Border

- Place a green border around the image to indicate that it is the current image being displayed.
- Any other previously active image will have it's border color removed

**Hint:** Since you already know how to assign a className AND know about ternary operators, try combining the two together and assign a class based on the result of a ternary operator. 

OR

**Hint:** Since you already know how to use the style prop  AND know about ternary operators, try combining the two together and assign a border based on the result of a ternary operator. 


## Bonus - Memory Game

Convert the following [CodePen](https://codepen.io/jkeohan/live/opvVGN) into a single React Component and implement the logic for click events and adding state.

Here is the dataset to use for the cards:

<details><summary>DataSet</summary>

```js
const cardBackgroundImage = 'https://res.cloudinary.com/jkeohan/image/upload/v1511808091/back_xldk5l.png'

const cardsArr = [
  {
    rank: "queen",
    suit: "hearts",
    cardImage: "https://res.cloudinary.com/jkeohan/image/upload/v1511808103/queen-of-hearts_nbvwls.png"
  },

  {
    rank: "queen",
    suit: "diamonds",
    cardImage: "https://res.cloudinary.com/jkeohan/image/upload/v1511808103/queen-of-diamonds_opxv6b.png"
  },

  {
    rank: "king",
    suit: "hearts",
    cardImage: "https://res.cloudinary.com/jkeohan/image/upload/v1511808103/king-of-hearts_njmwml.png"
  },

  {
    rank: "king",
    suit: "diamonds",
    cardImage: "https://res.cloudinary.com/jkeohan/image/upload/v1511808103/king-of-diamonds_mpn7sm.png"
  }
];
```

</details>

## Plagiarism

Take a moment to refamiliarize yourself with the
[Plagiarism policy](https://git.generalassemb.ly/DC-WDI/Administrative/blob/master/plagiarism.md).
Plagiarized work will not be accepted.

## License

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
