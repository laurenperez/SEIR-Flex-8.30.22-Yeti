---
track: "React Fundamentals"
title: "Intro to AJAX and HTTP service"
week: 15
day: 1
type: "lecture"
---

# Intro to AJAX and HTTP Service

## Learning Objectives
- Describe what is AJAX
- Describe what are 3rd Party APIs and how they are used
- Describe what are API keys and how to use them
- Review Query Parameters and how they can be used in 3rd Party API requests
- Set up fetch for a React App
- Learn how to use fetch to make API requests
- Learn how to incorporate data from fetch requests into a SPA using React
- Learn to work with Async/Await as a way to handle Promises used in fetch
- Working with the useEffect hook

## What We Are Building

Here is a working version of the [OMBd Movie App](https://jmk0w.csb.app/) we will be building together. 

##  AJAX

AJAX stands for Asynchronous JavaScript an XML.

XML was once a popular way to store and send data over the internet and it is still used. However, JSON has become the predominant way to send data over the internet. But, no one seems to want to change the acronym AJAX to AJAJ.

When we will use AJAX, we will be sending and receiving JSON.

AJAX allows us to only reload portions of a web page. Thinking of an embedded google map, you can click around it and change the view/data, without having to reload the page every single time.

## AJAX/HTTP Requests

At first, jQuery was the only 'friendly' way to make AJAX requests. Over time, new libraries have developed.

Some top choices
- [$.ajax] (https://api.jquery.com/jquery.ajax/)
- [fetch] (https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)
- [axios] (https://www.npmjs.com/package/axios) 

## Third Party APIs

Many web sites have their own data, but they can pull in other data. For example, many news sites have a weather widget. This widget gets its data from a weather resource.

There are many APIs that can be used by individuals and companies. Some are totally free, some are available for a small fee, and some are really expensive.

There are APIs for
- Weather
- Stocks
- Beer
- Dictionaries
- Books
- Sports
- Art
- Games
- Movies

[Here is one list of APIs](https://github.com/toddmotto/public-apis)

## API

Many APIs are restricted. Maintaining data on a server can get expensive and the data on a lot of these sites is valuable.

The two main ways individuals/companies can get access to APIs is through API keys - a special set of characters that is purchased through the website. Every time you make a request, the key must be used, this lets the API keep track of how many requests you make and limit/charge you accordingly.

The other way is OAuth. OAuth is a tangent to what we'll talk about today, but if you want to learn more, here is a [good start](https://stackoverflow.com/questions/4201431/what-exactly-is-oauth-open-authorization).

Typically, we include additional parameters to send to the API using `query strings` which go at the end of a URL. API keys are one such query parameter however additional parameters can also sent using query parameters.  

The begining of a `query string` starts with a `?` and will have at least one or more key=value pair. 

Here are a few examples of API's using a `query string` to include additional parameters in the request:

**Random User**

```html
https://randomuser.me/api/?results=5
```

**Swapi (Star Wars)**

```html
https://swapi.dev/api/people/?search=r2
```

Notice how the above examples include a `/` before the `?` query string. That is not always the case and the format of the url is dependent on the API provider.  The API's below don't include a `/` before the `?`

**Hacker News**
```
https://hacker-news.firebaseio.com/v0/item/8863.json?print=pretty'
```

## API Keys

Some API's require keys in order to request data. The keys are added to url along with any additional query paramaters.

We can include any additional `key=value` pairs by using an `&` to seprate them. 

Here is an example of a request to OMDB (open movie data base), for a movie with the title `Eraserhead` and the `apikey`. The apikey may or may not work depending on if it is still active.  If not active then replace that key with the one provided to you by OMDB.

```
http://www.omdbapi.com/?t=Eraserhead&apikey=98e3fb1f
```

The order of the paramaters is not important and we could have written it as follows:

```
http://www.omdbapi.com/?apikey=98e3fb1f&t=Eraserhead
```

The name of query paramater for the api key may be different depending on the API.  The below example uses `apiKey` as the query param name.

**News API**

```
http://newsapi.org/v2/everything?q=bitcoin&apiKey=5d74ff271abd4bd3b2109d14d74c7d0b
```

While this API uses `api_key`

**Giphy API**

```
http://api.giphy.com/v1/gifs/search?q=ryan+gosling&api_key=YOUR_API_KEY&limit=5"
```

## EXERCISE

Perform the following:

- Sign up for API keys for the API's listed below
- Using your browser confirm that you are able to retrieve data from the API's

API's

- [http://newsapi.org](http://newsapi.org)
- [http://api.giphy.com](http://api.giphy.com)
- [Open Weather Map](https://home.openweathermap.org/users/sign_up)

## A Note On JSON

`JSON` stands for JavaScript Object Notation and is a subset of the object literal notation of JavaScript. It is primarliy used to transfer data between client and server when requesting or sending data using an API.  When sendinng data the API server first converts the JS into a string and then its the responsibility of the client to convert it back into the original JS data type. 

There are 2 rules that govern JSON.

- all keys/values must use double quotes
- trailing commas are forbidden (don't include a comma after the last `key:value` pair)

 Below is an example of the first and last 2 key value pairs of the JSON returned by OBMD.  Notice how all key:value pairs include double quotes.  Also make note that the last key:value pair does not have a trailing comma. 

```js
let movie = {
   "Title": "Eraserhead",
    "Year": "1977",
    "Website": "N/A",
    "Response": "True"
 }
```

There  are also online tools that can help you validate JSON and confirm that it follow the above rules such as: [jsonlint.com](https://jsonlint.com/)

JSON includes 2 methods that allow us to either convert an object to a string or parse it back into an object:

- JSON.stringify() - converts JS to a string
- JSON.parse() - converts a JS string back into its original format

The OMDB API first uses JSON.stringify() to convert the data into a string and then the JSON Formatter Chrome Extension converts it back into a object.

We can use these JSON methods in our own JS code when needed.  Let's test them out in Chromes Dev Tools and see what they return. 

```js
let stringifiedMovie = JSON.stringify(movie)
// this is now one long string of text
stringifiedMovie
```

```js
let parsedMovie = JSON.parse(stringifieMovie)
parsedMoovie
```

## Mini Movie App

We're going to build a tiny React single page app that has a text input, a button and when a user inputs a movie, they will get a set of movies that match from OMDB.


### React Architecture 

Our React architecture will look like the following:

```
App
 |
  - Form
  - MovieInfo
```

### Setup

1. Create a New React Project , cd into folder, run npm install
 - In your React folder run command ```npx create-react-app@latest react-movie-search```
 - cd into the newly created "react-movie-search" folder and `npm start`
 - go to localhost:3000 in your browser to see react project
2. Create a new Form Component that returns only a div with words `Form Componennt`
3. Import and render the Form Component in App
4. Create a new MovieInfo Component that renders only a div with words `Movie Componennt`
5. Import and render the MovieInfo Component in App

### Setting Up Our Form

We're going to be making requests to OMDB using `fetch`. We'll be viewing those results in Chrome's Console. Once we build out the functionality, we'll start incorporating our data into our web page.

Go to [request a FREE api key from OMDB](http://www.omdbapi.com/apikey.aspx)

Fill out the form, you should get an email asking you to validate your key within a few minutes. Hold on to this key, we'll be using it soon enough.

Let's build our first fetch request.

`fetch` is a function and it returns a JS Promise.  A `Promise` is used to allow developers to structure code based on how JS runs code all its code asynchronously.  

`fetch` will retrieve the data and `.then()` will work with returned data once the `Promise` is resolved.    

`fetch` requires 2 `.then()` methods.  The first will parse the data from a string back into its js data type and the second will then allow us to work with the actual data. 

**A URL String**
 
 ```
 fetch(someUrl)
  .then(res => res.json())
  .then(data => someFunction(data))
 ```
 
 **A URL String and an Object**
 - `method` - a string: `GET`, `POST`, `PUT`, `DELETE`
 - `url` - a string of where to send the request
 
 `fetch` can contain an additional arguement which must be an object. The object can contain additional keys related to the requesting the data. 
 
 ```
  fetch('someUrl', {
   method: 'GET',
  })
  .then(res => res.json())
  .then(data => someFunction(data))
 ```

The object requires a minimum of two key value pairs
- `method` - a string: `GET`, `POST`, `PUT`, `DELETE`
- `url` - a string of where to send the request

Since we are just sending GET requests, we won't need `data` for now - We'll build out our `fetch` request after we set up our form.



## App.js

Lets set our initial state and our `handleSubmit` method.

```js
 const [movieData, setMovieData] = useState({});
 
 const handleSubmit = title => {
  //...
 }
```

Lets pass down the handleSubmit function to `Form`

```
 <Form handleSubmit={handleSubmit} />
```

## Form.js

Now lets update our form to include the form and supporting functions.


```js
const [movieTitle, setMovieTitle] = useState('')

const handleSubmit = e => {
  console.log('handleSubmit clicked');
  e.preventDefault();
  props.handleSubmit(movieTitle)
  setMovieTitle('')
};

 const handleChange = e => {
   console.log('handleChange clicked');
   const title =  e.target.value
   setMovieTitle(title)
 };
   return (
     <React.Fragment>
       <form onSubmit={handleSubmit}>
         <label htmlFor='movieTitle'>Title</label>
         <input
           id='movieTitle'
           type='text'
           value={movieTitle}
           onChange={handleChange}
         />
         <input
           type='submit'
           value='Find Movie Info'
         />
       </form>
     </React.Fragment>
   )

```

```js
http://www.omdbapi.com/?apikey=9999999&t=Eraserhead
```

When you click on your anchor tag, you should be taken to the JSON view:

![OMDB response](https://i.imgur.com/ojU0Qrp.png)


Note: including your API key in the `app.js` and then pushing it up to github makes your API key findable. OMDB keys are not that valuable, so it shouldn't be a worry.

However, there are services that cost thousands of dollars a month. People write bots to look for exposed API keys to steal them. We don't have time to cover hiding API keys today. But keep it in mind for your projects.

### Using Fetch

Lets fetch our data


```js
const handleSubmit = title => {
  let movieUrl = `https://www.omdbapi.com/?t=${title}&apikey=98e3fb1f`;

  fetch(movieUrl)
    .then(res => res.json())
    .then(data => setMovieData(data));
}
```


Expected Appearance:
![success OMDB Eraserhead console response](https://i.imgur.com/ADtTqUz.png)

## Rendering our response in the Browser

Let's make a movie component that will render a view of our movie

```js
function MovieInfo(props){
    return  (
      <div>
        <h1>Title: {props.movie.Title}</h1>
        <h2>Year: {props.movie.Year}</h2>
        <img src={props.movie.Poster} alt={props.movie.Title}/>
        <h3>Genre: {props.movie.Genre}</h3>
        <h4>Plot: {props.movie.Plot}</h4>
      </div>
    )  
}

```

Finally, let's add some conditional logic that will either render `MovieInfo` and pass in our movie data as props or render nothing at all.

```js
  return (
    <div className="App">
      <div>Best Movie App Ever</div>
      <Form handleSubmit={handleSubmit} />
      {movieData.Title ? <MovieInfo movie={movieData} /> : null}
    </div>
  );
```

## Async / Await
The keyword `await` makes JavaScript wait until a line of code completely finishes executing. However, you can only use `await` inside of an `async` function. How do we turn a function into an `async` function? 

Easy! We just put the word `async` in front of the word `function`. With arrow functions, we just write `async` before the parenthesis, like this: `async () => { etc...}`. So, if we use `async` and `await` we can refactor our code and remove the `.then()` methods.

```js
const handleSubmit = async (title) => {
  let movieUrl = `https://www.omdbapi.com/?t=${title}&apikey=98e3fb1f`;
}
```

In order to force the code to run asynchronously we then add the keyword `await` before and statement that returns a `Promise`. In the example below both `fetch` and `res.json()` return `Promises` and must be preceded by the keyword `await`

```js
const handleSubmit = async (title) => {
  let movieUrl = `https://www.omdbapi.com/?t=${title}&apikey=98e3fb1f`;

  const res = await fetch(movieUrl)
  const json = await res.json()
  setMovieData(data)
}
```


## Resources:
- [David Walsh's Blog - The Fetch API](https://davidwalsh.name/fetch)
- [JS Promises](https://javascript.info/promise-basics)
- Add Link To All ES# Features