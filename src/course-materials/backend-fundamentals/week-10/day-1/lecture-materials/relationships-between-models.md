---
track: "Backend Fundamentals"
title: "Creating A Relationship Between Two Models"
week: 2
day: 5
type: "lecture"
---

# Creating A Relationship Between Two Models

**Today we are creating what is called a _one to many_ relationship between _Authors_ and the many _Articles_ they write.** 

<br>
<br>
<br>

## Lesson Objectives

1. Add Articles Array to Author Model
1. Display Authors on New Article Page
1. Creating a new Article Pushes a Copy Onto Author's Articles Array
1. Display Author With Link on Article Show Page
1. Display Author's Articles With Links On Author Show Page
1. Deleting an Article Updates An Author's Articles List
1. Updating an Article Updates An Author's Articles List
1. Deleting an Author Deletes The Associated Articles
1. Change Author When Editing an Article

<br>
<br>
<br>

## Getting Set Up

[You can find todays starter code here. ](https://git.generalassemb.ly/laurenperez-ga/blog)

**Fork this repo so that you have your own copy in github.**  ( The fork button is in the top right coner of the page. )

**Now clone this copy to your local machine.**

1. Click the green code button. 

2. Select HTTPS and copy the link generated. 

3. In your terminal:

``` shell
git clone https://git.generalassemb.ly/laurenperez-ga/blog
```
 4. `cd blog`

 5. `npm install`

 6. Create a `.env` file with your PORT and DATABASE_URL - set the name of the DB to `blog`

You are ready to code! 


<br>
<br>
<br>

### Review the starter code...

Take a tour of the repo. As a new developer you will spend a significant amount of time getting to know new code bases at your first job. This means looking through the folder set up, taking note of how things are organized and what patterns you see in the code. 

<br>
<br>
<br>

## Add Articles Array to Author Model

Now lets create a relationship between Authors and the Articles they write. 
Add a new property to the Author schema called articles.

models/authors.js

```javascript
const mongoose = require("./connection");
const Article = require("./article");

const { Schema, model } = mongoose;

const authorSchema = new Schema({
  name: String,
  articles: [Article.schema], //  <-- here
  },
  { timestamps: true }
);

const Author = model("Author", authorSchema);

module.exports = Author;
```

<br>
<br>
<br>

## Display Authors on New Article Page

Require the Author model in `controllers/articles.js`:

```javascript
const Author = require("../models/author")
```

<br>
<br>
<br>

Find all Authors When Rendering New Page:

```javascript
router.get("/new", (req, res) => {
  Author.find({}, (err, allAuthors) => {
    res.render("articles/new.ejs", {
      authors: allAuthors,
    })
  })
})
```

<br>
<br>
<br>

Create A Select Element in `views/articles/new.ejs`:

```html
<form action="/articles" method="post">
  <!-- This is the new code -->
  <select name="authorId" class="browser-default">
    <% for(let i = 0; i < authors.length; i++) { %>
    <option value="<%= authors[i]._id %>"><%= authors[i].name %></option>
    <% } %>
  </select>
  <!-- end of new code -->
  <br />
  <input type="text" name="title" />
  <br />
  <textarea name="body" />
  <br />
  <input type="submit" value="Publish Article" />
</form>
```

<br>
<br>
<br>

## Creating a new Article Pushes a Copy Onto Author's Articles Array

Inside of `controllers/articles.js`:

```javascript
router.post("/", (req, res) => {
  Author.findById(req.body.authorId, (err, foundAuthor) => {
    Article.create(req.body, (err, createdArticle) => {
      //req.body.authorId is ignored due to Schema
      foundAuthor.articles.push(createdArticle)
      foundAuthor.save((err, data) => {
        res.redirect("/articles")
      })
    })
  })
})
```

**NOTE: req.body.authorId is ignored when creating Article due to Article Schema**

<br>
<br>
<br>

## Display Author With Link on Article Show Page

controllers/articles.js:

```javascript
router.get("/:id", (req, res) => {
  Article.findById(req.params.id, (err, foundArticle) => {
    Author.findOne({ "articles._id": req.params.id }, (err, foundAuthor) => {
      res.render("articles/show.ejs", {
        author: foundAuthor,
        article: foundArticle,
      })
    })
  })
})
```

views/articles/show.ejs:

```html
<h2><%= article.title %></h2>
<small>by: <a href="/authors/<%= author._id %>"><%= author.name %></a></small>
```

<br>
<br>
<br>

## Display Author's Articles With Links On Author Show Page

views/authors/show.ejs:

```html
<section>
  <h2>Articles Written By This Author:</h2>
  <ul>
    <% for(let i = 0; i < author.articles.length; i++) { %>
    <li>
      <a href="/articles/<%= author.articles[i]._id %>"
        ><%= author.articles[i].title %></a
      >
    </li>
    <% } %>
  </ul>
</section>
```

<br>
<br>
<br>

## Deleting an Article Updates An Author's Articles List

controllers/articles.js

```javascript
router.delete("/:id", (req, res) => {
  Article.findByIdAndRemove(req.params.id, (err, foundArticle) => {
    Author.findOne({ "articles._id": req.params.id }, (err, foundAuthor) => {
      foundAuthor.articles.id(req.params.id).remove()
      foundAuthor.save((err, data) => {
        res.redirect("/articles")
      })
    })
  })
})
```

<br>
<br>
<br>

## Updating an Article Updates An Author's Articles List

controllers/articles.js

```javascript
router.put("/:id", (req, res) => {
  Article.findByIdAndUpdate(
    req.params.id,
    req.body,
    { new: true },
    (err, updatedArticle) => {
      Author.findOne({ "articles._id": req.params.id }, (err, foundAuthor) => {
        foundAuthor.articles.id(req.params.id).remove()
        foundAuthor.articles.push(updatedArticle)
        foundAuthor.save((err, data) => {
          res.redirect("/articles/" + req.params.id)
        })
      })
    }
  )
})
```

<br>
<br>
<br>

## Deleting an Author Deletes The Associated Articles

controllers/authors.js

```javascript
const Article = require("../models/article")

//...farther down the file
router.delete("/:id", (req, res) => {
  Author.findByIdAndRemove(req.params.id, (err, foundAuthor) => {
    const articleIds = []
    for (let i = 0; i < foundAuthor.articles.length; i++) {
      articleIds.push(foundAuthor.articles[i]._id)
    }
    Article.remove(
      {
        _id: {
          $in: articleIds,
        },
      },
      (err, data) => {
        res.redirect("/authors")
      }
    )
  })
})
```

<br>
<br>
<br>

## Change Author When Editing an Article

controllers/articles.js

```javascript
router.get("/:id/edit", (req, res) => {
  Article.findById(req.params.id, (err, foundArticle) => {
    Author.find({}, (err, allAuthors) => {
      Author.findOne(
        { "articles._id": req.params.id },
        (err, foundArticleAuthor) => {
          res.render("articles/edit.ejs", {
            article: foundArticle,
            authors: allAuthors,
            articleAuthor: foundArticleAuthor,
          })
        }
      )
    })
  })
})
```

`views/articles/edit.ejs`

```html
<form action="/articles/<%= article._id %>?_method=PUT" method="post">
  <select name="authorId" class="browser-default">
      <% for(let i = 0; i < authors.length; i++) { %>
          <option
              value="<%= authors[i]._id %>"
              <% if(authors[i]._id.toString() === articleAuthor._id.toString()) { %>
                  selected
              <% } %>>
              <%=authors[i].name%>
          </option>
      <% } %>
  </select>
  <br/>
  <input type="text" name="title" value="<%= article.title %>"/><br/>
  <textarea name="body"><%= article.body %></textarea><br/>
  <input type="submit" value="Update Article"/>
</form>
```

<br>
<br>
<br>

**NOTE: to compare ObjectIds, which are objects, you must first convert them to Strings (e.g. articleAuthor.\_id.toString())**

<br>
<br>
<br>

Update the PUT route in `controllers/articles.js`

```javascript
router.put("/:id", (req, res) => {
  Article.findByIdAndUpdate(
    req.params.id,
    req.body,
    { new: true },
    (err, updatedArticle) => {
      Author.findOne({ "articles._id": req.params.id }, (err, foundAuthor) => {
        if (foundAuthor._id.toString() !== req.body.authorId) {
          foundAuthor.articles.id(req.params.id).remove()
          foundAuthor.save((err, savedFoundAuthor) => {
            Author.findById(req.body.authorId, (err, newAuthor) => {
              newAuthor.articles.push(updatedArticle)
              newAuthor.save((err, savedNewAuthor) => {
                res.redirect("/articles/" + req.params.id)
              })
            })
          })
        } else {
          foundAuthor.articles.id(req.params.id).remove()
          foundAuthor.articles.push(updatedArticle)
          foundAuthor.save((err, data) => {
            res.redirect("/articles/" + req.params.id)
          })
        }
      })
    }
  )
})
```


### You now have two models in a _one to many_ relationship! 