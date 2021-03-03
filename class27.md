# Django: Models and Admin

## Django Models

[Source: Django Tutorial Part 3: Using models](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)

Django access/manage data trough python objects or "models". The model is the definition based on your underlying database.

In this framework, you don't interact with your database directly-- only with the model.

### Designing models

Define model for every "object". May also want separate models for selections, like drop-downs. Think about the relationships between objects (1:1, 1:n, n:n, foreignkey)

Create a UML to help with this.

![uml for library app](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models/local_library_model_uml.png)

### Model primer

- Models are defined in an apps models.py file.
- Models are subclasses of django.db.models.Model
- Include fields, methods, metadata

```python
from django.db import models

class MyModelName(models.Model):
    """A typical class defining a model, derived from the Model class."""

    # Fields
    my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
    ...

    # Metadata
    class Meta:
        ordering = ['-my_field_name']

    # Methods
    def get_absolute_url(self):
        """Returns the url to access a particular instance of MyModelName."""
        return reverse('model-detail-view', args=[str(self.id)])

    def __str__(self):
        """String for representing the MyModelName object (in Admin site etc.)."""
        return self.my_field_name
```

#### Fields

- Each field represents a column of data in the db table
- Each db row will consist of one of each field value
- Field types are assigned with classes (models.CharField)

```python
my_field_name = models.CharField(max_length=20, help_text='Enter field documentation')
```

[field types and arguments](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)

#### Metadata

- Declare model-level metadata in the class
- Common for defining default ordering

```python
class Meta:
#   ordering = ['-example_field']
  ordering = ['title', '-pubdate']

```

[metadata options](https://docs.djangoproject.com/en/3.1/ref/models/options/)

#### Methods

- Models should have a dunder str

```python
def __str__(self):
  return self.field_name
```

- `get_absolute_url()` also common
  - returns url to access individual model records

```python
def get_absolute_url(self):
  """Returns the url to access a particular instance of the model."""
  return reverse('model-detail-view', args=[str(self.id)])
#   e.g. /myapplication/mymodelname/2
```

### Model management

Use for CRUD operations

#### Create

- Create by defining instance and calling save()
- If you don't declare a primary key, given one automatically with id

```python
# Create a new record using the model's constructor.
record = MyModelName(my_field_name="Instance #1")

# Save the object into the database.
record.save()
```

#### Modify

- Access field with dot syntax
- Change value and save

```python
# Access model field values using Python attributes.
print(record.id) # should return 1 for the first record.
print(record.my_field_name) # should print 'Instance #1'

# Change record by modifying the fields, then calling save().
record.my_field_name = "New Instance Name"
record.save()
```

#### Searching

- Search with .objects attribute

```python
all_books = Book.objects.all()

wild_books = Book.objects.filter(title__contains='wild')
#  type of match defined here. note double underscore.
#  Format: field_name__match_type
number_wild_books = wild_books.count()
```

[Making queries](Making queries)

### Re-run DB migrations

- After models are generated, run db migrations

```bash
python3 manage.py makemigrations
python3 manage.py migrate
```

## Admin

[Source: Django Tutorial Part 4: Django admin site](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Admin_site)

- Admin app allows you to CRUD records
- Helps you test your model to see if it's right
- May help with production management (depending on use case)

### Registering models

- admin.py

```python
from django.contrib import admin

from .models import Author, Genre, Book, BookInstance

admin.site.register(Book)
admin.site.register(Author)
admin.site.register(Genre)
admin.site.register(BookInstance)
```

### Creating a superuser

```bash
python3 manage.py createsuperuser
python3 manage.py runserver

```

### Access / Use

- Default: [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)
- enter superuser userid and password
- interact with the UI
