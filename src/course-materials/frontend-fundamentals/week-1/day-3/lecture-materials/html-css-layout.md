---
track: "Frontend Fundamentals"
title: "Html & Css Layout"
week: 1
day: 3
type: "lecture"
---

[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# HTML & CSS Layout

## Prerequisites

- Basic knowledge of HTML
- Basic knowledge of CSS

## Objectives

By the end of this talk, developers should be able to:

- Explain the box model.
- Contrast `px`, `%`, and `em` measurements.
- Use Flexbox to create rich layouts.
- Use media queries to change CSS rules based on screen size.
- Explain the difference between 'static' and 'fixed' positioning.

## CSS Layout

So far, we've mostly talked about using CSS for styling our page - adding
colors, fonts, etc. In this talk, we'll be examining how CSS can be used to
control a webpage's layout.

Back in the day, web layout was achieved with just HTML, let's look at an
example of that, just for fun:
[# 90's CSS Example](https://www.spacejam.com/1996/)

Today, layout is specified with CSS. It's easier, more modular, and it looks
much better too!

## Box Model

In addition to setting an element's `height` and `width`, elements have three
other properties that explicitly control spacing:

1. 'Border' sets a perimeter around an element. In addition to specifying a
   color and a particular type of border, you can also specify a thickness.
2. 'Margin' specifies spacing between the outside of an element's border and any
   adjacent elements.
3. 'Padding' specifies spacing between the inside of an element's border and the
   contents of that element (which includes `height` and `width`!)

Together, these attributes form _the box model_, a way of describing the space
taken up by an element.

![Box Model](https://media.git.generalassemb.ly/user/16103/files/0e74e100-623d-11eb-95c8-19a8c78922ea)

Every one of these attributes, including `height` and `width`, can be specified
in the following terms:

- `px` : fixed number of pixels.
- `%`  : size is relative to element that contains it ("parent"). As a value of
  `height`, `%` is relative to the parent's `height`, but for every other
  dimension, `%` is relative to the parent's `width` value.
- `em` : ties dimensions to *font size* - one `em` is the width of the letter
  'm'. For all dimensions except `font-size`, `em` will refer to the font size
  of the element; as a value for `font-size`, `em` refers to the font size of
  the *parent*.

### Demo: Box Sizing

The Box Model explains how CSS `width` and `height` is Calculated.

By default, the size of an element's height or width can be calculated with:

> `width` = `width` + `padding` + `border`
> and
> `height` = `height` + `padding` + `border`

This can be problematic when trying to create a layout or position things logically on the page, as it does not include the margin of the element.

To fix this, we can set the element's CSS [`box-sizing`](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) property to `border-box`
instead of the default `content-box`.

> In the developer tools' "Elements" tab, you can find all of the styles applied
> to elements on the page, including an interactive display of the box model
> and a listing of all calculated values, such as `height` and `width`. We
> can use these tools to help us understand how our elements are being rendered.

## Positioning

All of the rules that we'll learn about CSS layout are based on one paradigm of
positioning, called 'static' positioning. Static positioning is the default
positioning model for elements. They are displayed in the page where they
rendered as part of normal HTML flow.

![Static](public/images/static.gif)

Though there are others, the most significant type of positioning besides
`static` positioning is `fixed` positioning. `fixed` positioning defines the
position of an element with respect to the _view window_, essentially 'fixing'
its position on the screen. Fixed positioning is frequently used in parallax
scrolling, or for holding a navigation bar at the top/side/bottom of the screen.

![Fixed](public/images/fixed.gif)

### Relative and Absolute Positioning ( Further Reading - After Class )

[Research](https://developer.mozilla.org/en-US/docs/Web/CSS/position) relative
and absolute positioning in CSS. How are they used? A helpful reference for
understanding is [this CSS-tricks blog post](https://css-tricks.com/absolute-positioning-inside-relative-positioning/).


## Additional Resources

- [Getting Started With CSS Layout](https://www.smashingmagazine.com/2018/05/guide-css-layout/)
- [4 Types of CSS Positioning, Explained](http://www.nigelbuckner.com/downloads/handouts/web/pos-explained/index.html)
- [flexplorer](http://bennettfeely.com/flexplorer/)
- [Interactive CSS Intro](https://rupl.github.io/unfold/)
- [Interactive Box Model Demo](http://guyroutledge.github.io/box-model/)
- [CSS Floats](https://css-tricks.com/all-about-floats/)
- [Visual Guide to Flexbox](https://scotch.io/tutorials/a-visual-guide-to-css3-flexbox-properties)
- [Flexbox Cheatsheet](https://yoksel.github.io/flex-cheatsheet/)
- [Flexbox Demos](https://demos.scotch.io/visual-guide-to-css3-flexbox-flexbox-playground/demos/)