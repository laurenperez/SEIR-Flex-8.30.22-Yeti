<img src="https://i.imgur.com/VunmGEq.jpg">

# React Fundamentals - Handling Input and Events

## Learning Objectives

| Students Will Be Able To:                             |
| ----------------------------------------------------- |
| Handle Events in React                                |
| Optionally Pass Arguments to Event Handlers           |
| Use "Controlled Inputs" in React                      |
| Properly Update Objects/Arrays in State               |
| Prevent a `<form>` From Being Submitted to the Server |


## Road Map

- Setup
- PART 1 : Attaching Event Handlers Using Event Props
- PART 2 : Adding the New To-Do to the To-Do's list
- PART 3 : Using Forms for validation in React
- PART 4 : Using State to Handle Multiple Inputs (OPTIONAL EXTRA STUDY)



<br><br>


## PART 1 : Attaching Event Handlers Using Event Props

### Browser Events in React

Like many things in React, event handling is a little different than vanilla Javascript.


#### Using Handler Functions for Events in React

In React, we do not add event listeners using JavaScript's `addEventListener` method.

**Let's review the old way:**

```html
<p id="my-element">Hello World</p>
```

```JS
let element = document.getElementById("my-element")
element.addEventListener("click", (event)=>{
  element.style.color = "blue"
})
```

**What does the above code do?**

Instead, we pass **event handling functions** as "event" props, e.g. functions that are passed down to child components as props.



### Event Handling Review

- The names for event props are camelCased (`onClick`). In HTML, the attribute would be `onclick`. Here's the [list of events](https://facebook.github.io/react/docs/events.html#supported-events) supported by React.

- The JS expression (always within curly braces) assigned to an event prop must evaluate to a **function** or  a **function type**, but **not** a function call (unless that function call returns a function).

- React automatically implements event delegation and makes event handling more efficient in a React app by managing events internally - check out React's [Synthetic Event Object](https://reactjs.org/docs/events.html) to learn more.

**Review - what is event delegation in JS?**


<br><br>

## Lets Make a ToDo List

<br><br>

## Setup

In this lesson we will be building a react to do list from scratch using `npx create-react-app@latest react-todo-list`.

### Using "Controlled Inputs" in React

<br><br>


### 💪 Warm up Exercise - SETUP : Create 2 new components (5 minutes)

1. Create two components `<ToDoList>`  &  `<ToDoForm>` in their own modules (named according to best practices).

2. `<ToDoForm>` should just render `<h2>New To-Do</h2>` for now.

3. `<ToDoList>` should just render an empty `<ul></ul>` for now. 

3. Import ToDoList and ToDoForm in **App.js**.

<br>
<br>

### `<Form>` Tags in React Are Not Required

`Forms` are not required to wrap input elements in SPAs because we send data to the server using AJAX, not by submitting a form with a `form action`. 

However, we often still use forms in React for things like validation, layout/styling using CSS frameworks, etc.

We'll begin by adding a to-do without using a form. Then, we'll add a form later to perform validation and to learn how to prevent them from accidentally triggering a full-page refresh when submitted.

<br>
<br>

### Controlled Components (`<input>`)

In a typical HTML page, input elements such as `<input>`, `<textarea>` and `<select>`, maintain their own internal state.

However, handling input the "React way" is different:

 - The value of an input, (what is displayed) is maintained by a `state` variable in the component.

 -  Updating that state requires assigning an event handler function to the `onChange` prop.


This scenario, is what React refers to as a [controlled component/input](https://reactjs.org/docs/forms.html#controlled-components).


<br>
<br>

### Adding a Controlled `<input>`

Let's add the following JSX to **ToDoForm.jsx**:

```jsx
export default function ToDoForm() {
  return (
    <>
      <h2>New To-Do</h2>
      <input placeholder="New To-Do" />
      <button>ADD TO-DO</button>
    </>
  )
}
```

> Reminder: React Elements have attributes such as `placeholder` that provide the functionality of their HTML attribute equivalents.

Now let's make it a controlled input by first adding state for the `<input>`

<br>
<br>

#### 💪 Practice Exercise - Add `newTodo` State (2 minutes)

1. Use the `useState` hook to add state named `newTodo`.
2. Initialize `newTodo` to an empty string.
3. You know what to name the setter - right?


 > Hint: Don't forget to import `useState`

<br>
<br>


### The Key to Controlled Inputs: `value` & `onChange` Props

All controlled inputs must include the following two props:

- `value`: This prop is used to bind the state to the input's value (what is displayed).

- `onChange`: This event prop will call the provided callback function whenever the user types in the input. Within the callback, we need to update the state variable bound to `value`.

<br>

### Bind State to the `value` Prop

Let's start by binding the `newToDo` state you just defined to the `value` prop of the `<input>` in **ToDoForm.jsx**:

```jsx
<input value={newTodo} placeholder="New To-Do" />
```

And that's it. Easy peasy. To test it out, try initializing the state var to something other than empty string.


<br><br>


### Handling the `onChange` Event

You just saw how the `<input>` displays the value of the state it is bound to using the `value` prop.

However, try typing in the input - nothing happens because we need to update the state as the user types.

Here's a simple way for now:

```jsx
export default function ToDoForm() {

  const [newTodo, setNewTodo] = useState("") // our input's state

  return (
    <>
      <h2>New To-Do</h2>
      <input
        value={newTodo}
        onChange={(evt) => setNewTodo(evt.target.value)}
        placeholder="New To-Do"
      />
      <button>ADD TO-DO</button>
    </>
  )
}
```

When React invokes the handler (function), it passes in an event object as an argument for which we defined a parameter, `evt`. Then, just like in vanilla JS, the event object can be used to access the input's internal value via `evt.target.value`.

<br><br>

### ❓ Review Questions

1. What is wrong with the following code?

   ```jsx
   <div onClick={setStateValue(newValue)}> Click Me! </div>
   ```

2. **True or False:** In a SPA, forms tags are required to send data to the server.

3. What are the **two props** that must be added to a controlled input in React?


<br><br>

<hr>

<br><br>



## PART 2 : Adding the New To-Do to the To-Do's list

Now that we have the `<input>` working and know how to respond to events in React, we can add a new to-do `<li>` by:

- Adding an `onClick` handler to the `ADD TO-DO` button.

- When the button is clicked, add the new to-do to the `todos` state using the `setTodos` setter function.

- Clear the input for a better UX.


<br><br>



### Adding the `onClick` Handler to the `ADD TO-DO` Button

We're not going to be able to perform all of the required logic inline this time, instead let's write an additional function within **ToDoForm.jsx**:

```jsx
export default function ToDoForm() {

  const [newTodo, setNewTodo] = useState("");

  // Add this new handler 
  function handleAddTodo() {
    console.log(newTodo);
  }

  ...
```

We're console logging the `newTodo` state value as a baby step.

Now let's attach this new handler function by adding an `onClick` prop to the button:

```jsx
<button onClick={handleAddTodo}>ADD TO-DO</button>
```

<details><summary>❓ Wait, should we wrap <code>handleAddTodo</code> with an arrow function in this case?</summary>
<p>

**Nope. We don't need to provide any arguments to `handleAddTodo` when invoking it, so we can simply provide the function itself.**

</p>
</details>

<br><br>

### Updating the `todos` State

A few considerations...

<details><summary>❓ Which component "owns" the <code>todos</code> State?</summary>
<p>

**`<App>`**

</p>
</details>

<details><summary>❓ Which component "owns" the setter function used to update the <code>todos</code> State?</summary>
<p>

**`<App>`**

</p>
</details>

<details><summary>❓ Does the <code>ToDoForm</code> have access to the <code>setTodos</code> setter function?</summary>
<p>

**No**

</p>
</details>


<br><br>

### Time to Lift State!


So, we could solve our dilemma by passing a reference to `setTodos` as a prop. However, we also would need to pass a reference to the `todos` state as well to use with the spread operator.

We certainly could pass those props without much hassle, however, it just doesn't feel right - instead, it's a better practice to always update state within the component that owns that state, in this case, `<App>`.

Let's add a new function within **App.js** that we can then pass to `<ToDoForm>` as a prop:

```jsx
const [todos, setTodos] = useState(true)

// Add this function
function addTodo(todo) {
  // Remember! We replace state, don't mutate it!
  setTodos([...todos, todo])
}
```

We are using the spread syntax to include (spread) the current elements of the `todos` array within a new array literal, adding the new todo at the end.


<br><br>


#### 💪 Practice Exercise - Pass `addTodo` to the `<ToDoForm>` Component (2 minutes)

1. Pass the `addTodo` function from **App.js** to the `<ToDoForm>` component using a prop with the same name.

2. Update `<ToDoForm>` to destructure the `addTodo` prop being passed to it.

<br><br>

Now we're ready to add the to-do by updating the `handleAddTodo` function in **ToDoForm.jsx**:

```jsx
function handleAddTodo() {
  // console.log(newTodo);
  addTodo(newTodo)
}
```
<br><br>

#### Render Todos in ToDoList Component

1. Pass the todos state from <App> to <ToDoList> as props

2. Add a map function in your <ToDoList> component to create an <li> for each todo in the props list


Test it out! 

<br><br>

**Now let's improve the UX...**

#### 💪 Practice Exercise - Reset the `<input>` Back to an Empty String (2 minutes)

- Improve the UX by adding a single line of code to `handleAddTodo` that clears out the value displayed in the `<input>`.

> Hint: Controlled inputs get their displayed value from state!

<br><br>

<hr>

<br><br>



## PART 3 : Using `<form>` React Elements for Validations and Multiple Inputs

Now that we've shown that using a `<form>` in React is optional, let's refactor "React To-Do" to use a `<form>` so that we can learn how to prevent them from being submitted to the server, etc.

### Add the `<form>` React Element

Let's start by refactoring to use a `<form>`:

```jsx
<form onSubmit={handleAddTodo}>
  <input
    value={newTodo}
    onChange={(evt) => setNewTodo(evt.target.value)}
    placeholder="To-Do"
  />
  <button type="submit">ADD TO-DO</button>
</form>
```

> Key Point: Forms in React will never have an `action` or `method` prop/attribute because they are never submitted to a server!

Note that we've:

- Removed the `onClick` from the button and changed its type to `type="submit"`

- Added the `onSubmit` event prop to the form - this is a more typical approach when working with forms.

With the above refactor, a new to-do will only appear for a second before the form triggers a refresh when the submit button is clicked...

## Preventing a `<form>` From Being Submitted

To prevent a form from being submitted, we need to invoke the `preventDefault()` method on the event object.

First we need to add a parameter to the `handleAddTodo` function to accept the event object that React passes automatically:

```jsx
//
function handleAddTodo(evt) {
  ...
```

Next, we invoke the `preventDefault()` method first thing:

```jsx
function handleAddTodo(evt) {
  evt.preventDefault()
  addTodo(newTodo)
  setNewTodo("")
}
```

Now there's no more page refresh when adding a new to-do!

## Validating Data In a `<form>`

Because all of the data for a form's inputs will be held in state, it's possible to validate that state every time it changes using code in the handler; setting additional state, e.g, `isValid`, accordingly.

However, we can easily take advantage of the form's HTML5 validation capabilities to ensure that data has been entered as desired.

For example, we can add both the [required](https://www.w3schools.com/tags/att_input_required.asp) and [pattern](https://www.w3schools.com/tags/att_input_pattern.asp) attributes to HTML inputs to validate their data. React works with those props too!

Let's prevent empty to-dos from being created:

```jsx
<input
  value={newTodo}
  onChange={(evt) => setNewTodo(evt.target.value)}
  placeholder="To-Do"
  required
  pattern=".{4,}"
/>
```

Now, thanks to built-in HTML5 validation, the form cannot be submitted unless at least 4 characters are entered!

We've only scratched the surface of validation built-into browsers. You can begin to learn more by reading about the comprehensive [constraint validation Web API here](https://developer.mozilla.org/en-US/docs/Web/API/Constraint_validation).






## (OPTIONAL) PART 4 : Using State to Handle Multiple Inputs


We've seen how to handle a single input within a form. However, we'll usually need to use several inputs for gathering all of the data we need.

For extra practice, let's create a new react code sandbox where we can add a form with multiple inputs.

### Set Up the New Form

Copy and paste this into the **App.js**:

```jsx
import React from "react"
import "./styles.css"

export default function App() {
  return (
    <div className="App">
      <form>
        <label>
          <span>NAME</span>
          <input name="name" />
        </label>
        <label>
          <span>EMOTION</span>
          <select name="emotion">
            <option value="😁">Happy</option>
            <option value="😐">Neutral</option>
            <option value="😠">Angry</option>
          </select>
        </label>
      </form>
    </div>
  )
}
```

Sure, there's only two inputs in this example, but the approach we're going to use applies to any number of inputs.

Here's just a bit of CSS to add to **App.css**:

```css
form {
  display: grid;
  grid-template-columns: auto auto;
  width: 20rem;
  text-align: left;
  font-size: 1.5rem;
}
```

### Do We Really Need a State Variable For Each Input?

Imagine that we had a form with a dozen or more inputs. Based on what you've seen thus far you might think that you would need to have a dozen or more state variables with their own setter functions, etc.

Having dedicated state for each input is not only verbose, it's also not convenient when having to send that data to a server.

Instead, as we learned in the lesson about state, a single state can be anything, including an object and that's our ticket:

```jsx
export default function App() {
  const [formData, setFormData] = useState({
    name: "",
    emotion: "😁"
  });
  ...
```

It's no coincidence that the name of the properties on the `formData` object match the names assigned to the `name` props on the inputs. Doing so allows for the following concise `onChange` handler that will update the state for any number of inputs.

### Add the `onChange` Handler Function

Here's the only handler function we need:

```jsx
export default function App() {
  const [formData, setFormData] = useState({
    name: "",
    emotion: "😁"
  });

  function handleChange(evt) {
    // Replace with new object and use a computed property
    // to update the correct property
    const newFormData = { ...formData, [evt.target.name]: evt.target.value };
    setFormData(newFormData);
  }
  ...
```

> [Computed property names](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) allow for a JS expression to dynamically determine the key of the property.

Similar to how we used the spread operator with arrays, we're using it here to first spread all of the existing properties within a new object literal. Then, any additional properties are comma separated and are either added to the object, or used to updated existing properties.

### Bind the Handler to the `onChange` Event in the Inputs

Each input needs to have an `onChange` prop added so that the handler is invoked:

```jsx
<input name="name" onChange={handleChange} />
...
<select name="emotion" onChange={handleChange}>
```

> Note: We don't use event delegation in React because React's event system is implementing it automatically behind the scenes.

That's all it takes! We could use React Developer Tools to verify it's working, but let's add an `<h1>` to display the results instead:

```jsx
...
</form>
<h1>{formData.name} is {formData.emotion}</h1>
```

## The Keys to Programming in React

Now that you've worked a bit with the fundamentals of React, let's look a few **key** thoughts every React developer considers when developing a React application - whether they know it or not:

1. **We code components to render (visualize) application-state**, for example, render a `<ToDoListItem>` component for each to-do in the `todos` application-state array.
2. **We can code components to render other components based upon UI-state**, for example, hide/show a component based upon `showDetails` UI-state, disable a button based upon `isValid`, etc.
3. **In response to user-interaction, we apply any necessary program logic and/or calculations and ultimately update all impacted state causing the components to re-render.**

## React Inputs Cheat Sheet



| React Fundamental                    | Summary                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Event Handling                       | <ul><li>Instead of using `addEventListener`, in React we connect event handlers (functions) to events using event props on React Elements.</li><li>Examples of event props are: `onClick`, `onChange`, `onSubmit`, etc.</li></ul>                                                                                                                                                                                                                                                                                                                                                  |
| Handling Input                       | <ul><li>[Controlled Inputs](https://reactjs.org/docs/forms.html#controlled-components) are the React way to gather input from the user with `<input>`, `<select>` or `<textarea>` React Elements.</li><li>A controlled input must include both `value` & `onChange` props.</li><li>Forms are optional in a SPA but they can be beneficial for validation & CSS layout/formatting. If forms are used, be sure to prevent them from being submitted to the server by calling `preventDefault()` on the event object from within the `onSubmit` event handler.</li></ul>              |
| **The Keys to Programming in React** | <ul><li>**We code components to render (visualize) application-state**, for example, render a `<ToDoListItem>` component for each to-do in the `todos` application-state array.</li><li>**We can code components to render other components based upon UI-state**, for example, hide/show a component based upon `showDetails` UI-state, disable a button based upon `isValid`, etc.</li><li>**In response to user-interaction, we apply any necessary program logic and/or calculations and ultimately update all impacted state causing the components to re-render.**</li></ul> |

#### Congrats on Handling Input and Events in React!

## ❓ Essential Questions

1. An input displays the value of the state assigned to its **\_\_\_\_** prop.

2. An input must use a **\_\_\_\_** prop to bind a handler function.

3. What must the above handler function's code update?

4. What method needs to be invoked to prevent a form from triggering a full-page refresh?

## References

- [React Docs - Synthetic Events](https://reactjs.org/docs/events.html)
