---
track: "React Fundamentals"
title: "Token-based Auth with React & JWTs"
week: 14
day: 3
type: "lecture"
---


# Token-based Auth with React & JWTs

<br>
<br>
<br>

<!-- ### [DanielJS Recording](https://generalassembly.zoom.us/rec/share/4k1vEm8AOOaZn3dJRv1IQpkpXjZwzhJyDW1bjAni9y6iFawzk7fMvNbOUZqKrnC9.3MK_5mcpPqzachfC?startTime=1608395918000) -->


## Learning Objectives

| Students Will Be Able To: |
| --- |
| Explain the use case of token authentication |
| Encode/decode a JSON Web Token (JWT) |
| Configure an Express app to provide JWTs |
| Persist a JWT on the client |
| Send a JWT with each request |
| Verify a JWT on the server |
| Protect "private" client-side routes |
| Protect "private" server routes with middleware |

<br>
<br>
<br>



## Roadmap

- Review of session-based authentication
- What's a JSON Web Token (JWT)?
- Flow of token-based authentication
- Advantages of JWT-based authentication
- Set up the code
- Review the code

<br>
<br>


#### Steps to Implement Token-based Authentication & Authorization:

1. Refactor the server to hash the password when a user signs up. 
2. Refactor the server to provide a JWT when a user signs up.
3. Persist the token (JWT) on the client.
4. Update the `<App>` component's state to hold the authenticated user's info.
5. Refactor the `<Header>`'s display based on auth status.
6. Implement Log Out functionality.
7. Update the `user` in `<App>`'s state when signing up.
8. Implement Log In functionality.
9. Provide the token when making AJAX requests.
10. Verify JWTs sent by the client and add the `user` to the Express `request` object.
11. Implement authorization: Protect the `/people` client-side route.
12. Implement authorization: Protect server-side routes with custom middleware.


<br>
<br>
<br>


## Token-based Authentication

<br>

### Review of Session-based Authentication

Before we talk about token-based authentication, let's review one of the types of auth that you've already used, session-based authentication.

<img src="https://i.imgur.com/TZoeAVv.png" width="900">


<br>
<br>
<br>




### What's a JSON Web Token (JWT)?

A _JSON Web Token_ is a single encoded (not encrypted) string that plays the role of a "token".

The key points about a JWT are:

- The token can contain whatever custom data (called _claims_) we want to put in it.
- The token is cryptographically _signed_ by the server when it is created so that if the token is changed in any way, it is considered invalid.
- The token is _encoded_, but **not encrypted**.  It is encoded using a standard known as _base64url encoding_ so that it can be easily serialized across the internet or even be included in a URL's _querystring_. It's easy to look at **encoded** data and think that its content cannot be read - this is not the case, as you'll soon see.

<br>
<br>


Here is how a JWT is structured:

<img src="https://i.imgur.com/8J6Rhx9.jpg">

There is a great website dedicated to JWTs that explains in detail their format as well as has the ability to create them:  [https://jwt.io/](https://jwt.io/)

Allow me to take a JWT from the website and demonstrate the fact that the token can be easily decoded in the browser's console:

```javascript
> let jwt = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ';
> let payload = jwt.split('.')[1]  // only interested in the payload (claims)
> window.atob(payload)
< "{"sub":"1234567890","name":"John Doe","admin":true}"
```
> The `atob()` method decodes a base-64 encoded string and `btoa()` base-64 encodes data.

Okay, JWTs are cool, how does the client get one; and how do we use them?


<br>
<br>
<br>




### Flow of Token-based Authentication

<img src="https://i.imgur.com/3quZxs4.png">

The diagram above shows that the client app:

1. Attempts to log in a user by sending an HTTP POST request, sending along the user's credentials.
2. The server will, if the creds check out, generate a JWT and send it back to the client. It may be sent back as JSON, or in a header (usually named **Token**).
3. Not shown on the diagram, but important, is the fact that the token needs to be persisted somewhere on the client. In a web app, the token is typically persisted in `localStorage`.
4. The reason a client needs to persist a token is that now, whenever the client makes a request, it can send along the token in the HTTP request, either as a querystring, in the request's body, or, as a best practice, in a header named `Authorization`.
5. The server will then validate the token and respond to the request.

What are some advantages of token-based vs. session-based auth...

<br>
<br>
<br>


### Advantages of JWT-based Authentication

Here's a graphic contrasting sessions and tokens:

<img src="https://i.imgur.com/HlzMMRq.jpg" width="900">

Sessions are stateful on the server - they have to be maintained in a server's memory or a database.  The more active users there are, the more sessions there are to keep track of. High-volume websites require multiple servers and would therefore require special software to manage the sessions.

The key to token-based authentication is that it's **stateless**, meaning there is no _state_ being stored on the server regarding the session/login.

A JSON web token is self-contained, it can itself contain the user's identity, etc. There's no need to fetch the user from a database with each request on the server (an expensive operation). You will only have to query the database for the user if you need to modify the user or obtain additional information from the user document that is not included in the JWT.

The stateless nature of token-based auth allows the implementation of single sign-on (SSO) - where the same token can be used to access several different applications, for example, Google Mail, Google Docs, etc.

When making an HTTP request, a token can be sent in an HTTP header (or even the HTTP body). They don't have to be sent in a cookie, which are implemented by web browsers. Thus, you can use token-based authentication without a web browser - great news for _native mobile apps_.

<br>
<br>


## Set Up
JWT Auth codealongs can be very lengthy, but we want the main focus to be on the authentication layer we are adding to our server and react app. Therefore, some **enhanced** starter code has been provided. 

<br>

We'll be using a version of the *people app*: 
- Frontend: [**click here for startercode**](https://git.generalassemb.ly/laurenperez-ga/people-frontend).
- Backend: [**click here for startercode**](https://git.generalassemb.ly/laurenperez-ga/people-backend).
- Install the `node_modules` in both: `$ npm i`
- Inside `server.js`, an environment variable is referenced for connecting to a cloud-hosted MongoDB; we need to create the `.env` file, and then add the variable that references our own MongoDB Connection URI

<br>
<br>
<br>



#### Review the Code

Here are a few of the highlights of the starter code:

**SERVER CODE**

- The `dotenv` module has been installed and required in **server.js**.
- Make sure to create a `.env` and add your environment variable for **mongoDB connection string** here.

- We've already added a simple `User` model defined in **models/user.js** in the startercode.

- We have API Routes for `User`  defined in **server.js** (just one for now).

- There is a **controllers/users.js** module that at this point, only has a `signup` action for creating users when they sign up. 

	Currently, the method returns the JSON of the created user, however, we will soon refactor this to return a JWT.

- There is a `People` model and a  **controllers/people.js** module with full **CRUD** actions.

-	Later in the lesson, we will make these "protected" routes that require a user to be logged in.
	
<br>
<br>


**CLIENT CODE**

- Client-side routes and components have been defined for:
	- `/signup`: Shows the `<SignupPage>` component.
	- `/login`: Shows a `<LoginPage>` component.

- The `<Header>` component has been altered so that it currently has `<Link>`'s to the `/signup` and `/login` routes. It's only rendered in `App`.


- `<SignupPage>` displays a sign up form that is working, we can use this as a slightly more advanced solution/reference for creating forms!

	Submitting the form adds a user to the database, via the `signup` function from the `signupService` in the **services** folder.
	
	Note how the `Sign Up` button is disabled using a custom `validForm()` helper function.
	
- After a user signs up, we want to redirect to the root route.


- Upon mounting, `<Main>` makes an AJAX call with `useEffect` to fetch all people.

- Although people data is going to be returned by the server almost instantly, we're still initializing `people` to be an empty array - **always do this**.

- Currently, it has a working `signup` function.

<br>
<br>
<br>


## Implement Token-based Authentication & Authorization

Implementing token-based auth will require plenty of code in both the Express server app and the React client app.

There's lots to do, so let's get going!


<br>



### Step 1: Refactor the server to hash the password when a user signs up  ( 15 min )

Currently, when a user signs up, the password is being stored in the database as cleartext (plain text) - not good!

Let's fix this security flaw by refactoring the server to salt and hash the users' password.

Open up **models/user.js**.

We're already using Mongoose's `set` method on the schema to ensure that a user's password is not included when serialized to JSON (sent to the browser).

Now we will take advantage of Mongoose middleware to salt and hash the password whenever a user instance is being saved **and** the password has changed (including when a user is being created for the first time).

To perform the actual salting and hashing, we will use the ever so popular **bcrypt** library - let's install and save it as a dependency:

`$ npm install bcrypt`

First, bring in **bcrypt**:

```javascript
// models/user.js

const mongoose = require('mongoose');
const bcrypt = require('bcrypt');
```

**bcrypt** has a setting that tells it how many times to randomize the generation of salt. Let's add a constant in the module to set it - usually 6 is enough:

```javascript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;
// require bcrypt 👇
const bcrypt = require('bcrypt');

// set up a salt rounds constant value
const SALT_ROUNDS = 6;
```

Now for the middleware. We'll write a function that runs before a user is saved. This is called **pre** middleware, also known as a mongoose "hook".

Type in this skeleton just above the `module.exports`:

```javascript
userSchema.pre('save', function(next) {
  // this will be set to the current document
  const user = this;

});
```

Note that we are assigning `this` (the user document being saved) to a variable. The reason is that we will need to access this user doc from within the `bcrypt.hash()` method's callback (see code below). 

Now let's add the code that checks if the password for this user document has been changed, and if so, salt & hash it, then assign the hash to password, replacing the cleartext version:

```javascript
userSchema.pre('save', function(next) {
  const user = this;
  if (!user.isModified('password')) return next();
  // password has been changed - salt and hash it
  bcrypt.hash(user.password, SALT_ROUNDS, function(err, hash) {
    if (err) return next(err);
    // replace the user provided password with the hash
    user.password = hash;
    next();
  });
});
```

Let's check our code by signing up a new user and using the MongoDB Atlas Interface to check that the password has been hashed in the new user document.

The user's password should be hashed!

**Done with Step 1, on to Step 2...**


<br>
<br>


### Step 2: Refactor the server to provide a JWT when a user signs up. ( 15 min )

The starter code was set up to temporarily return the new user document when a user signs up, however, we need to return a JWT instead - thus, auto-logging in when a user signs up.

First, we're going to need to install the Node module that can create and verify JWTs.

[https://jwt.io](https://jwt.io) lists libraries available for your programming language of choice.

Let's install the one for Node apps:

`$ npm install jsonwebtoken`


Creating a JWT requires a "secret" string used for "signing" the JWT. Let's define one in our **.env** file:

```shell
DATABASE_URL=mongodb://someusername:somepassword@ds064799.mlab.com:64799/mastermind
SECRET=SEIRocks!
```

🚨 **NOTE:** You will need to restart express for the new `SECRET` env var to be added to server memory

<br>
<br>
<br>

With **jsonwebtoken** installed, **controllers/users.js** is where we're going to use it:

```javascript
// controllers/users.js

const User = require('../models/user');
const jwt = require('jsonwebtoken');
```


Let's create a shortcut variable in our controller to hold the SECRET:

```javascript
const User = require('../models/user');
const jwt = require('jsonwebtoken');
// Add the SECRET
const SECRET = process.env.SECRET;
```

The **jsonwebtoken** library has a `sign` method that creates JWTs. Let's add a `createJWT` helper function at the bottom of **controllers/users.js** that we can use both when a user signs up and when they log in:

```javascript
/*----- Helper Functions -----*/

function createJWT(user) {
  return jwt.sign(
    {user}, // data payload
    SECRET,
    {expiresIn: '24h'}
  );
}
```

> Note: There are several ways to specify the expiration of the JWT. Check [the docs](https://www.npmjs.com/package/jsonwebtoken) for more info.

Now let's refactor the `signup` action to return a JWT:

```javascript
const signup = async (req, res) => {
  try {
    let user = await User.create(req.body);
    const token = createJWT(user);
    // Send back a JWT
    res.json({ token });
  } catch (error) {
    // Probably a duplicate email
    res.status(400).json(error);
  }
};
```

The `signup` action is transporting the token string to the client within an object (assigned to a key named `token`). Keep this in mind because we'll need to refactor **signup.js** on the client to extract only the token string.

Open up the Network tab in Chrome's DevTools, clear the requests, and then sign up another user to verify that a token is being returned.

Moving on to Step 3...

<br>
<br>


### Step 3: Persist the token (JWT) in the client ( 15 min )

As discussed, token-based authentication requires the client to send the token when making a request to a server's API. To pull this off, we're going to have to persist it somewhere in the browser...

`localStorage` is typically where web apps persist data in the browser.

> Note: Data saved in `localStorage` is persisted by domain until removed. If you want to save data for only the duration of the browser session, use `sessionStorage` instead.

Keeping the token string stored in `localStorage` allows users to remain logged in until the token expires. We will be logged in, even if we close the browser and come back tomorrow! However, you get to determine how long the token is good for when you generate it on the server.

We'll keep all token related code in it's own utility module, but first, let's do that quick refactor to **signup.js** I mentioned a bit ago...

<br>
<br>


#### Refactor the `signup` function in **signup.js**

Again, we only want to store the token **string** in `localStorage`, however, the token string is received by the client within a response object.

Here's a small refactor to the `signup` function:

```javascript
function signup(newUser) {
  return fetch(`${CONFIG.DEV.URL}/users/signup`, {
    method: "POST",
    headers: new Headers({ "Content-Type": "application/json" }),
    body: JSON.stringify(newUser),
  })
    .then((res) => {
      if (res.ok) return res.json();
      // Probably a duplicate email
      throw new Error("Email already taken!");
    })
    .then(({ token }) => {
      // TODO: set the token in local storage
    });
}
```

This funky syntax, `{token}` is object parameter destructuring! Only array destructuring was part of ES2015, however most browsers can now destructure objects as well.

<br>
<br>


#### Creating the `tokenService` utility module

Just like with the `signup` module, we're going to follow the single-responsibility principle by putting token related functions in a module for:

- Storing, retrieving and removing tokens from `localStorage`
- Verifying that a token has not expired and removing it from storage if it has.
- Extracting the data payload (the user's info).

Let's create a file for our token service:

`$ touch src/services/tokenService.js`

Just add a `setToken` function for now:

```javascript
function setToken(token) {
    localStorage.setItem("token", token)
}

export {
  setToken
};
```

We'll add other functions in a bit, but for now, this is all we need to persist the token...

<br>
<br>


#### Persisting the token to `localStorage`

Now let's refactor the `signup` function in **signup.js** to use the `setToken` function we just created.

First, we need to import **tokenService.js**:

```javascript
// services/signup.js

import { setToken } from './tokenService';
```

Then the refactor of `signup`:

```javascript
async function signup(user) {
	...

  setToken(token);
}

```

Now sign up another user and go to **`localStorage`** within the **Application** tab of DevTools. Verify that the token is in `localStorage` stored as a string.

> For fun, decode the payload portion of the token string.

**Nice, Step 3 is done!**

<br>
<br>


### Step 4: Update the `<App>` component's state to hold the authenticated user's info ( 15 min )

We will want to keep a `user` object in the `<App>` component's `state` so that it can be passed via props to components that need to be aware of the logged in user, such as `<Header>`.

If there is no user logged in, we will set the `user` property on the `state` object to `null`. We need a function to get the user data from the token. 

<br>
<br>


#### Add a `getUserFromToken` function to the `tokenService`

Anytime the app is loaded or refreshed, we're going to want to check to see if there's a valid token in `localStorage` and "log in" that user automatically. We need two functions. One to get to token from local storage and one to extract the user data from it. Lets write them now!

<br>
<br>

in `tokenService.js`:

**Now we can define `getToken` and  `getUserFromToken` function below `setToken`

```javascript

function setToken(token) {
    localStorage.setItem("token", token)
}

function getToken() {
  let token = localStorage.getItem("token");
  return token
}

function getUserFromToken() {
  const token = getToken();
  // if there is a token, pull the user data out of it, if not set user to null
  const user = token ? JSON.parse(atob(token.split(".")[1])).user : null;
  return user
}


export {
  setToken,
  getToken,
  getUserFromToken,
}
```

Pause! Before we return the token we found in local storage, we should check and make sure that it has not expired. If the token is expired then we should delete it and force the user to log in again. 

To keep our code clean, we will delegate dealing with verifying token expiration to an additional separate function.

Let's create an `isTokenExpired` function.


In **tokenService.js**:

```javascript
...

function isTokenExpired(token) {
  // we get the expiration from the payload
  const payload = JSON.parse(atob(token.split(".")[1]));
  // JWT's expiration is expressed in seconds, not milliseconds, so convert
  return payload.exp < ( Date.now() / 1000 ) // <- this will return true or false
}

export {
  setToken,
  getToken,
  getUserFromToken,
  isTokenExpired,
}
```


> Note: We needed to divide Date.now() by 1000. This is because the JWT spec says the `exp` claim should be in Unix time - Unix Time is the number of seconds since the Unix epoch (Jan 1, 1970). However, JS returns the number of milliseconds (not seconds) since the Unix epoch. We therefore must divide by 1000 to convert milliseconds to seconds.

<br>

Now lets refactor `getToken` to always check if the token has expired. 
If the token has expired we'll remove it from local storage and return null.

```js

function getToken() {
  let token = localStorage.getItem("token");
  if (token) {
    // Check if expired, remove if it is
    let expired = isTokenExpired(token)
    if (expired) {
      localStorage.removeItem("token");
      token = null;
    }
  }
  return token;
}

```

Be sure to update the `export` as shown above as well.

<br>
<br>


#### Add `user` to `<App>`'s state

Time to add a `user` property to `<App>`'s state.

First, import `tokenService` in **App.js**:

```javascript
import { getUserFromToken } from './services/tokenService';
```

Since adding a `user` to state from a token in localStorage is not an asynchronous process, we'll set up another `useState` hook to store the logged-in user:

```javascript
  // Initialize user if there's a token, otherwise null
const [userState, setUserState] = useState({user: getUserFromToken()});
```

The last user we signed up should now be in the state of `<App>`. Use the React DevTools to check it out!

<br>
<br>


### Step 5: Refactor the `<Header>`'s display based on auth status ( 8 min )

Just like in the other two authentication lessons, we want the navigation links to render according to whether there is a user logged in or not:

- **Logged in:** Display a greeting and a **Log Out** link.
- **Not logged in:** Display **Log In** and **Sign Up** links like we are currently doing.

Now that we've added a `user` to `<App>`'s `state`, we need to pass it on down to the `<Header>` component as a prop.

**I bet you can do it in 5 minutes or less!**

Now that `<Header>` has a `user` prop, let's refactor **Header.js**.

We want to display one of two choices -

```javascript
import { Link } from "react-router-dom";

function Header({ user, handleLogout }) {
  
  return !user ? (
    <nav className="nav">
      <Link to="/">Home</Link>
      <Link to="/signup">Sign Up</Link>
      <Link to="/login">Log In</Link>
    </nav>
  ) : (
    <nav className="nav">
      <h1>Welcome {user.name}, </h1>
      <Link to="/">Home</Link>
      {/* TODO: Add a logout link */}
    </nav>
  );
}

export default Header;
```

Awesome!

<br>
<br>


### Step 6: Implement Log Out functionality ( 8 min ) 

Lets add a `<Link to='' className='Header-link'>LOG OUT</Link>` for logging out.

When the **LOG OUT** link is clicked, we don't want to change routes, instead we want to:

1. Remove the token from `localStorage`
2. Set `user` state to `null`

First let's add an `onClick` prop to the link:

```javascript
<Link to='' className='Header-link' onClick={handleLogout}>LOG OUT</Link>
```

**Write the below helper function in App.js**

```javascript
function handleLogout (){
  logout(); // 👈  we'll define this inside of signupService next
  setUserState({ user: null });
}
```

**Don't forget to add `logout` to your named import**

```javascript
import { logout } from './services/signupService';
```

**As usual, pass that function down to where it's needed (Header.js) - you got this.**

Now let's add the `logout` function to **signup.js**:

```javascript
function logout() {
  removeToken(); // 👈  we'll define this removeToken function inside tokenService.js next
}

export {
  signup,
  logout
}
```

<br>
<br>
<br>

**As you probably noticed, we're working in reverse order! 🤪 ... Welcome to software development! 🎉**

We're using a `removeToken` function inside `logout`. **NOTE:** we still haven't defined this function inside our token service, but let's import that at the top of `signup`:

```javascript

// update your named import statement
import { setToken, removeToken } from './tokenService';
```

Next, we need that `removeToken` function added to **tokenService.js**:

```javascript
function removeToken() {
  localStorage.removeItem('token');
}

export {
  setToken,
  getToken,
  getUserFromToken,
  isTokenExpired,
  removeToken
}
```

Test it out and verify that the LOG OUT link is working - sweet!

Sign up again and yikes, the nav bar didn't update!

Let's fix this problem in the next step...

<br>
<br>



## Step 7: Updating the `user` in `<App>`'s State When Signing Up  ( 8 min )

**Why didn't the display update?**

Let's take care of this by first adding a `handleSignup` helper in **App.js**:

```javascript
    function handleSignupAndLogin() {
      setUserState({user: getUserFromToken()});
    }
```
We need to pass it from `<App>` down to `<SignupPage>` via props.

Here's the refactor that adds the call to `<App>`'s `handleSignupAndLogin` in **SignupPage.js**:

```javascript
  async function handleSubmit (e) {
    e.preventDefault();
    try {
      await signup(formState);
      // Let <App> know a user has signed up!
      handleSignupAndLogin();
```

That should do the trick! Feel free to sign up and log out all you want!

What's that? You're tired of signing up different users?

On to the next step!

<br>
<br>


## Step 8: Implement Log In functionality ( 20 min )


To implement logging in, we need to write code on both the client and server...

<br>


### Implement logging in on the client

We already have a `<LoginPage>` component.


We're using controlled `<input>`s here, however, the `handleChange` function in the `onChange` is not yet implemented - here's the finished product:

```javascript
 function handleChange(e) {
    setFormState((prevState) => ({
      ...prevState,
      [e.target.name]: e.target.value
    }));
  }
```

The above code is awesome because this single function can handled updating the state for any number of `<input>`s! This is more elegant than writing dedicated functions for each `<input>`.

Since logging in is almost the same as signing up, let's **copy** the `handleSubmit` function from `<SignupPage>` and **replace** the one that's currently in `<LoginPage>`.

Now a few of tweaks:

1. Since we'll need to define a `login` function in `signup`, let's import it ... again, this doesn't exist yet:
	
```javascript
	import { login } from '../../services/signupService';
```

2. Let's update the code to invoke a `login` function (which we will write next) and also tweak the error handling to something like this:


```javascript

async function handleSubmit (e) {
  e.preventDefault();
  try {
  
    await login(formState); // this will make the api call to the server to verify credentials
    // TODO: tell App.js that the user is signed in and set the user state for the application
    // TODO: navigate to the homepage once user is logged in

  } catch (err) {
    // Use a modal or toast in your apps instead of alert
    alert('Invalid Credentials!');
  }
}
```

3. We're now going to use the same function to notify `<App>` when someone logs in. This is how we lift state to set the logged in user in App.js:


```javascript

function LoginPage({ handleSignupAndLogin, updateMessage }) {

...

	async function handleSubmit(e) {
    e.preventDefault();
    try {
      await login(formState);
      // lift state to app
      handleSignupAndLogin();
      // TODO: navigate to the homepage once user is logged in
    } catch (err) {
	    alert('Invalid Credentials!');
    }
  }
```

<br>

Awesome, the next step in implementing log in functionality is to add the `login` function to **signup.js**:


```javascript
function login(credentials) {
  return fetch(`${CONFIG.DEV.URL}/users/login`, {
    method: "POST",
    headers: new Headers({ "Content-Type": "application/json" }),
    body: JSON.stringify(credentials),
  })
    .then((res) => {
      // Valid login if we have a status of 2xx (res.ok)
      if (res.ok) return res.json();
      throw new Error("Bad Credentials!");
    })
    .then(({ token }) => setToken(token));
}

export {
  signup,
  login,
  logout
}
```

As you can see, the `login` function is pretty similar to that of `signup`.

Whew, that should take care of the client, on to the server...

<br>

### Implement logging in on the server

When adding functionality on the server, a great place to start is defining the route.

In **routes/users.js**:

```javascript
// routes/users.js
router.post('/signup', usersController.signup);
router.post('/login', usersController.login);
```

Now we need that `usersController.login` action - in **controllers/users.js**:

```javascript
const login = async (req, res) => {
  try {
    const user = await User.findOne({ email: req.body.email });
    if (!user) return res.status(401).json({ err: "bad credentials" });
    user.comparePassword(req.body.password, (err, isMatch) => {
      if (isMatch) {
        const token = createJWT(user);
        res.json({ token });
      } else {
        return res.status(401).json({ err: "bad credentials" });
      }
    });
  } catch (err) {
    return res.status(401).json(err);
  }
};

```

**... one more thing:**

```javascript
// don't forget this:
module.exports = {
  signup,
  login
};
```

<br>
<br>


**The above code is using a `comparePassword` instance method on the `User` model that doesn't exist yet. We need it!**

When we want to add custom functionality to a particular instance of a Mongoose model, we can define instance methods like this:

In **models/user.js**:

```javascript
userSchema.methods.comparePassword = function(tryPassword, cb) {
  bcrypt.compare(tryPassword, this.password, cb);
};
```

As you can see, `bcrypt` includes a `compare` method for verifying that a plain text password matches a given hash.

Also note that we coded the `comparePassword`'s function to accept a callback function that has the same signature that bcrypt's `compare` method expects, which results in that single line of sweet code.

> Interestingly, bcrypt's `compare` method is written as an asynchronous method, thus the necessity to provide a callback. The developers of bcrypt made this decision due to the fact that hashing is a CPU intensive task. There is a synchronous version available, `compareSync`, but it's use is not recommended.

Okay, now that we've implemented logging in on the client and server, try it out!


<br>
<br>


## Step 9: Provide the token when making AJAX requests ( 8 min )

The server is going to want to verify that a user has a token and that it's valid before allowing access to protected routes.

If we are logged in, we want to ensure that we send our JWT in a header.
<br>

#### Refactor the **Main.js** service module

As just mentioned, we need to send the JWT along with each HTTP request made to any protected route on the server.

Let's refactor **Main.js** to provide the JWT when its `createPeople` function is called.

First we need to import the `getToken` function from **tokenService.js**, so we can obtain the token:

```javascript
// Add this import at the top of Main.js
import { getToken } from './tokenService';

```

Here's the refactor that adds simply adds a header:

```javascript
const createPeople = async (person) => {
    await fetch(peopleAPI, {
      method: "POST",
      headers: {
        "Content-Type": "Application/json",
        // Add this header - don't forget the space after Bearer
        Authorization: "Bearer " + getToken(),
      },
      body: JSON.stringify(person),
    });
    getPeople();
  };
```

As you can see, we added an additional header named [Authorization](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Authorization) which has been specified by the HTTP specification as the header to use for providing credentials.

Note the pre-pending of the word **Bearer** to the token, followed by a space, then the token. This is a standard to follow when using token-based authentication.

To verify that the token is being sent in the headers properly:

- Use Chrome dev tools to inspect the request in the Network tab


--- 
**YOU DO EXERCISE (5 mins)**

Please complete the following steps:

1. Add the same Athentication header for the other crud routes in `<Main>`
2. Verify in Chrome dev tools that the `Bearer <token>` appears in the request Headers for each one
---
<br>
<br>

## Step 10: Verify JWTs sent by the client and add the `user` to the Express `request` object ( 10 min )


We're sending the JWT in an `Authorization` header when creating people. 

The token already contains the user's info we can attach to the `req` object - we won't have to hit the database! No session, no querying the database - that's scalability!

> Note, this `user` property will **not** be an actual Mongoose document, it's just a plain JS object that we're grabbing from the token. This is very lightweight and performant. However, if you need to perform any CRUD on an actual document for the logged in user, you will have to query the DB to obtain the user document first using `req.user._id` provided by the token.


<br>
<br>


#### Create the Custom Middleware

On the server, we'll create a module that exports a custom middleware function responsible for:

1. Checking if there's a token in the headers of the HTTP request. For additional flexibility, we'll also check for a token being sent in the query string or the body of the request.
2. Verifying the token is valid and hasn't expired.
3. Decode the token to obtain the user data from its payload.
4. Adding the user payload to the Express request object

First, let's create a module file for the middleware function:

`$ touch config/auth.js`

Here's the custom **auth.js** middleware that we'll discuss as we type it in:

```javascript
const jwt = require('jsonwebtoken');
const SECRET = process.env.SECRET;

module.exports = function(req, res, next) {
  // Check for the token being sent in three different ways
  let token = req.get('Authorization') || req.query.token || req.body.token;
  if (token) {
    // Remove the 'Bearer ' if it was included in the token header
    token = token.replace('Bearer ', '');
    // Check if token is valid and not expired
    jwt.verify(token, SECRET, function(err, decoded) {
      if (err) {
        next(err);
      } else {
        // It's a valid token, so add user to req
        req.user = decoded.user;    
        next();
      }
    });
  } else {
    next();
  }
};
```

We're using the `jsonwebtoken` module's `verify` method to verify the token.

Again, we are checking for a token being sent in the request in three different ways by a client:

- In the header (this is how we are currently sending it)
- In a query string, or
- In the body

Adding this extra flexibility costs nothing and may allow our API to be accessed from other apps/devices more easily.


<br>
<br>


#### Mount the Custom Middleware

As we've seen before, the order that middleware is mounted matters.

For efficiency's sake, we don't want to bother checking for a token, verifying it, and adding the user payload to the Express request object unless we need to!

If all the routes in **routes/people.js** needed to be protected, we should add the middleware in **server.js** like this:

```javascript
app.use("/users", usersRouter);
// Mount our custom auth middleware to protect routes below it
app.use(require("./config/auth"));
app.use("/people", peopleRouter);
```

The above code will skip checking for a token when handling any of the **user** related routes (signing up or logging in/out).

Just be sure to mount your auth middleware before mounting any routes/routers that need access to `req.user`.


<br>
<br>

## Step 11: Protect the `/people` index client-side route ( 8 min )

In the client, it's usually a good idea to "hide" functionality that users should not be able to access.

**In the previous units, how have we been "hiding" functionality from anonymous visitors?**

Let's say that we don't want anonymous users to be able to view all people in the database.

We could just use if statements to not show certain links. OR...
<br>



#### Use Protected Rotes

Its easy to protect links in React, but what if we want to protect entire Routes?


One solution is to nest our protected Routes inide of a parent component that serves as a gatekeeper. We call these Protected Routes.

```javascript
const ProtectedRoute = ({ user, children }) => {
  if (!user) {
    return <Navigate to="/login" replace />;
  }

  return children;
};
```

In this case, if there is no user logged in the protected route component will simply redirect them to the login page. This prevents someone from typing the route into the browser and potentially bypassing a hidden link. 

If there is a user logged in the protected component will simply render its child, i.e. the protected route. 

In `Main`: 

```jsx
<Route
  path="/"
  element={
    <ProtectedRoute user={props.user}>
      <Index people={people} createPeople={createPeople} />
    </ProtectedRoute>
  }
/>

```

<br>
<br>


## Step 12 (optional): Protect any additional server-side routes with custom middleware

You can use a tiny middleware function inserted before the controller action that will check if `req.user` is present.

Example: 
```javascript
...

/*---------- Protected Routes ----------*/
// Process the token for only the routes below
router.use(require('../../config/auth'));
router.post('/', checkAuth, otherModel.create);

/*----- Helper Functions -----*/
function checkAuth(req, res, next) {
  if (req.user) return next();
  return res.status(401).json({msg: 'Not Authorized'});
}
```

<br>

## We did it! 🥳 🎉
<br>



**Tips For using Auth in Project 3**

The time to implement auth will come very early during your project's development! The earlier the better because of the number of files that much be changed, and the posibility for merge conflicts. 

