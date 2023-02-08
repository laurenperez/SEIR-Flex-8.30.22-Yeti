---
track: "Second Language"
title: "Django Relationships"
week: 24
day: 1
type: "lecture"
---

[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Django Relationships

## Prerequisites

- [django-crud](https://git.generalassemb.ly/ga-wdi-boston/django-crud)

## Objectives

Developers should be able to:

- Create one-to-many relationships with Django
- Create many-to-many relationships with Django
- Diagram relationships with ERDs

## Preparation

1. Fork and clone this repository **into the `django-env` folder**.
 [FAQ](https://git.generalassemb.ly/ga-wdi-boston/meta/wiki/ForkAndClone)
1. Run `pipenv shell` **inside the `django-env` folder** to start up your
 virtual environment.
1. Change into this repository's directory
1. Create a psql database with `createdb "django-relationships"`
4. Generate and run migrations with `python manage.py makemigrations` and
   `python manage.py migrate`
5. Run the server with `python manage.py runserver`

## Building an API with Relationships

So far we have built a pretty basic application with Django and Python, but
now we are going to step it up and add in relationships to our models. To guide
us, we have plenty of documentation about the API we are building, including
user stories and ERDs (Entity Relationship Diagrams).

- This repository has our V1 for library app. 
- Our docs show us that for V2 and V3, we are adding relationships to the different models.

## Library V2

 - For V2 of our library app we need to add a new resource called `Author` and connect it to our `Book` resource. 
  - We will be building a one-to-many relationship where a `Book` has one `Author` and an `Author` can be referenced on many `Book` resources.

We can also think of this as an `Author` having multiple `Book`s, which is how
we diagram our ERD:

`Author` -|---< `Book`

<br><br>

### Code-Along: Create Author - One-to-Many Relationships

To get started let's test the `Author` starter code and
make sure that resource works on it's own before connecting it to `Book`.

#### 1. Connect Book to Author

Now, to connect `Book` and `Author` we have to look to the model first. This
step will involve us adding a field to `Book` that references the `Author`
resource.

🟢  **Code:**
```py

class Book(models.Model):
  title = models.CharField(max_length=100)
  # author = models.CharField(max_length=100)  <- replace with reference to Author Model
  author = models.ForeignKey('Author', related_name='books', on_delete=models.CASCADE)
  created_at = models.DateTimeField(auto_now_add=True)
  updated_at = models.DateTimeField(auto_now=True)
```

Let's break this down:

- `models.ForeignKey()`: creates a foreign key reference to the `'Author'` model.
This will allow the `author` field on `Book` to point to a specific `Author` in
our database.Foreign keys are used in databases to connect resources.

- `related_name`: this is what we will be able to access on the other side of
our relationship to see what books an author wrote.

- `on_delete=models.CASCADE`: this will cause the books our author wrote to be
deleted if the author is as well.
    - We have [lots of options](https://stackoverflow.com/questions/38388423/what-does-on-delete-do-on-django-models) of what we can put here.

That's all we need to do to actually connect these resources!

We can ping our endpoints to create or update a book and send along an ID value
for the `author` field. This will be that "foreign key" that points to the
author who wrote the book.

However, we can't see the books that our authors wrote on the other side yet!

#### 2. Update Author Serializer

The `AuthorSerializer` can be updated to reference the `BookSerializer` and then return the `'books'` field. This should match the name on the `Book` model for the `related_name`.

🟢  **Code:**
```py
class AuthorSerializer(serializers.ModelSerializer):
    books = BookSerializer(many=True, required=False) # <- Add this reference to the Books Serializer
    class Meta:
        model = Author
        fields = ('first_name', 'last_name', 'books') # <- update fields
```

Now, when an `Author` is returned from the database it will include a reference
to that author's `books`.

- NOTE: To make adding books OPTIONAL during the creation of an Author, pass the flag  `required=False`


## Lab : Do it again! 

## Hospital V2

### Lab: `Doctor` -|---< `Patient`

Time to work on our next app V2: hospital.

The [hospital docs](docs/hospital.md) lay out the details for two resources,
`Doctor` and `Patient`. This repository has some starter code for an app
`hospital`. Just like with the library's starter code, right now both
`Doctor` and `Patient` support successful CRUD on their own.

Let's connect these two resources:

🟢  **Code:**

1. Add `ForeignKey` field to `Patient`
2. Update `DoctorSerializer` to return `patients`

Use the Library Example to complete these steps.

#### NOTE: Providing Migration Defaults - If you make migrations before adding a new Foreign Key

When you add a new `ForeignKey` to `Patient` and then go to make the migration
files, you may find that Django will stop you with a prompt something like this:

```sh
$ python manage.py makemigrations
You are trying to add a non-nullable field 'primary_care_physician' to patient without a default; we can't do that (the database needs something to populate existing rows).
Please select a fix:
 1) Provide a one-off default now (will be set on all existing rows with a null value for this column)
 2) Quit, and let me add a default in models.py
Select an option:
```

As Django is explaining, the migration files won't generate without being provided
some reasonable default for this new field that isn't allowed to be empty. This is
because the migration files don't know the state of our database, so even if it's
empty we still need to provide some value for any "existing rows" that don't
already have this field.

We have two options - give Django a value to use for the existing rows in the database, or
go back and modify the field in the models file. We'll choose the first one for right now.

Go ahead and type `1` to select this option and hit `enter` to continue.

Once you do this, you'll be moved on to a message like this:

```sh
Please enter the default value now, as valid Python
The datetime and django.utils.timezone modules are available, so you can do e.g. timezone.now
Type 'exit' to exit this prompt
>>>
```

You might recognize this - it's the Python REPL! The `>>>` prompt is telling us we can
provide Python code to be interpreted. In this case, we want to provide a valid ID
of some other row in the database. Assuming our database is empty, the safest value
to provide would be `1`, since that's the first ID that will ever exist.

Go ahead and type `1` to provide this value and hit `enter` to continue.

Django should spit you back out to the `$` filesystem where you can run your migrations.

> Note: Use your judgement when providing defaults. If your database isn't empty,
> there may be more to consider when picking a good default value.


<br><br>

# Many to Many Relationships


## Library V3 - Many to Many

### Code-Along: Add Many-to-Many Relationships


Time to add a many-to-many relationship to `Book`. 

🟢  **View ERD**

Following the [library documentation](docs/library.md), `Book` should have a field that references who has or is borrowing that books from the library.

`Book` >---< `Borrower`

`Borrowers` can borrow many `Books`, and `Books` can be borrowed by many `Borrowers`!

 - As before, there is already a `Borrower` model that supports full CRUD.

 - In the `Book` model we will add a new field to reference the `Borrower` model.

 - The `borrowers` field will be a `ManyToManyField` that will allow `Book` to store multiple `borrowers`.

- This field might be empty at some point, and that's okay, so the `blank=True`option sets `borrowers` to be optional.

- On the `Borrower` resource, the books borrowed will be referenced with the `related_name` which is `loans`.

🟢  **Code:**

 ```py
 from django.db import models

from .loan import Loan
from .borrower import Borrower

class Book(models.Model):
  title = models.CharField(max_length=100)
  # author = models.CharField(max_length=100)
  author = models.ForeignKey('Author', related_name='books', on_delete=models.CASCADE)
  created_at = models.DateTimeField(auto_now_add=True)
  updated_at = models.DateTimeField(auto_now=True)

  # Many to many borrowers relationship through loans:
  borrowers = models.ManyToManyField(
    Borrower,
    related_name='loans',
    blank=True
  )

  def __str__(self):
    return self.title
 ```

 - In the `BorrowerSerializer` we can reference the `BookSerializer` to be able to see the books a borrower has borrowed.

 [Django Rest Framework Serializer Docs](https://www.django-rest-framework.org/api-guide/relations/)

### SQL Joins

Sometimes, we want a table to keep track of the connections between resources.
For example, a resource called `Loan` could keep track of the ID of the
`Borrower` and the `Book` in our many-to-many relationship. 

 - In relational databases, this kind of resource/table that "joins" the relationship between two other resources is known as a join table. This table could also hold information about the loan, like due date and checkout date.

 - We often talk about building a relationship that goes "through" this join table, which is why you'll often see the keyword `through` when building out join tables in different frameworks.

 - In Django we can accomplish a join table for our `Book` >---< `Borrower` relationship using the current `Loan` model. 

We will need to modify our models so that `Loan` can be used to represent this relationship:

1. Add two `ForeignKeys` to `Loan` (join table)
    - This will keep track of the relationship to the `Book` and `Borrower`
    - Note: We can also give it any other fields we want, like a due date!


🟢  **Code:**

```py
from django.db import models

class Loan(models.Model):
    # define ForeignKey fields for the book ID and borrower ID
    book_id = models.ForeignKey('Book', on_delete=models.CASCADE)
    borrower_id = models.ForeignKey('Borrower', on_delete=models.CASCADE)
    due_date = models.DateField()

    def __str__(self):
        return f"Book #{self.book_id}, Borrower #{self.borrower_id}"
```


2. Update the `borrower` field on `Book` to go "through" the `Loan` table
    - Use `through` to reference the `Loan` model
    - Use `through_fields` to reference the fields on `Loan` that should refer to the `Borrower` and `Book` IDs

```py
from django.db import models
from .loan import Loan  # <- import Loan
from .borrower import Borrower

class Book(models.Model):
  title = models.CharField(max_length=100)
  # author = models.CharField(max_length=100)
  created_at = models.DateTimeField(auto_now_add=True)
  updated_at = models.DateTimeField(auto_now=True)

  # Add a foreign key to the book as `author`:
  author = models.ForeignKey('Author', related_name='books', on_delete=models.CASCADE)

  # Many to many borrowers relationship through loans:
  borrowers = models.ManyToManyField(
    Borrower,
    through=Loan, # <- add Loan
    through_fields=('book_id', 'borrower_id'), #<- add through_fields
    related_name='loans',
    blank=True
  )

  def __str__(self):
    return self.title
```
    
3. Add a `LoanReadSerializer` to display the book and borrower objects

`Book` -|--< `Loan` >--|- `Borrower`

We should be able to create a `Loan` and have that translate over to both
`Book` and `Borrower`, based on the data in the `Loan` we created. We actually
don't end up interacting with the `Book` or `Borrower`, but instead just CRUD on
`Loans`.

```py

from rest_framework import serializers

from .models.book import Book
from .models.author import Author
from .models.borrower import Borrower
from .models.loan import Loan

class BookSerializer(serializers.ModelSerializer):
  class Meta:
    model = Book
    fields = '__all__'

class BookReadSerializer(BookSerializer): # <- inherit from above 
  author = serializers.StringRelatedField() 

class AuthorSerializer(serializers.ModelSerializer):
  # add the books field and pass it through the BookSerializer
  books = BookSerializer(many=True, required=False, read_only=True)
  class Meta:
    model = Author
    fields = ('first_name', 'last_name', 'books', 'id')

class BorrowerSerializer(serializers.ModelSerializer):
  class Meta:
    model = Borrower
    fields = '__all__'

class BorrowerLoanSerializer(BorrowerSerializer):
  loans = BookReadSerializer(read_only=True, many=True)

class LoanSerializer(serializers.ModelSerializer):
    class Meta:
        model = Loan
        fields = '__all__'

class LoanReadSerializer(serializers.ModelSerializer):
  book = BookReadSerializer(source='book_id')
  borrower = BorrowerSerializer(source='borrower_id')
  class Meta:
      model = Loan
      fields = ('book', 'borrower', 'due_date', 'id')
```

<br><br>


## Hospital V3 : Many to Many

Time to plug in a many-to-many relationship between `Doctor` and their
`Patient`s. The [hospital documentation](docs/hospital.md) shows us that we
should have a resource called `Appointment` to join `Doctor` and `Patient`.

🟢  **View ERD**

### Lab: Join Patients and Doctors through `Appointment`

We need our `Appointment` before we can use it to "join" `Doctor` and `Patient`.

Use the current files for doctors and patients to help guide you.

1. Create an `Appointment` model
3. Add a new serializer class for `Appointment`s
2. Add CRUD views for `Appointment`s
4. Test CRUD on `Appointment`
5. Connect `Patient` and `Doctor` using the `Appointment` table

## Bonus Lab: Cafeteria App

[Django Relationships Practice Lab](https://git.generalassemb.ly/ga-wdi-boston/django-relationships-practice)

## Additional Resources

- [Django Models](https://docs.djangoproject.com/en/3.0/topics/db/models/)
- [Django One-to-Many Relationships](https://books.agiliq.com/projects/django-orm-cookbook/en/latest/one_to_many.html)

## [License](LICENSE)

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
