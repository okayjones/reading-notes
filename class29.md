# Django User Model & DjangoX

## Django Custom User Model

[Source: https://learndjango.com/tutorials/django-custom-user-model](https://learndjango.com/tutorials/django-custom-user-model)

Django comes wth an OOB authentication model, however for a real-world project a custom model should be used to provide more flexibility.

### User Model

> Don't migrate DB until custom model is setup

- update config/settings.py
  - Add `accounts` to installed apps
  - Set `AUTH_USER_MODEL` to `accounts.CustomUser`
- create a new CustomUser model
  - Add `CustomUser` class extending `AbstractUser` in accounts/models.py
- create new UserCreation and UserChangeForm
  - Create accounts/forms.py
  - Add classes for `CustomUserCreationForm` and `CustomUserChangeForm`
- update the admin
  - accounts/admin.py
  - Add class for `CustomUserAdmin`
- Do `makemigrations` and `migrate`
- Create a superuser

### Templates/Views/URLS

- Set redirect links for login/logout
  - config/settings.py
- In templates, control based on auth
  - `{% if user.is_authenticated %}`

> Go [here](https://learndjango.com/tutorials/django-custom-user-model) for complete walkthrough

## DjangoX

[Source: https://github.com/wsvincent/djangox](https://github.com/wsvincent/djangox)

DjangoX is helpful Django starter project, including:

- Django 3.1 & Python 3.8
- Django-allauth
- Staticfiles via Whitenoise
- Styling with Bootstrap v4
- Debugging with django-debug-toolbar
- Forms with django-crispy-forms
