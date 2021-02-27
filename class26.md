# Django

[Source: Getting started with Django](https://www.djangoproject.com/start/)

[Source: Django introduction](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction)

[Source: How Django Works Behind the Scenes](https://wsvincent.com/how-django-works-behind-the-scenes/)

> Django is a Python web framework, aiming to be a complete "out of box" solution. Django has a component-based "shared nothing" architecture.

Django's "Model View Template" (MVT) achritecture.
![Django diagram](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction/basic-django.png)

## Installation

- `python -m pip install Django`
- Can also be installed within poetry

## Intro

### Object-relational mapper

- Data models defined in python
- Provides a dynamic database-access API
- SQL can still be used

```python
class Band(models.Model):
    """A model of a rock band."""
    name = models.CharField(max_length=200)
    can_rock = models.BooleanField(default=True)


class Member(models.Model):
    """A model of a rock band member."""
    name = models.CharField("Member's name", max_length=200)
    instrument = models.CharField(choices=(
            ('g', "Guitar"),
            ('b', "Bass"),
            ('d', "Drums"),
        ),
        max_length=1
    )
    band = models.ForeignKey("Band")
```

### URLS and views

- Clean URL scheme
- Setup in URLconf
- Acts as a table of contents
- Maps URL patterns and views

URLS

```python
from django.urls import path

from . import views

urlpatterns = [
    path('bands/', views.band_listing, name='band-list'),
    path('bands/<int:band_id>/', views.band_detail, name='band-detail'),
    path('bands/search/', views.band_search, name='band-search'),
]
```

Views

```python
from django.shortcuts import render

def band_listing(request):
    """A view of all bands."""
    bands = models.Band.objects.all()
    return render(request, 'bands/band_listing.html', {'bands': bands})
```

### Templates

- Templating language for HTML

```html
<html>
  <head>
    <title>Band Listing</title>
  </head>
  <body>
    <h1>All Bands</h1>
    <ul>
    {% for band in bands %}
      <li>
        <h2><a href="{{ band.get_absolute_url }}">{{ band.name }}</a></h2>
        {% if band.can_rock %}<p>This band can rock!</p>{% endif %}
      </li>
    {% endfor %}
    </ul>
  </body>
</html>
```

### Forms

- Renders forms as HTML
- Validated user-submitted data
- Coverts data to native python types

```python
from django import forms

class BandContactForm(forms.Form):
    subject = forms.CharField(max_length=100)
    message = forms.CharField()
    sender = forms.EmailField()
    cc_myself = forms.BooleanField(required=False)
```

### Auth

- Secur auth system

```python
from django.contrib.auth.decorators import login_required
from django.shortcuts import render

@login_required
def my_protected_view(request):
    """A view that can only be accessed by logged-in users"""
    return render(request, 'protected.html', {'current_user': request.user})
```

### Admin

- Automatic admin interface

```python
from django.contrib import admin
from bands.models import Band, Member

class MemberAdmin(admin.ModelAdmin):
    """Customize the look of the auto-generated admin for the Member model"""
    list_display = ('name', 'instrument')
    list_filter = ('band',)

admin.site.register(Band)  # Use the default options
admin.site.register(Member, MemberAdmin)  # Use the customized options
```

### Internationalization

- Full support for translation
- Locale-specific formatting

```python
from django.shortcuts import render
from django.utils.translation import gettext

def homepage(request):
    """
    Shows the homepage with a welcome message that is translated in the
    user's language.
    """
    message = gettext('Welcome to our site!')
    return render(request, 'homepage.html', {'message': message})
```

```html
{% load i18n %}
<html>
  <head>
    <title>{% trans 'Homepage - Hall of Fame' %}</title>
  </head>
  <body>
    {# Translated in the view: #}
    <h1>{{ message }}</h1>
    <p>
      {% blocktrans count member_count=bands.count %}
      Here is the only band in the hall of fame:
      {% plural %}
      Here are all the {{ member_count }} bands in the hall of fame:
      {% endblocktrans %}
    </p>
    <ul>
    {% for band in bands %}
      <li>
        <h2><a href="{{ band.get_absolute_url }}">{{ band.name }}</a></h2>
        {% if band.can_rock %}<p>{% trans 'This band can rock!' %}</p>{% endif %}
      </li>
    {% endfor %}
    </ul>
  </body>
</html>
```

### Security

Protects against: clickjacking, cross-site scripting, cross-site request forgery, sql inhection, remote code execution

## How does Django operate?

- `Django Software Foundation` - Non-profit setup to promote, support, and advance Django
  - Contrast with corporate sponsor like React/Angular
- Technical team - Core team of volunteers
