---
track: "React Fundamentals"
title: "MERN Stack Build Part 1"
week: 13
day: 3
type: "lecture"
---

# MERN Stack Build Part 1

In this full build we will:

Part 1
1. Build an Express API
1. Use Mongo/Mongoose with 1 model
1. Deploy the API with Heroku - (We'll keep it local)

Part 2 & 3
1. Build a Full CRUD Frontend with React
1. Deploy with Netlify

<br>
<br>
<br>

## Setup for Express Build


![Imgur](https://i.imgur.com/8tZRT1k.png)



1. Create a folder called `mern-people-app`
1. Inside this folder create another folder called `backend`
1. Generate a React app called frontend `npx create-react-app@latest frontend`

_Your folder structure should look like this..._

```shell
/mern-people-app
 -> /backend
 -> /frontend
```

4. `cd` into the `backend` folder

<br>
<br>
<br>

## Setting up the Express app

_Make files `touch .env server.js`_

1. Create a new node project `npm init -y`
1. Install dependencies `npm install dotenv mongoose express cors morgan`

<br>
<br>
<br>

6. Put the following in `.env` (make sure to use YOUR MongoDB connection url)

```shell
DATABASE_URL=mongodb+src://...
PORT=3001
```

<br>
<br>
<br>

## Starting Server.js

Let's build out the minimum to get `server.js` running:

```js

// DEPENDENCIES

// get .env variables
require("dotenv").config()
// pull PORT from .env, give default value of 3001
const { PORT = 3001 } = process.env
// import express
const express = require("express")
// create application object
const app = express()


// ROUTES

// create a test route
app.get("/", (req, res) => {
  res.send("hello world")
})


// LISTENER

app.listen(PORT, () => console.log(`listening on PORT ${PORT}`))
```

<br>
<br>
<br>

Run the server `npm start` and make sure you see `"Hello World"` when you go to `localhost:3001`.

<br>
<br>
<br>

## Adding a Database Connection

Let's update our `server.js` to include a database connection:

```js

// DEPENDENCIES

// import mongoose
const mongoose = require("mongoose")


// DATABASE CONNECTION

// Establish Connection
mongoose.connect(DATABASE_URL)
// Connection Events
mongoose.connection
  .on("open", () => console.log("You are connected to MongoDB"))
  .on("close", () => console.log("You are disconnected from MongoDB"))
  .on("error", (error) => console.log(error))


```

<br>
<br>
<br>

_Make sure you see the MongoDB Connection message when the server restarts_

<br>
<br>
<br>

## Adding the People Model

1. Let's add a People model to `server.js` 

2. Let's add `cors` and` express.json` middleware!

```js

// MODELS

const PeopleSchema = new mongoose.Schema({
  name: String,
  image: String,
  title: String,
})

const People = mongoose.model("People", PeopleSchema)


// MiddleWare

app.use(cors()) // to prevent cors errors, open access to all origins
app.use(morgan("dev")) // logging
app.use(express.json()) // parse json bodies

```

## Adding Routes


3. Lets add an index and create route to see and create our people.

```js

// ROUTES

// create a test route
app.get("/", (req, res) => {
  res.send("hello world")
})

// PEOPLE INDEX ROUTE
app.get("/people", (req, res) => {
 res.send("/people - Index Route")
})

// PEOPLE CREATE ROUTE
app.post("/people", (req, res) => {
  res.send("/people - Create Route")
})

```

Test them out before adding your database calls. 

### Getting Data - Async Await & Try Catch


**Async Await instead of .then()**

We know that making a request for data isn't instant and must be handled asyncronously. 
Javascript provides us with a tool called a Promise to handle this asyncronous process. 

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise):


The **Promise** object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

**A Promise is in one of these states:**

- pending: initial state, neither fulfilled nor rejected.
- fulfilled: meaning that the operation was completed successfully.
- rejected: meaning that the operation failed.

We have a few different options for "consuming promises" that are returned to us from our data base fucntions. In the past we used .then( ) and callback functions to do something after the data was sucessfully returned. Another more modern method is using **Async Await**.

When we use async await we should utilize a try catch to handle any errors that might come up. 

<br>


**Try/Catch**

From [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/try...catch):

The try statement always starts with a try block. Then, a catch block or a finally block must be present. It's also possible to have both catch and finally blocks. This gives us three forms for the try statement:

- try...catch
- try...finally
- try...catch...finally

<br>


```js

// ROUTES

// create a test route
app.get("/", (req, res) => {
  res.send("hello world")
})

// PEOPLE INDEX ROUTE
app.get("/people", async (req, res) => {
  try {
    // send all people
    res.json(await People.find({}))
  } catch (error) {
    //send error
    res.status(400).json(error)
  }
})

// PEOPLE CREATE ROUTE
app.post("/people", async (req, res) => {
  try {
    // send created person
    res.json(await People.create(req.body))
  } catch (error) {
    //send error
    res.status(400).json(error)
  }
})

```

<br>
<br>
<br>

3. Create 3 people using postman to make post requests to `/people`

4. Test the index route with a get request to `/people`

<br>
<br>
<br>

## Update and Delete

Let's add an Update and Delete API Route to `server.js`:

```js


// PEOPLE DELETE ROUTE
app.delete("/people/:id", async (req, res) => {
  try {
    // send deleted record
    res.json(await People.findByIdAndDelete(req.params.id))
  } catch (error) {
    //send error
    res.status(400).json(error)
  }
})

// PEOPLE UPDATE ROUTE
app.put("/people/:id", async (req, res) => {
  try {
    // send updated person
    res.json(
      await People.findByIdAndUpdate(req.params.id, req.body, { new: true })
    )
  } catch (error) {
    //send error
    res.status(400).json(error)
  }
})

```

<br>
We're Done! Test it in postman! 

<br>
<br>

## Deploy

1. Create a git repo in the `backend` folder `git init`

1. Add all files to staging `git add .`

1. Commit `git commit -m "message"`

1. Create a new repo on github.com (make sure its empty and public)

1. Add the remote to your local repo `git remote add origin URL` replace `URL` with your repos url

1. Push up your code `git push origin branchName` replace branch name with your active branch, find that with `git branch`

1. Go to heroku and create a new project

1. Under deploy connect your repo, enable auto deploys, and trigger a manual deploy

1. Under settings set your `DATABASE_URL` config var

1. In postman test all your API endpoints

<br>
<br>
<br>


## Lab Part 1 - Your Own Full Stack MERN Application

1. Create another project folder

2. Create the backend and frontend folders like you did for today's lesson

3. Create an API with index, create, update and delete routes

4. Create your model schema

5. Test the API with postman
