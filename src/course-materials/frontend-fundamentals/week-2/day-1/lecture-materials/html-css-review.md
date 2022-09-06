---
track: "Frontend Fundamentals"
title: "Html & Css Review"
week: 1
day: 3
type: "lecture"
---


[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# HTML & CSS Overview

Let's go over the basics of HTML and CSS! Most of you should have some
experience with this stuff already, since you completed the
[myGA](https://my.generalassemb.ly/) pre-work and each built a simple website
as part of your admissions process.

## Objectives

Developers should, at the end of the lesson, be able to:

- Write out the basic skeleton of an HTML page.
- Add CSS to an HTML file by linking to an external stylesheet with `<link>`.
- Explain at a high level how CSS styling works.
- Write CSS and use it to add styling to a basic page.

# Intro to HTML & CSS

Our first official hands on lecture into the world of web development will be to learn HTML & CSS. There are three components to every website: **HTML, CSS, and JavaScript**.

Each component has a specific function and these two tools that allow developers the visualize content in unique and different ways.

| Component  | Purpose             |
| ---------- | ------------------- |
| HTML       | Content & Structure |
| CSS        | Styling and Layout  |
| JavaScript | Interactivity       |


### Investigating Web Sites Using Chrome DevTools

We will be using **Chrome** as the default browser for the entirety of the cohort so let's open **Chrome** now if you haven't already done so. 

One of the main tools that front end developers use to investigate web sites in order to inspect html and css is **Chrome Developer Tools** or **DevTools** for short.

Although DevTools provides a list of tools to work with we will mainly use it for the following:

- View the HTML layout of a web site
- Copy entire HTML sections and supporting CSS
- Make changes to CSS and see the page immediately update
- Learn about what technologies are being used to support the site
- Run and debug JavaScript


### Open the DevTools

Let's take a look at this portfolio website created by a previous SEIR student: [https://carlynicholson.github.io/portfolio/](https://carlynicholson.github.io/portfolio/).

To open DevTools go to: **View > Developer > Developer Tools**.

<img src="https://i.imgur.com/MQJgZQu.png" />

As you can see it also provides access to the following shortcuts:

- **⌘ + ⌥ + i** to open the DevTools on Elements tab
- **⌘ + ⌥ + j** to open the DevTools on the Console tab

<!-- deployed version of the most basic [GA Press Release](http://press-release-basic.surge.sh) and use DevTools to investigate. -->

#### DevTools Tabs

Overall, there are eight main tools available in the DevTools. There are also additional extensions that we can install like React DevTools.  

During this Unit we will only focus on the following tabs:

- **Elements** 
- **Console**
- **Sources** 

#### Sources Tab

Let's start with the **sources** tab as it contains all the files and supporting assets that have been delivered to your browswer in order to view the web site.

What we see are the **index.html**. **styles.css** and **app.js** files.

<img src="https://i.imgur.com/XF2VIII.png" width=500/>

#### Elements Tab

Today our focus will be on the **Elements** tab which is most useful for:

- Inspecting the HTML elements and structure
- Inspecting the CSS of each element(s)
- :fire: Live-editing HTML & CSS on-the-fly

<img src="https://i.imgur.com/0bJiCnz.png" width=300/>

## HTML

HTML defines the structure and content of information on the page.

All HTML pages have the same basic structure:

```html
<!DOCTYPE html>
<html>
  <head>
  <!-- Meta-data goes here. -->
  </head>
  <body>
   <!-- Page content goes here. -->
  </body>
</html>
```

A web page is defined in an HTML document, ending with an **.html** extension . 

 It also requires several HTML elements that are used to define it as an HTML document and provide support.

The main elements are:

- **DOCTYPE ** - defines which type of HTML. In this case v5
- **html** - starting/ending tag for all html content
- **head** - title, meta, script, link tags added here
- **body** - everything you see on the page


```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<title>This is the title</title>
	</head>
	<body></body>
</html>
```

#### Common Tags

There are 113 HTML tags in the HTML specification - that's a lot but for the most part
you'll only use a handful of them day-to-day.

Let's take a look at [W3Schools](https://www.w3schools.com/tags/ref_byfunc.asp) and review a few categories and their tags:

- Basic HTML
- Lists
- Styles and Semantics
- Meta


Don't worry about memorizing any of these (unless you want to). Instead, learn them as you go!

#### Search Engine Optimization (SEO) Tags

Beside using the tags we just previously discussed, Front End Developers also have to keep in mind how best to implement SEO. 

We won't be delving into SEO in this class but once you are able to build scalable/responsive web apps you  might want to how focus on how best to leverage the following tags for SEO

- Meta
- Title
- Header
- H1


### Semantic Tags

HTML tags generally come in matched pairs, with the format `<tag> ... </tag>`.
The first tag is called the _opening tag_, while the second is called the
_closing tag_.

HTML5 encourages the use of _semantic_ tags whose names reflect their content
and role within the page.

Some of the new elements that HTML5 added are: `<section>`, `<header>`, `<nav>`,
`<footer>`, and `<main>`. These tags help us to structure our content better
and vastly improve accessibility for those who experience our content in
non-visual ways.

<img src="https://i.imgur.com/QgtlbjQ.png" width=500>

![semantic](https://www.jungledisk.com/blog/content/images/blog/flow-chart-semantic-html-elements.png)


### Block and Inline Elements

Historically, elements could be classified as either: **block** elements or
**inline** elements.

Block elements have built-in line breaks, causing them
to automatically _stack vertically_, while inline elements wrap within their
containing elements. One way we can distinguish block elements from inline
elements is to think of block elements as relating to parts of the page,
creating "larger" structures than inline elements.

![blockvsinline](https://media.git.generalassemb.ly/user/16103/files/25a9d380-6229-11eb-9c8c-d59909b0d57e)

Even though the HTML5 specification provides different categories for elements,
by default, all elements behave by either creating a new line or wrapping
inside of other elements. These two characteristics are important to
understand because they determine how elements can be styled.

Inline elements are only as large as their contents. This means they cannot
have `width` or `height` set with CSS (with one exception: `<img>` is
technically an inline element), and `margin` and `padding` can only be
applied to the left or right of the element. Inline elements can also
be aligned within a containing block element that has the `text-align`
property set on it (including `<img>` elements).

Examples of block and inline elements:

|  Block  |  Inline  |
|:-----------:|:------------:|
|  `<div>`  |  `<span>`  |
| `<article>` |  `<input>` |
| `<header>` | `<strong>` |
|  `<p>`  |   `<a>`  |

### Attributes

HTML attributes describe certain behavior or settings for a given HTML element.

All HTML elements support attributes of different kinds, but many attributes
can only be used on certain elements. Attributes always live within the opening
tag of an HTML element.

For example:

```html
<a href="http://google.com">Google</a>
```

Here, `href="http://google.com"` is an attribute.

Attributes will always follow the `attribute-name="value"` convention.

Below are a list of extremely helpful attributes that allow you to add
custom meta-information to your HTML elements. They become immensely helpful
when targeting these elements with CSS and/or JavaScript (we'll see this
more in jQuery).

| Attribute |   Usage  |
|:-----------:|:------------:|
| [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) | Value can only be used once, elements can only have max of one ID. Creates a unique identifier for an element. |
| [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) | Value can be used multiple times, elements can have many classes. |
| [`data-*`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-%2A) | Very helpful when used with the CSS content property and jQuery. Allows data to be bound to HTML element using custom `data-<custom>="<custom value>"` convention. |

The above attributes are globally-applicable, however some attributes only work
or make sense on certain elements. Here are some examples:

| Attribute |   Usage  |
|:-----------:|:------------:|
| [`src`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#attributes) | Location of an image to be displayed with an `img` tag. |
| [`placeholder`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input#attributes) | A value to display inside of an `input` tag when there is no value yet (before the user types). |

You'll notice the links for `src` and `placeholder` bring us to the
documentation for individual tags, like the `img` and `input` tags. This is
because these attributes do not exist globally for all elements.

<br>
<br>

## CSS

In the early days of the web, people used to style their pages using explicit
styling tags such as `<font>`, `<center>` and `<strike>` (all of which have
long been **deprecated** and should never be used today). This was inflexible,
difficult to maintain, and conflated _presentation_ with the document structure.

CSS emerged in the mid-90s as a way to make styling webpages easier. It's
core idea was to replace explicit styling in HTML with _styling rules_ which
could be applied to multiple elements; this would have the benefits of (a)
reducing duplication, and (b) separating styling instructions from content.

### Basic Syntax

CSS works by selecting some group of elements (using a special reference called
a **selector**) and defining a set of properties and values to apply to that
group of elements. The general syntax for this is:

```css
selector {
  property: value;
  property: value;
}
```

A specific example is

```css
.article-header {
  height: 100px;
  width: 100px;
  background-color: green;
}
```

- This looks similar to a JS object literal; however, one important difference
- is that key-value pairs are separated by _semicolons_ instead of _commas_.

### Properties

There are many, many CSS properties available to us. We will touch on some
important properties to know about, but we'll also want to get comfortable
searching for what we need using the [CSS property documentation](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference#index).

#### Color

We can declare the color of text inside a given element with the `color` property. To declare background color, we can use `background` or `background-color`.

```css
.article-header {
  /* Sets font color to black using a hex code */
  color: #000000;
  /* Sets background color to white using a hex code */
  background-color: #ffffff;
}
```

We can define colors many ways, but you will often see the use of hex codes, or
hexadecimal codes, which represent the red, green, and blue values of the color
using letters and numbers.

You don't need to memorize these codes, instead there are many resources to use
to find the hexadecimal code of a given color, such as the [MDN Color Keywords Hex Code Table](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value#color_keywords).

#### Fonts

We set fonts using the `font-family` property. In addition to the built-in fonts, you can also find fonts using Google Fonts or other services.

When defining a font, you can use family names, such as `'Garamond'`, or generic
names, like `serif`.

```css
.article-header {
  /* Sets the font-family to use 'Garamond'. If that font cannot be loaded,
  uses the generic serif font as a default. */
  font-family: 'Garamond', serif;
}
```

#### Sizing

We will learn more about layout with CSS, but for now we should know about
the `width` and `height` properties, used to define the sizes of different
elements.

_Note: Keep in mind that we cannot use these properties on inline elements._

```css
.article-header {
  /* Provided this is a block element, sets the width to 200px and the
  height to 300px */
  width: 200px;
  height: 300px;
}
```

### Last CSS Rule Standing

Since CSS is just a collection of style rules, one key concern is: what happens
if two rules disagree? CSS has two mechanisms to resolve these disagreements
when they come up.

#### Specificity

The first is that, if two rules disagree about the value that a property
(e.g. 'background-color') should have, a property called **specificity**
can be calculated for each selector, and the selector with higher specificity
will win out.

The short version of how specificity works is that IDs are
more 'specific' than classes, which are more 'specific' than tags, which are
more 'specific' than properties inherited from parent elements.

> Specificity is actually a very precise calculation:
>   +1000pts for each inline style attribute
>   +100pts for each ID
>   +10pts for each attribute, class, or pseudo-class
>   +1pt for each element or pseudo-element tag

#### Cascading

The second mechanism handles when two _equally specific_ rules disagree.
In that case, the last rule that is read by the browser wins! This kind of
behavior is called "cascading", and is where the "C" in CSS comes from.


#### Pseudo-class

**Pseudo-classes** are used to apply settings based either the  state of the elements, such as when a user **hovers** over an element or it's **position** in the HTML. 


```css
p:first-of-type span {
 color: #c99cd5;
}
```

<hr>

### Adding CSS to HTML

To add CSS to a page, either include it

1. Inline, within an element's opening tag as a `style` attribute.
```html
<button style="color: blue">Click me</button>
```

1. Between two `<style>` tags, typically in the the `<head>` of the document.

1. **Most Common** In a separate file referred to by a `<link>` tag, also
typically in the the `<head>`. The syntax for using a `<link>` tag is
`<link rel="stylesheet" type="text/css" href="...">` where 'href' is set to
the location of the desired stylesheet.

_Note: We will be using separate files to write 99% of our CSS styles._


## From Mockup To Implementation

As a developer it's your job to build the design based on mockups. In the real world a UX team would provide the mockups which are the end result of hours of research and user testing.

<img src="https://i.imgur.com/tJ2TNQa.png" width=500/>

<br>
<br>

Although we don't have a team of UX designers at our disposal, we can opt to rebuild something that is already beautifully designed. 

## Additional Resources

Here are some sites you might want to bookmark, if you haven't already.

- [HTML5 Cheatsheet](http://htmlcheatsheet.com/)
- [CSS Cheatsheet](http://htmlcheatsheet.com/css/)
- [Semantics in HTML](https://developer.mozilla.org/en-US/docs/Glossary/Semantics#semantics_in_html)
- [HTML5 Element Flowchart](http://html5doctor.com/downloads/h5d-sectioning-flowchart.pdf)
- [Introduction to HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML)
- [Introduction to CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS)
- [CSS specificity calculator](http://specificity.keegan.st/)

## [License](LICENSE)

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
  alternative licensing, please contact legal@ga.co.