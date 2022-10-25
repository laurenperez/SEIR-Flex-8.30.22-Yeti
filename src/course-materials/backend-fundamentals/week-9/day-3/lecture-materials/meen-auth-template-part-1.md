---
track: "Backend Fundamentals"
title: "MEEN Auth Template Build - Part 1"
week: 9
day: 1
type: "lecture"
---

# MEEN Auth Template Build - Part 1

As a Junior Developer, User Authentication is something you really shouldn't be working on just yet. But having some experince with it, and building projects with authentication looks really good on your resume and portfolio.

So, today we're going to build a template repo which will allow you to get up and running with Authenticatication without having to build it out every time you want to create a portfolio project with it.

And if you do want to code it out on your own, this template repo will provide great reference code as you do so!

<br>
<br>
<br>

Here's what we're going to create:

![example functionality](https://i.imgur.com/VenbpMj.gif)

<br>
<br>
<br>

## Set Up

Note: Your default branch on github might be set to 'main' if you want to make sure it's the same everywhere to avoid branching issues, navigate to [https://github.com/settings/repositories](https://github.com/settings/repositories) and change your default branch to `main`

- On [github.com](https://github.com) NOT GHE, create a new repo called `meen-auth-starter` with a node `.gitignore` \
  You may already have a global .gitignore configured, but it never hurts to have a local one, and if someone else wants to use your template, they'll be all set up with the proper files ignored.
- Clone that repo down to your computer
- `cd meen-auth-starter`
- `touch server.js`
- `npm init -y`
- `npm install express express-session bcrypt dotenv mongoose ejs method-override`
- `touch .env`

<br>
<br>
<br>

## Explain what a session is

Cookies are little strings of data that get stored on your computer so that, when you return to a web page, it will remember what you did the last time you were there. You can specify how long a cookie will stay around on a browser before it "expires" or is deleted. This can be a specific date, or it can end as soon as the user closes their browser.

<br>

The problem with cookies is that if you store sensitive information in them (usernames, etc), someone could take the computer and view this sensitive information just by opening up the web browser. Sessions are basically cookies, but the server stores the sensitive info in its own memory and passes an encrypted string to the browser, which gets stored in the cookie. The server then uses this encrypted string to know what was saved on the user's computer.

<br>

Sessions typically only last for as long as the user keeps their window open, and aren't assigned a specific date to expire. **BE CAREFUL: IF YOU RESTART YOUR SERVER, IT WILL LOSE ALL MEMORY OF THE SESSIONS IT CREATED, AND USERS' SESSIONS WILL NOT WORK**

## Set up Environmental Variables

We need a way to protect our sensitive information and a way to store environmental variables that are specific to our computer (in contrast to a co-workers computer or the environment in a cloud service).

<br>

Typically we'll have a `.gitignore` file to help with this. Sometimes this is a global file, sometimes we add is per-project. This file tells git which files to ignore when tracking our files. In there it states to never track `node_modules` nor `.env` - that way our values stay safely on our machines.

## Set up ENV file

In `.env`:

(This is an example, do not copy & paste)

```shell
PORT=3000
DATABASE_URL=mongodb+srv://<username>:<password>@general-assembly.1wjse.mongodb.net/meen-auth-starter?retryWrites=true&w=majority
SECRET=feedmeseymour
```

We'll be using this `SECRET` value soon. In general, it should be a completely random string. You do not want to copy this value from app to app or your stuff can get hacked. Feel free to jazz up your secret now if you like!

- Remember to use your own `DATABASE_URL`. Copying the one above will not work.
- Remember your `SECRET` should be a totally random string. This matters less in development, but it'll be important when you deploy your apps and have to add the variable to Heroku.

<br>
<br>
<br>

## Create a Basic Express Server

In `server.js`:

```js
// Dependencies
const express = require("express")
const app = express()
require("dotenv").config()

// Listener
const PORT = process.env.PORT
app.listen(PORT, () => console.log(`server is listening on port: ${PORT}`))
```

<br>
<br>
<br>

## Configure the Database

In `server.js`:

```js
// Dependencies
const mongoose = require("mongoose")

// Database Configuration
mongoose.connect(process.env.DATABASE_URL, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
})

// Database Connection Error / Success
const db = mongoose.connection
db.on("error", (err) => console.log(err.message + " is mongod not running?"))
db.on("connected", () => console.log("mongo connected"))
db.on("disconnected", () => console.log("mongo disconnected"))
```

<br>
<br>
<br>

**STOP! Check your work.**

Boot up your server. You should see:

```shell
server is listening on port: 3000
mongo connected
```

<br>
<br>
<br>

## Create User Stories

AAU = "As a User..."

- AAU I should be able to navigate to a registration page and create an account
- AAU I should be able to navigate to a login page and login to my account
- AAU I should be able to navigate to a protected dashboard page when logged in
- AAU I should be redirected to the login page if I try to access the dashboard when logged out
- AAU I should be able to log out of my account

<br>
<br>
<br>

## Create the User Model

- `mkdir models`
- `touch models/user.js`

In `models/user.js`:

```js
// Dependencies
const mongoose = require("mongoose")
const Schema = mongoose.Schema

// User Schema
const userSchema = Schema({
  email: { type: String, unique: true, required: true },
  password: { type: String, required: true },
})

// User Model
const User = mongoose.model("User", userSchema)

// Export User Model
module.exports = User
```

<br>
<br>
<br>

## Create Users Controller

- `mkdir controllers`
- `touch controllers/users.js`

<br>
<br>
<br>

## Configure Users Controller as Middleware

Let's just take care of this now so it doesn't go forgotten. \
While we're here, let's also configure our `body-parser` middleware, since we're about to begin working with `req.body`

<br>
<br>
<br>

In `server.js`:

```js
// Middleware
// Body parser middleware: give us access to req.body
app.use(express.urlencoded({ extended: true }))

// Routes / Controllers
const userController = require("./controllers/users")
app.use("/users", userController)
```

<br>
<br>
<br>

## Add Dependencies to User Controller and Export Router

While we're here, let's also add some comments to remind ourselves which routes we'll need in this file. Remember INDUCES (Index, new, delete, update, create, edit, show) to help organize your routes and prevent conflicts.

<br>
<br>
<br>

In `controllers/users.js`:

```js
// Dependencies
const express = require("express")
const userRouter = express.Router()
const User = require("../models/user.js")

// New (registration page)

// Create (registration route)

// Export User Router
module.exports = userRouter
```

<br>
<br>
<br>

## Create Registration Route (Create / POST)

This is where the user is first created and their username and password are saved for the first time. 



### Explain what bcrypt does

bcrypt is a package that will encrypt passwords so that if your database gets hacked, people's passwords won't be exposed.


### Include bcrypt package

In `server.js`:

```js
// Dependencies
const bcrypt = require("bcrypt")
```

<br>
<br>
<br>

Here's the code for hashing a string:

```js
const hashedString = bcrypt.hashSync("yourPasswordStringHere", bcrypt.genSaltSync(10))
```

<br>
<br>
<br>

Let's add it to a route to see how it works:


In `controllers/users.js`:

```js
userRouter.post("/", (req, res) => {
  //overwrite the user password with the hashed password, then pass that in to our database
  req.body.password = bcrypt.hashSync(req.body.password, bcrypt.genSaltSync(10))
  res.send(req.body)
})
```

<br>

**STOP! Check your work with postman**

<br>

Add an email and password (don't use any of your real passwords incase you need to share your screen) \
Has your password been hashed? Yes? Awesome! No? Take a moment to debug.

<br>
<br>
<br>

## Update Registration route to Create a User in the Database

In `controllers/users.js`:

```js
// Create (registration route)
userRouter.post("/", (req, res) => {
  //overwrite the user password with the hashed password, then pass that in to our database
  req.body.password = bcrypt.hashSync(req.body.password, bcrypt.genSaltSync(10))

  User.create(req.body, (error, createdUser) => {
    res.send(createdUser)
  })
})
```

**STOP! Check your work with Postman.**
This time, you should get back a MongoDB Entry

<br>
<br>
<br>

## Touch Up The Registration Route to Redirect to the Index Page

In `controllers/users.js`:

```js
// Create (registration route)
userRouter.post("/", (req, res) => {
  //overwrite the user password with the hashed password, then pass that in to our database
  req.body.password = bcrypt.hashSync(req.body.password, bcrypt.genSaltSync(10))

  User.create(req.body, (error, createdUser) => {
    res.redirect("/")
  })
})
```

<br>
<br>
<br>

## Configure Express Sessions

In `server.js`:

```js
// Dependencies
const session = require("express-session")

// Middleware
app.use(
  session({
    secret: process.env.SECRET,
    resave: false,
    saveUninitialized: false,
  })
)
```

We've already added the `SECRET` variable to our `.env` file, so we should be good to go!

<br>
<br>
<br>

## Create Sessions Controller

Login (session creation)/ Logout (session destruction) functionaliy will be handled with express-session, so we'll have a separate controller for that.

- `touch controllers/sessions.js`

<br>
<br>
<br>

## Configure Sessions Controller as Middleware

Let's just take care of this now so it doesn't go forgotten.

In `server.js`:

```js
// Routes / Controllers
const sessionsController = require("./controllers/sessions")
app.use("/sessions", sessionsController)
```

<br>
<br>
<br>

## Require Dependencies in Sessions Controller

While we're here, let's also add some comments to remind ourselves which routes we'll need in this file. Remember INDUCES!

In `controllers/sessions.js`:

```js
// Dependencies
const express = require("express")
const bcrypt = require("bcrypt")
const sessionsRouter = express.Router()
const User = require("../models/user.js")

// New (login page)

// Delete (logout route)

// Create (login route)


// Export Sessions Router
module.exports = sessionsRouter
```

<br>
<br>
<br>

## Login Functionality - Using Bycrypt compare 

Here's the code to compare a string:

```js
bcrypt.compareSync("yourGuessHere", hashedStringFromDatabase )
```

compareSync evaluates to true or false.

<br>
<br>
<br>

### Compare a string to a hashed value to see if they are the same

Because the same string gets encrypted differently every time, we have no way of actually seeing what the value of the string is. We can compare it to another string and see if the two are "mathematically" equivalent.

Take a moment to think about how bcrypt can help us protect users passwords (we should never store an un-hashed password in our database) and how it can help us check to make sure the password a user is trying to log in with matches the hashed password we have stored in the database.


## Create Login Route (Create / POST)

Before we start coding, let's think this through a bit.

When a user tries to login, we need to check a few things.

1. First we want to check if the user exists in our database
   - If the user doesn't exist, return an error (they havent signed up yet)
   - If the user exists...
1. Compare the password they provided with the hashed password we have stored in the database for them
   - If the passwords don't match, return an error and ask the user to try again
   - If the passwords do match...
1. Create a new express session for the user (log them in)

Alright, let's baby step this!

<br>
<br>
<br>

In `controllers/sessions.js`:

```js
// Create (login route)
sessionsRouter.post("/", (req, res) => {
  // Check for an existing user
  User.findOne(
    {
      email: req.body.email,
    },
    (error, foundUser) => {
      // send error message if no user is found
      if (!foundUser) {
        res.send(`Oops! No user with that email address has been registered.`)
      } else {
        // If a user has been found
        // compare the given password with the hashed password we have stored
        // this will return a true or false 
        const passwordMatches = bcrypt.compareSync(
          req.body.password,
          foundUser.password
        )

        // if the passwords match
        if (passwordMatches) {
          // add the user to our session
          req.session.currentUser = foundUser

          // redirect back to our home page
          res.redirect("/")
        } else {
          // if the passwords don't match
          res.send("Oops! Invalid credentials.")
        }
      }
    }
  )
})
```

<br>
<br>
<br>

**STOP! We have a lot of work to check.**

<br>

Let's do so with Postman:

1. Create a `POST` request to `http://localhost:3000/sessions`
1. Use an INCORRECT email address and an INCORRECT password to login \
   You should see `Oops! No user with that email address has been registered.`
1. Now use the CORRECT email address and an INCORRECT password \
   You should see `Oops! Invalid credentials.`
1. Now use the CORRECT email address and the CORRECT password \

<br>
<br>
<br>

This should redirect you to the index page. We don't yet have anything at that index route, so you should see:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Error</title>
  </head>

  <body>
    <pre>Cannot GET /</pre>
  </body>
</html>
```

<br>
<br>

Perfect! Now we can register and login a user.

## Create a Logout Route (Destroy / Delete)

In `controllers/sessions.js`:

```js
// Delete (logout route)
sessionsRouter.delete("/", (req, res) => {
  req.session.destroy((error) => {
    res.redirect("/")
  })
})
```

Delete routes are often pretty easy. Here we're deleting the session and redirecting to the index page.

**STOP! Check your work in Postman.**

Create a `DELETE` request to `http://localhost:3000/sessions`
Once again, this should redirect you to the index page. We don't yet have anything at that index route, so you should see:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <title>Error</title>
  </head>

  <body>
    <pre>Cannot GET /</pre>
  </body>
</html>
```

<br>
<br>
<br>

Awesome! Now that our core functionality is built out, all we need is:

- Index View
- Navigation Partial
- Register View
- Login View
- Protected Dashboard View


## References

- [Bcrypt in a little more depth - Thanks Eric Lewis!](https://www.dailycred.com/article/bcrypt-calculator)

- [Express Session Middleware](https://www.npmjs.com/package/express-session)
- [Express.js Docs on Session Middleware](https://expressjs.com/en/resources/middleware/session.html)
- [Bcrypt](https://www.npmjs.com/package/bcrypt)