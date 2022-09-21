---
track: "Frontend Fundamentals"
title: "Array Methods with Callbacks"
week: 3
day: 3
type: "lecture"
---

# Array Methods with Callbacks

## Lesson Objectives

- Define and understand the various different callback methods that can be used on an array.
- Understand what each method does and when we might want to use it.

### With food first!

<img src="https://i.redd.it/yf7rw3pjiapx.jpg" width=400/>

<br><br>

## Getting Started

Let's begin with a basic for loop:

```javascript
const iceCreams = ['Vanilla','Chocolate','Strawberry','Rocky Road'];
for(let i = 0; i < iceCreams.length; i++){
    console.log(iceCreams[i]);
}
```

This whole thing with `i` and `iceCreams[i]` is kind of obnoxious.  Wouldn't it be nice if we could just deal with the element in the array, instead of indexes?  

## Creating a ForEach function
Let's create a function that takes two parameters: 
- `array` - the list oof things we want to loop over
- `function` - that we want to be called on each element in the array:

```hs
// CREATE THE FUNCTION EXPRESSION
const forEach = (array, callback) => {
    for(let i = 0; i < array.length; i++){
        callback(array[i]);
    }
}

// INVOKE FOREACH
forEach(
    ['Vanilla','Chocolate','Strawberry','Rocky Road'],
    (currentArrayElement) => {
        console.log(currentArrayElement + ' ice cream');
    }
);
```



#### The .forEach Array Method

Great, but writing the definition of `forEach` is a pain.  

Fortunately, JavaScript has something like this already built in, the `.forEach` method.

```javascript
const iceCreams = ['Vanilla','Chocolate','Strawberry','Rocky Road'];
iceCreams.forEach((currentElement)=>{
    console.log(currentElement);
})
```

There are lots of other handy functions like `forEach`, such as `map`, `filter`, and `reduce` to just name a few.  These functions are called `Array Methods`.

**Note:** Any time you pass a function another function as an argument the initial function is considered a `Higher Order` functionn. 

<hr>

#### <g-emoji class="g-emoji" alias="alarm_clock" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/23f0.png">⏰</g-emoji> Activity - 3min

These array methods are so useful that people write articles about them all the time.  Let's take a look at one of them:

[5 Array Methods That You Should Be Using Now](https://colintoh.com/blog/5-array-methods-that-you-should-use-today)


<hr>



## What is an Array Method with a Callback?

You'll notice that even though `iceCreams` is an array, it functions like an object as well.  Consequently, you can add methods to them, just like normal objects (don't forget, `array.length` is a property of the array).

JavaScript has lots of pre-defined array methods available for us.  Each array method accepts a callback function which is executed on each of the elements in the array and may or may not do something with the results of that function.

In the previous example, `forEach` simply calls the callback function, passing in the current element as a parameter.


## Creating a Map function

Lets take a look at another array method called `map`.  In the previous example, `forEach` iterated over each of its elements and ran the given callback on each of them.  

What if we wanted to modify each element?  How would we write that?

```javascript
const map = (array, callback) => {
    const newArray = []
    for (let i=0; i < array.length; i++) {
        const newElement = callback(array[i]);
        newArray.push(newElement);
    }
    return newArray;
}

const resultArray = map(
    ['Vanilla','Chocolate','Strawberry','Rocky Road'],
    (currentArrayElement) => {
        return currentArrayElement + " ice cream";
    }
)

console.log(resultArray);
```

### The .map Array Method

Again, the implementation of this function is tricky.  Lucky for us we have the `.map` method.

```javascript
const iceCreams = ['Vanilla', 'Chocolate', 'Strawberry', 'Rocky Road'];

const updatedIceCreams = iceCreams.map((flavor)=>{
    return flavor + " Ice Cream";
});

console.log(updatedIceCreams);
```

`map` calls a provided callback function once for each element in an array, in order, and constructs a new array from the return values.

:question: **Question:** Does the `map` method mutate the original array?

### Lets try that again!

Use the `map` method with the following array to multiply each item by 2 and log the new array.

```javascript
const orinalArray = [2,4,6,8,10];

const newNumArray = orinalArray.map((num) => {
    return num * 2
})

console.log(newNumArray);
```

What was the result?

```javascript
[4, 8, 12, 16, 20]
```

**Small Refactor**

Since the fat arrow callback function is running a single like of code we can write it as a one liner.

Also take note that since we aren't using curly braces we no longer need the return keyword as a fat arrow function written as a one liner includes an `implicit` return under the hood.

```js
const newNumArray = orinalArray.map((num) => num * 2)
```

Also since there is only 1 parameter we can remove the parens around `num`


```js
const newNumArray = orinalArray.map( num => num * 2)
```

<hr>

#### <g-emoji class="g-emoji" alias="alarm_clock" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/23f0.png">⏰</g-emoji> Activity - 5min

You have been provided the following array of names

```js
const names = ['joe', 'alex', 'kenny', 'stack'];
```

- Using .map, return a new array of all the names converted to upper case

```js
=> ['JOE', 'ALEX', 'KENNY', 'STACK'];
```

**Bonus**

- Using Array.map, return a new array of all the names but only the first letter or each word is upper case

```js
=> ['Joe', 'Alex', 'Kenny', 'Stack']
```

<hr>

[5 Array Methods That You Should Be Using Now](https://colintoh.com/blog/5-array-methods-that-you-should-use-today)


<hr>

### Filtering Items

Another useful array method is `.filter`.  Let's use it to filter the `mappedNumbers` array and return only even numbers.

```js
const filteredNumbers = mappedNumbers.filter( element => element % 2 === 0 )
```

<hr>

#### <g-emoji class="g-emoji" alias="alarm_clock" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/23f0.png">⏰</g-emoji> Activity - 5min

You have been provided the following array of colors

```js
const favColor = ["green","blue","yellow",'blue','purple','blue'],
```
- Using .filter, return a new array  of all the colors that with a name of 'blue' 

```js
=> ['blue', 'blue', 'blue']
```

<hr>

### Method chaining & using declared functions

When using these array methods we can method chain. 

So instead of doing:

```javascript
const numbers = [1, 2, 3, 4, 5];

const mappedNumbers = numbers.map( element => element + 1 )

const filteredNumbers = mappedNumbers.filter( element => element % 2 === 0 )

console.log(filteredNumbers)

```

We can do:

```javascript
const filteredAndMappedNumbers = numbers
  .map( element  => element + 1)
  .filter( element  => element % 2 === 0)
```

Furthermore, we don't have to inline the anonymous functions, we can declare them elsewhere:

```javascript
const numbers = [1, 2, 3, 4, 5];

const add1 = num => num + 1 

const isEven = num => num % 2 === 0

const result = numbers.map(add1).filter(isEven);

console.log(result)
```

<hr>

#### <g-emoji class="g-emoji" alias="alarm_clock" fallback-src="https://github.githubassets.com/images/icons/emoji/unicode/23f0.png">⏰</g-emoji> Activity - 5min

You have been provided the following list of students:

```js
  let students = [
    {name: 'John', grade: 8, gender: 'M'},
    {name: 'John', grade: 8, gender: 'M'},
    {name: 'Bob', grade: 3, gender: 'M'},
    {name: 'Johnny', grade: 2, gender: 'M'},
    {name: 'Ethan', grade: 4, gender: 'M'},
    {name: 'Paula', grade: 6, gender: 'F'},
    {name: 'Donald', grade: 5, gender: 'M'},
    {name: 'Jennifer', grade: 13, gender: 'F'},
    {name: 'Courtney', grade: 15, gender: 'F'},
    {name: 'Jane', grade: 9, gender: 'F'}
]
```

- Using .filter, return a new array of only those students with a grade of 8 or higher

```js
=> [
{name: 'John', grade: 8, gender: 'M'},
{name: 'John', grade: 8, gender: 'M'},
{name: 'Jennifer', grade: 13, gender: 'F'}
{name: 'Courtney', grade: 15, gender: 'F'},
{name: 'Jane', grade: 9, gender: 'F'}
]
```

**Bonus**

- Using method chaining return a new array of only those students with a grade of 8 or higher AND increase their grade by 1 as they have all graduated to the next level

```js
=> [
{name: 'John', grade: 9, gender: 'M'},
{name: 'John', grade: 9, gender: 'M'},
{name: 'Jennifer', grade: 14 gender: 'F'}
{name: 'Courtney', grade: 16, gender: 'F'},
{name: 'Jane', grade: 10, gender: 'F'}
]
```

<hr>



## Additional Array Methods To Explore

For the lab you will use the following methods:

- forEach()
- map()
- filter()
- reduce() - possibly

Theare a quite a few additional methoods that you should familiarize yourself with as well. 

- indexOf
- includes
- find
- sort 
- every
- some


## Resources

- [array-methods-explained](https://codeburst.io/array-methods-explained-filter-vs-map-vs-reduce-vs-foreach-ea3127c6d319)



## 7. Further Study

### The Browser's Event Loop

Here's a great video on how asynchronous operations, such as the handling of events, do their business and notify JavaScript when their work is done.

[The Event Loop](https://www.youtube.com/watch?v=cCOL7MC4Pl0) (in this video, the amazing and funny Jake Archibald from Google does an amazing job demonstrating the browser's event loop).