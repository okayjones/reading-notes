# Django REST & Docker

## Django REST

[Source: Library Website and API](https://djangoforapis.com/library-website-and-api/)

- REST framework work alongside Django project
- Must be added to project after Django installed and configured
- Creates web APIs rather than webpages
- `wsgi.py`  Web Server Gateway Interface
- `asgi.py` Asynchronous Server Gateway Interface, new in 3.0+

> After creating your app (template, view, url, model), we can add a REST API

### Install Django REST Framework

```bash
poetry install djangorestframework~=3.11.0
```

### Configure Django project

```python
# config/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # 3rd party
    'rest_framework', # new

    # Local
    'books',
]
```

### Create dedicated API app

> Includes urls, views, serializers

```bash
python manage.py startapp api
```

```python
# config/settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    # 3rd party
    'rest_framework',

    # Local
    'books',
    'api', # new
]
```

#### Urls

```python
# config/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')), # new
    path('', include('books.urls')),
]
```

```bash
touch api/urls.py
```

```python
# api/urls.py
from django.urls import path
from .views import BookAPIView

urlpatterns = [
    path('', BookAPIView.as_view()),
]
```

#### Views

```python
# api/views.py
from rest_framework import generics

from books.models import Book
from .serializers import BookSerializer


class BookAPIView(generics.ListAPIView):
    queryset = Book.objects.all()
    serializer_class = BookSerializer
```

### Serializers

> translated data to easy to consume format, like JSON

```bash
touch api/serializers.py
```

```python
# api/serializers.py
from rest_framework import serializers

from books.models import Book


class BookSerializer(serializers.ModelSerializer):
    class Meta:
        model = Book
        fields = ('title', 'subtitle', 'author', 'isbn')
```

### cURL

> Test your endpoint by curling it

```bash
python manage.py runserver
curl http://127.0.0.1:8000/api/

# [  
#    {  
#       "title":"Django for Beginners",
#       "subtitle":"Build websites with Python and Django",
#       "author":"William S. Vincent",
#       "isbn":"978-198317266"
#    }
# ]
```

### Browsable API

> Similar to swagger

Navigate to `http://127.0.0.1:8000/api/`

## Docker

[Source: A Beginner's Guide to Docker](https://wsvincent.com/beginners-guide-to-docker/)

Docker containerizes the development environemnt so software can be easy deployed with the correct setup.

### Containers vs Virtual Env

- Virtual environments isolate python software packages locally
  - Allows you to run projects on different versions of python on the same machine
- Containers provide a virtual environment for the entire machine. Supports databases and other services.

### Docker Setup

1. Download desktop app [here](https://www.docker.com/get-started)
2. Confirm `docker --version`
3. Browse contianers/images in Docker app or in CLI

### Images & Containers

- Image is a snapshot of what a project contains (receipe)
- Container is a running instance of that image (screenshot of receipe)
- Create images with a `Dockerfile` (directions)
- Run the container with `docker-compose.yml` (the cake)

### Images

#### Create an image

```bash
mkdir code && cd code
mkdir python3.7 && cd python3.7
touch Dockerfile
```

```text
# Dockerfile
FROM python:3.7-alpine

Sending build context to Docker daemon  2.048kB
Step 1/1 : FROM python:3.7-alpine
3.7-alpine: Pulling from library/python
c9b1b535fdd9: Pull complete
2cc5ad85d9ab: Pull complete
53a2fca3c2ea: Pull complete
30fce49de8b1: Pull complete
49bcb9571cc7: Pull complete
Digest: sha256:7655eea15dfd981eeb7d243af21e8561e967709caba938d50b136cdb992f3546
Status: Downloaded newer image for python:3.7-alpine
 ---> b2c276538711
Successfully built b2c276538711
```

#### Build image

```bash
docker image build .
```

- Outputs image hash

#### Image Layers

- Images made of layers
- Base generally a "flavor" of Linux (alpine)
- Each image layer is immutable
- Docker caches `Dockerfile` steps to speed up builds

### Containers

-`docker-compose.yml` container instructions

### Django Example

> Note: Example uses pip/pipenv, not poetry

In the root project folder:

```bash
touch Dockerfile
touch docker-compose.yml
```

Update Dockerfile:

```text
# Dockerfile

# Python version
FROM python:3.7-alpine

# Set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Set work directory
WORKDIR /code

# Install dependencies
COPY Pipfile Pipfile.lock /code/
RUN pip install pipenv && pipenv install --system

# Copy project
COPY . /code/
```

Update docker-compose.yml:

```text
# docker-compose.yml
version: '3.7'

services:
  web:
    build: .
    command: python /code/manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - 8000:8000
```

Build and run container:

```bash
docker-compose up --build
```

View it:

[http://127.0.0.1:8000/](http://127.0.0.1:8000/)

### CLI

- `docker --version`
- `docker-compose --version`
- `docker run image-name` (downloads image, runs thhe container)
- `docker info`
- `docker image ls` (current image)
- `docker container ls -la` (all containers)
