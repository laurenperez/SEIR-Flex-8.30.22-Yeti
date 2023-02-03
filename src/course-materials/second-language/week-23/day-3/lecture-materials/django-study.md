---
track: "Second Language"
title: "Django Study"
week: 23
day: 3
type: "lecture"
---

# Django Study

## What is Django

Django is a Python framework to build backend and full-stack applications.
We will be using it to make APIs with which our client applications can
communicate.

Read through this [history of Django](https://django-book.readthedocs.io/en/latest/chapter01.html#django-s-history) before we get started.

> "At its core, Django is simply a collection of libraries written in the Python programming language. To develop a site using Django, you write Python code that uses these libraries. Learning Django, then, is a matter of learning how to program in Python and understanding how the Django libraries work."
> - [ReadTheDocs](https://django-book.readthedocs.io/en/latest/chapter01.html#required-python-knowledge)

When we worked with JavaScript, Express allowed us to add routes and more to
our applications, but it didn't really care **how** we went about doing that.
That is known as an "unopinionated" framework. Django is a little different.
It's not quite "opinionated", but it has **some** opinions. Django is all about
customization, so we will be able to pick from many built-in options as well as
scale those to meet our needs.

## Prerequisites

- Experience with API frameworks like Express
- Experience with the Python programming language

## Objectives

By the end of this, developers should be able to:

- Explain the purpose of Django and the Django Rest Framework
- Identify the pieces of an MVC framework
- Define serializers in relation to Django and DRF

## Instructions

This study will walk you through several brand new concepts - one of those
will be the idea of having "virtual environments" for our projects when
working with Python packages like Django. During this program, we will
keep it simple by just having one virtual environment for all Django-related
work.

Before we get started, create a new folder in your `unit_4`
directory named `django-env`, then move into this new directory.

```sh
cd sei
mkdir django-env
cd django-env
```

Inside of this folder, you can run through the regular preparation steps below.

#### IMPORTANT

  > Note: We will do all of our future Django work inside of this new `django-env`
  > directory.



##  1. Pipenv and Virtual Environments

We haven't talked about them yet, but virtual environments are essential when
working with Python applications. When you install packages with `pip`, you are
installing them globally. That means every project you create will use the same
packages and versions! This is generally not a good thing, since you may have
projects that use an older version of Django and will break if you suddenly
install the newest version.

We will **always** use virtual environments when working with Django, and only
left them out until now because we weren't working with any packages while
looking at Python on it's own.

Pipenv is a tool that can help you manage your virtual environments as you move
from project to project. That way, your projects will end up looking more like
this:

![pipenv diagram](https://media.git.generalassemb.ly/user/16103/files/ab165900-6db8-11ea-8f8d-e3f410d9c2b7)

Pipenv is really important, and we'll need to make sure we are inside of our
virtual environments whenever we try to run any code related to our Django
projects. Otherwise, we may not have access to the packages and versions on
which our project relies.

Here are some commands we can get used to:

| Command | Description |
| --------| ------------|
| `pipenv shell` | Creates a virtual "shell" environment based on the current working directory. Generates `Pipfile` and `Pipfile.lock` to manage package dependencies if they do not exist. |
| `pipenv install`  | Installs all packages as defined in the `Pipfile` and/or `Pipfile.lock` |
| `pipenv install django==3.0`  | Installs a package name and version, adding it to the `Pipfile` and `Pipfile.lock`. This example installs the `django` package at version `3.0` |

### Checkpoint

Try running each of the commands above in **the `django-env` folder**, one by
one and in the order listed. Take note of any & all errors you run into:

```md
your errors here
```

## 2. MVC Frameworks

MVC stands for Model-View-Controller. Simply put, this is a way of
developing applications so that the code for defining and using our data
(Model), any request-routing logic (Controller), and the user interface (View)
are all separate from one another. These pieces work together, but are what we
call "loosely coupled", so they can be changed independently without affecting
other pieces.

![mvc](https://media.git.generalassemb.ly/user/16103/files/6c809e80-6db8-11ea-8774-9fce7a0598fb)

We will be using Django to make an API, so we won't be taking advantage of
Django views (feel free to look into that on your own). Instead, we will use
the views to return JSON data from our Django API and have our front-end update
the UI based on the data it receives from requests.

> Note: _Technically_, Django is known as an [MVT](https://www.javatpoint.com/django-mvt) (Model-View-Template)
> framework since Django actually does the "controller" part for us in the
> background. For now though, it's good to have an understanding of what MVC
> means when working in Django.

Think back to the tools we've used so far, such as the MERN
(MongoDB/Mongoose, Express, React, Node) stack. What would be the model, view,
and controller if that had been an MVC framework?

```md
your answer here
```

## Django Project Structure

We will be using the Django framework to build APIs that return JSON rather than
serving client views using Django's templates. However, there are several other
pieces of Django's loosely-coupled structure that we will be utilizing quite a
bit.

### Projects vs Apps

Django is made up of projects and apps. Projects will hold apps, and apps could
be used across multiple projects.

The `django-admin` command can be used to spin up projects and apps:

| Command | Description |
| --------| ------------|
| `django-admin startproject firstproject .` | Generates project folder **into current directory**, including the very important `settings.py` file. Also creates `manage.py` file that will be used to run our project. |
| `django-admin startapp myapp`  | Generates app folder into current directory, including basic structure for apps. |

You only need a project to start your server, though apps provide the majority
of the custom functionality. Ideally, these apps are standalone and could be
used in various instances.

All apps must be registered to a project by adding it's name to the list of
`INSTALLED_APPS` defined in the project's `settings.py` file.

<br>

This diagram visualizes the relationship between the different components of a Django project:

<img src="https://i.imgur.com/1fFg7lz.png">

The quirky thing about Django is how it names its high-level components.

What we think of as a web **application**, Django calls a **project**.

Further, what we think of as part of an app's functionality, or **modules**, Django refers to as **apps**.

A Django _project_ can have many _apps_, and a Django _app_ can belong to multiple _projects_. More on this later.

<br>

#### Checkpoint

Give each of the commands listed in the table above a try! Make sure to run them
in the root of this repository exactly as they're written.

Then, try running `python manage.py runserver` (or `python3 manage.py runserver`). Take note of any errors:

```md
your errors here
```

### 3. Views and URLs

In Express, the URL and the route function are usually referred to collectively
as the route, while it is possible to separate those out. This is exactly what
Django is doing: separating the declaration of the URL from the function that
runs when the URL is requested.

Views are really just functions that take in web requests and return web
responses. Often, they are used for displaying information in the form of the
front-end's user interface.

When we used Express, we were able to make the choice between responding to
requests with JSON data or other things, such as HTML files! If we had served
HTML, our backend would have been in control of the front-end "views". Instead,
we chose to respond with JSON like an API, allowing our client-side server to
decide what to do with that data and how to display things for our user. This
is a purposeful separation, however the MVC framework is very popular and is
another way of doing web development.

For our Django application, we will be using our "views" in the same way as
Express, so we will have them respond with JSON instead of display some UI.

Don't get confused as we talk about some of these new concepts like MVC and
Views, though. Let's think about it like this:

> **If our function/class will return a response to a client, it's a view and
> should go in our `views.py` file.** If it doesn't, it's just regular app
> logic and should go somewhere else.

To set up our URLs, we need to define a list called `urlpatterns`. When we spin
up an app using `django-admin`, this is pre-provided in the `urls.py` file in
our app folder.

#### Checkpoint

Read the following and answer the questions below:
- [Django Views Documentation](https://docs.djangoproject.com/en/3.1/topics/http/views/#writing-views) (up to "Mapping URLs to views")
- [Django URLs documentation](https://docs.djangoproject.com/en/3.1/topics/http/urls/#example) (just the "Example" section)

##### Views

In your own words, describe what a "view" is in a Django application:

```md
your answer here
```

##### URLs

```py
path('articles/2003/', views.special_case_2003)
```

In the path definition above, what is the endpoint and what is the view
function?

```md
your answer here
```

### 4. Models

Models represent entities in the database your Django project is using. In our
Express projects, we used `Mongoose` to help us build model schema. With Django
we won't need any external tools, and our app should come generated with a file
for our models, `models.py`.

Read the [Django models documentation](https://docs.djangoproject.com/en/3.1/topics/db/models/) up to and including the "Using models" section.

Models are classes and can define both fields and methods. They generally are
required to have a method called `__str__` which returns a string
representation of the instance.

#### Migrations

When we define models in our Django files, we are representing what our database
should look like using Python code. This code then needs to be translated into
a SQL statement that our Postgres database can understand. This process of
telling our database about changes to our Django models so it can update the
database tables, fields, etc. is called "migrating".

**Whenever we add new models or edit model fields in an app that is being used
by our project, we will need to "generate and run migrations" so that our
database is also updated.** Otherwise, we will run into lots of errors.

We will get very familiar with the following commands:

| Command | Description |
| --------| ------------|
| `python manage.py makemigrations` | Generates files into the `migrations` folders of any apps with Model changes. These files contain statements that will be applied to the database in the correct language. |
| `python manage.py migrate`  | Runs the generated migration files, actually updating the database tables, fields, etc. |

## 5. Building an API with Django - Django Rest Framework

Django, if you remember from the history reading, was built to be used for
content-sites like blogs. It's most basic purpose is to be a full-stack
framework, which handles both serving "html" and managing a database and
routing logic.

We will not being Django in this way - our goal is to use Django as a standalone
backend which our client applications can talk to using AJAX. Our application
will serve back JSON and act more as an API than a website itself, just like our
Express backends did.

Django _could_ be used to build an API, but that wasn't really what it was meant
to do. It would take a lot of work on our end to build what we want using just
Django by itself. Instead, there are tools out there built just for folks who
want to use Django to build APIs.

We will be using one of the most popular tools for building Django APIs: [the Django Rest Framework](https://www.django-rest-framework.org/).

The Django Rest Framework (or DRF) is just a big Python library built to work
with Django. It will enable us to build the necessary features of our API much
faster than if we started from scratch.

### Serializers

A huge benefit and vital piece of the Django Rest Framework is its serializers
library. Serializers are used to "serialize" or format and validate data. We
will define them with certain requirements and validations, then use them to
either format and display data, create new data, or update existing data.

#### Extra Study

Read through the following and then answer the questions below.

- [DRF Serializers API Guide](https://www.django-rest-framework.org/api-guide/serializers/) up to and including the "Serializing
objects" section
- [DRF Serializers: Saving Instances](https://www.django-rest-framework.org/api-guide/serializers/#saving-instances) stop before the "Passing additional attributes to .save()" section
- [DRF Serializers: ModelSerializer](https://www.django-rest-framework.org/api-guide/serializers/#modelserializer) stop before "Inspecting a ModelSerializer" section

1. In your own words, what is a serializer?

```md
your answer here
```

2. Given the code `serializer = CommentSerializer(comment)`, what could we
write to access the serialized data?

```py
your answer here
```

3. Given the code `serializer = CommentSerializer(data=new_comment)`, what
could we write to save the created comment?

```py
your answer here
```

4. What is the purpose behind building serializers with the `ModelSerializer`
class? What is different from the `Serializer` class?

```md
your answer here
```