# AJAX and APIs

## Learning Objectives

- Explain what an API is and how to use one
- Describe AJAX and how it lets us build rich client-side applications
- Render new HTML content using data loaded from an AJAX request.
- Perform GET requests to an API to retrieve data.

## Framing (5 minutes, 0:05)

Over the past few weeks we've learned how to build our own websites, using a
variety of technologies. But the content on these sites has been limited to
whatever we type into the html directly. What if there were another way? (there
is)

Today, we're going to learn how to request information from third-party
databases, and how to make our applications more dynamic by making it so our
application doesn't need to refresh the page to make a request to an API server!

## Turn & Talk (10 minutes, 0:15)

> 10 minutes: 5 min T&T, 5 min review

[Open up Google Maps in your browser](https://www.google.com/maps). As you
interact with the web app, consider the following questions:

1. How many times do you see the page refresh?
1. When you request new data (i.e. by searching for a location), does the page
   refresh? Does the Google Maps app get data from a server?

## What is an API (15 minutes, 0:30)

**API** stands for "Application Programming Interface" and is a way of
describing software design. At the highest level, an API is any application with
a set of instructions for how programmers can interact with it. The DOM is
actually an example of an API.

When we're talking about the web (and web APIs), we're generally talking about
an application that we interact with through it's routes. However, requests to
an API's routes won't respond with views, they'll respond with data.

### API Data

An API will receive a scripted request and send a response. But what makes an
API different from a full-stack application, is that an API won't render views,
it'll just send back data. That data will generally be in one of two forms: XML
or JSON.

#### XML

**XML** stands for "eXtensible Markup Language" and is the granddaddy of
serialized data formats (itself based on HTML). XML is fat, ugly and cumbersome
to parse. It is increasingly the less common of the two formats you'll
encounter. It is still a major format due to its legacy usage across the web.
You'll probably always favor using a JSON API, if available.

XML looks like this:

```xml
<users>
  <user id="23">
    <name><![CDATA[Bob]]></name>
  </user>
  <user id="72">
    <name><![CDATA[Tim]]></name>
  </user>
</users>
```

#### JSON

**JSON** stands for "JavaScript Object Notation" and has become a universal
standard for sending and receiving data across the web. It is light-weight,
easy-to-read and quick to parse.

JSON looks like this:

```json
{
  "users": [{ "name": "Bob", "id": 23 }, { "name": "Tim", "id": 72 }],
  "content": "This is a piece of content"
}
```

#### What is Serialization?

All data sent via HTTP is sent as strings. Unfortunately, what we really want to
pass between web applications is **structured data** (i.e., arrays and hashes).
In order to do so, native data structures are **serialized**: converted from a
javascript object into a string representation of the data. This string can be
transmitted and then parsed back into data (de-serialized).

### Working with an API

APIs can be either public or private. If an API is public, anyone can access the
data behind it. If an API is private, then you'll need to get a password (called
an API Key) or go through some other form of authorization before you can access
data through that API.

Here is a good list of [Free Apis](https://github.com/toddmotto/public-apis)

We'll start by exploring a public API: [PokéApi](https://pokeapi.co/)

#### You do: PokéApi (10 minutes, 0:40)

> 10 minutes

For the next 10 minutes, explore the PokéApi. Try the links out in the browser
as well as on the pokeapi page.

> In order to format JSON in the browser nicely, you might need a plugin like
> Chrome's
> [JSON View](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc).

1. How do you get data for all Pokemon?
1. What about for a specific Pokemon?
1. How do you get data about a specific ability?
1. How do you get data about a specific location?
1. What type of request is being made when you open a url in the browser?

## What is AJAX (15 minutes, 0:55)

**AJAX** stands for "Asynchronous JavaScript and XML".

Back in the early- and mid-1990s, the only way for a user to request new data
was through the server-based request-response cycle. The user would click on a
link or change some data in the UI and the whole page would reload. This was
inefficient for the user and the User Experience; it was also an inefficient use
of bandwidth, as an entire rendered page had to be transmitted rather than just
the new or updated data.

This is where AJAX came in to play. AJAX is a way for us to make HTTP requests
in JavaScript. So we can make requests to our server (asynchronously) without
having to reload the page!

AJAX was first implemented in Internet Explorer as the `XMLHttpRequest` object
and later adopted by Mozilla and Safari. In 2005, Gmail and Google Maps were
rebuilt using `XMLHttpRequest` and a developer named Jesse James Garret wrote an
article titled
_"[Ajax: A New Approach to Web Applications](http://adaptivepath.org/ideas/ajax-new-approach-web-applications/)"_,
where he coined the term _AJAX_.

Building apps with `XMLHttpRequest` lead to a better user experience and faster
applications but it was extremely verbose and cumbersome to work with. To make
it easier, jQuery implemented the `.ajax()` api, abstracting away
`XMLHttpRequest` into a chainable set of function calls. We won't be using jQuery
in this course since it's an outdated library, but you can read more about `.ajax()`
if interested: [http://api.jquery.com/jquery.ajax/](http://api.jquery.com/jquery.ajax/)

More recently, WHATWG (the standards body for HTML) introduced the `fetch()` api
as a browser-native implementation of AJAX similar to the jQuery api. Fetch has
gained a lot of popularity in the past few years because its easy to use and
doesn't require loading in jQuery. Today we will focus on fetch for our api
calls.

Here's an example of XMLHttpRequest in action. Note how annoying this would be
to work with.

```js
function reqListener() {
  console.log(this.responseText);
}

var oReq = new XMLHttpRequest();
oReq.addEventListener("load", reqListener);
oReq.open("GET", "http://www.example.org/example.txt");
oReq.send();
```

### We Do: Lets see it in Action

We will use Javascript's `fetch()` method to make AJAX calls to an API. There
are several standard requests we can make, including GET, POST, PUT, PATCH and
DELETE. Today, we're only going to be making GET requests.

Let's setup a mini page to make requests from.

- Make a new folder called `js-pokeapi-ajax` in your sandbox.
- Create an `index.html` and a `script.js` file. Link your script in your html
  file.
- `npm install -g http-server` to install a package called `http-server`
  globally.
- Run `http-server` in the new `js-pokeapi-ajax` folder.
- Open your browser to `localhost:8080` and open the console

Add the following code to your script! Then open your index.html file in the
browser **using a live server**. Don't open it using the `open` command or the
requests will be blocked.

```js
// const url = "http://pokeapi.salestock.net/api/v2/pokemon/7"
const url = "https://pokeapi.co/api/v2/pokemon/7";
// use first url as a backup only

fetch(url)
  .then(res => {
    console.log("success!", res);
  })
  .catch(err => {
    console.log("something went wrong...", err);
  });
```

Every AJAX request needs a URL (where we're making our request to), the type (or
method) of our request and any headers that get sent with our request. Sometimes
the headers are used to tell the server we're requesting data from to give us a
certain format of data back (JSON or XML).

The default type or request fetch makes is a GET request. We can also squeak by
with the default headers (in this case, we're not passing any) for our GET
request because PokeAPI is not strict in what it asks from us.

Open your browser console and you should see the response getting logged!

Now check out the network tab and you'll see something like this:

![requests-1](./assets/network-tab-1.png)

This is a list of all the requests your browser has made since loading this
page. Click on any of them to see details.

The very first one is the html document itself. Loading that document will
trigger other requests, like a script or a css file or any images the document
might contain.

Now click on the one that says `7`, this is the AJAX request to pokeapi:

![requests-1](./assets/network-tab-2.png)

You can see the request and response headers. All of this data is sent with
every request!

### Aside: Promises & Asynchronous JS (10 minutes, 1:05)

An AJAX request is asynchronous: we'll make our request to the server and some
time later will get our response. In the meantime, the rest of our code will
keep running. We need some way to handle it when it's finished. We've previously
handled asynchronous actions by using callbacks. The `.fetch()` method uses
Promises.

Asynchronous also means that things won't run in the order that you expect them
to. Here's an example:

```js
console.log(1);

fetch(url).then(res => {
  if (res.ok) {
    console.log(2);
  }
});

console.log(3);
```

Try it! You might be surprised that you'll see 1, 3, 2 in the console.

You'll notice there are 2 functions _chained_ onto the AJAX call. These are our
**promise methods**. Promises are callbacks that may or may not happen. A
promise represents the _future result_ of an asynchronous operation. It's how we
handle the return value of our `fetch` request.

- `.then()` - this code is run if the Promise is fulfilled
- `.catch()` - this code is run if the Promise is rejected
- `.finally()` - this code is always run, fulfilled or not

Note that .then() will be called even if we get a 404 or some other error code
from our requested url. This is because the response back is still considered a
fulfilled promise. The response will however include a property `ok: false`
which you can use to control the flow of your code.

Ex:

```js
fetch(url).then(res => {
  if (res.ok) {
    console.log("celebrate!");
  }
});
```

### The API Response

But how do we access the JSON object we saw in the browser before? If we use
`.then()` we can log out information about the response of the page. However, we
can't do much with the body of the data yet. Luckily, our response has a
`.json()` method that can be called which will turn our response into json.

Let's change our `script.js` file to incorporate this.

```js
fetch(url)
  .then(res => {
    return res.json();
  })
  .then(res => {
    console.log("success!", res);
  })
  .catch(err => {
    console.log("something went wrong...", err);
  });
```

What we have above is an example of chaining. You are almost guaranteed to have
to work with chaining when dealing with AJAX or Promises. I like to short-hand
the above code to something like this...

```js
fetch(url)
  .then(res => res.json())
  .then(res => console.log("success!", res))
  .catch(err => console.log("something went wrong...", err));
```

We can then dig through this response just like any other JS object to pull up
the information we want.

```js
.then(res => console.log(res.name))
```

## You Do: DOM Manipulation Using Response Data (15 minutes, 1:20)

1. Add a form and a `<h1>` to your HTML. Your form should include a single text
   input and a submit button.
1. Add an event listener to your form so that when submitted, you trigger an
   AJAX call to the PokéAPI to find the Pokemon with the value from the text
   input field in the form. So if you type `52` in the input, search for the
   pokemon with the ID of `52`.
1. Inside your `.then()` promise callback, handle the response and set the text
   of your `<h1>` to be the **name** of the Pokemon the user searched for.

> Hint: What does `.preventDefault()` do? Bonus: include an `img` tag to your
> html and have a picture of the pokemon you search for appear on your page!

## Break (10 minutes, 1:30)

## We do: More requests / DOM Manipulation Practice

Using the API docs as reference, let's explore outputting a list of items.

We can use the base url of any of the resources, like `/berry/` or `/pokemon/`
to get a list of that item. The API only returns 20 at a time, which is fine for
our purposes.

Let's use that info plus our knowledge of DOM methods to make a list of berry
names.

- Get the list of berries from the API
- Loop through the list of berries, making an HTML element from each
- Append the HTML element to the DOM in an `li` tag

## You do: Add urls to each element

Using the previous logic, what would we need to change to add the `url` property
from the berries results to the DOM?


