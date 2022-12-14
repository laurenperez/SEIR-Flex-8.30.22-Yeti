---
track: "React Fundamentals"
title: "MERN Stack Build Part 3"
week: 13
day: 3
type: "lecture"
---

# MERN Stack Build Part 3

<br>
<br>
<br>

## Links to Show Page

We want generate links to each person's show page so let's do the following in `Index.js`:

```jsx
import { useState } from "react"
import { Link } from "react-router-dom"

function Index(props) {

 ...

  // loaded function
  const loaded = () => {
    return props.people.map((person) => (
      <div key={person._id} className="person">

        <Link to={`/people/${person._id}`}>
          <h1>{person.name}</h1>
        </Link>

        <img src={person.image} alt={person.name} />
        <h3>{person.title}</h3>
      </div>
    ))
  }

...

export default Index
```

<br>
<br>
<br>

## The Show Page

Let's make an `update` and `delete` function for the show page, and pass the people data via props. 
Head over to `Main.js`:

```jsx
import { useEffect, useState } from "react"
import { Route, Routes } from "react-router-dom"
import Index from "../pages/Index"
import Show from "../pages/Show"

function Main(props) {
  
  ...

  const updatePeople = async (person, id) => {
    // make put request to create people
    await fetch(URL + id, {
      method: "PUT",
      headers: {
        "Content-Type": "Application/json",
      },
      body: JSON.stringify(person),
    })
    // update list of people
    getPeople()
  }

  const deletePeople = async (id) => {
    // make delete request to create people
    await fetch(URL + id, {
      method: "DELETE",
    })
    // update list of people
    getPeople()
  }

  useEffect(() => getPeople(), [])

  return (
    <main>
      <Routes>
        <Route exact path="/" element={
          <Index 
            people={people} 
            createPeople={createPeople} 
          />} />
        <Route
          path="/people/:id"
          element={
            <Show

              people={people}
              updatePeople={updatePeople}
              deletePeople={deletePeople}

            />
          }
        />
      </Routes>
    </main>
  )
}

export default Main
```

<br>
<br>
<br>

## Build the Show Page

1. We need to use the useParams() hook to access the person's `:id` from the routes path. 

2. Using that id, let's write a function to get the selected person from the people array in props and display them.

Check out [Array.prototype.find()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

`Show.js`

```jsx
import { useParams } from 'react-router-dom'
function Show(props) {
  const { id } = useParams();
  const person = props.people.find((person) => person._id === id)

  return (
    <div className="person">
      <h1>{person.name}</h1>
      <h2>{person.title}</h2>
      <img src={person.image} alt={person.name} />
    </div>
  )
}

export default Show
```

<br>
<br>
<br>

## Updating a Person

On the show page let's add:

1. State for a form

1. `handleChange` and `handleSubmit` function

1. A form in the JSX below the person

1. Use the [UseNavigate() hook from React Router v6](https://www.geeksforgeeks.org/reactjs-usenavigate-hook/) to redirect the user after a form submit

```jsx
import { useState } from "react"
import { useParams, useNavigate } from "react-router-dom"
function Show(props) {
  const { id } = useParams()
  const person = props.people.find((person) => person._id === id)
  let navigate = useNavigate()


  // state for form
  const [editForm, setEditForm] = useState(person)

  // handleChange function for form
  const handleChange = (event) => {
    setEditForm((prevState) => ({
      ...prevState,
      [event.target.name]: event.target.value,
    }))
  }

  // handlesubmit for form
  const handleSubmit = (event) => {
    event.preventDefault()
    props.updatePeople(editForm, person._id)
    // redirect people back to index
    navigate("/")
  }

  return (
    <div className="person">
      <h1>{person.name}</h1>
      <h2>{person.title}</h2>
      <img src={person.image} alt={person.name} />
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={editForm.name}
          name="name"
          placeholder="name"
          onChange={handleChange}
        />
        <input
          type="text"
          value={editForm.image}
          name="image"
          placeholder="image URL"
          onChange={handleChange}
        />
        <input
          type="text"
          value={editForm.title}
          name="title"
          placeholder="title"
          onChange={handleChange}
        />
        <input type="submit" value="Update Person" />
      </form>
    </div>
  )
}

export default Show
```

<br>
<br>
<br>

## Deleting a Person

Last Stop is adding a button on the show page to delete a user.

```jsx
import { useState } from "react";
import { useParams, useNavigate } from "react-router-dom"
function Show(props) {
  const { id } = useParams();
  const person = props.people.find((person) => person._id === id)
  let navigate = useNavigate();


  const [editForm, setEditForm] = useState(person);

  // handleChange function for form
  const handleChange = (event) => {
    setEditForm(prevState => ({
      ...prevState,
      [event.target.name]: event.target.value
    });
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    props.updatePeople(editForm);
    // redirect people back to index
    navigate("/");
  };

  const removePerson = () => {
    props.deletePeople(person._id);
    // redirect people back to index
    navigate("/")
  };

  return (
    <div className="person">
      <h1>{person.name}</h1>
      <h2>{person.title}</h2>
      <img src={person.image} alt={person.name} />
      <button id="delete" onClick={removePerson}>
        DELETE
      </button>
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={editForm.name}
          name="name"
          placeholder="name"
          onChange={handleChange}
        />
        <input
          type="text"
          value={editForm.image}
          name="image"
          placeholder="image URL"
          onChange={handleChange}
        />
        <input
          type="text"
          value={editForm.title}
          name="title"
          placeholder="title"
          onChange={handleChange}
        />
        <input type="submit" value="Update Person" />
      </form>
    </div>
  );
}

export default Show;
```

<br>
<br>
<br>

## Some Final Styling

A few more changes to our `styles.scss`:

```scss
// --------------------------
// VARIABLES
// --------------------------
$maincolor: black;
$contrastcolor: white;

@mixin white-text-black-bg {
  color: $contrastcolor;
  background-color: $maincolor;
}

@mixin black-test-white-bg {
  color: $maincolor;
  background-color: $contrastcolor;
}

// --------------------------
// Header
// --------------------------

nav {
  @include white-text-black-bg;
  display: flex;
  justify-content: flex-start;

  a {
    @include white-text-black-bg;
    div {
      margin: 10px;
      font-size: large;
    }
  }
}

// --------------------------
// Form
// --------------------------

section,
div {
  form {
    input {
      @include white-text-black-bg;
      padding: 10px;
      font-size: 1.1em;
      margin: 10px;

      &[type="submit"]:hover {
        @include black-test-white-bg;
      }
    }
  }
}

// --------------------------
// button
// --------------------------

button#delete {
  @include white-text-black-bg;
  display: block;
  margin: auto;
  font-size: 1.3em;
  padding: 10px;
}

// --------------------------
// images
// --------------------------

img {
  width: 300px;
  height: 300px;
  border-radius: 90px;
  object-fit: cover;
}
```

<br>
<br>
<br>

## Deploy


###Netlify
Add a `netlify.toml` with the following:

```toml
[[redirects]]
  from = "/*"
  to = "/"
```

1. Push frontend repo to github

1. Connect to netlify

1. Done


<br>
<br>
<br>

Solution Code: 

**[Finished Backend App Example](https://git.generalassemb.ly/laurenperez-ga/people-backend)**
**[Finished Frontend App Example](https://git.generalassemb.ly/laurenperez-ga/people-frontend)**

<br>
<br>
<br>

## Lab - Complete Your Full Stack MERN App

Complete your app using the steps of todays lessons adding the following:

1. The ability see an individual show pages

2. The ability edit a model

3. The ability to delete a model
