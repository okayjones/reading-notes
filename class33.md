# JSON Web Tokens

[Source: Introduction to JSON Web Tokens](https://jwt.io/introduction/)  

## Overview

`JWT` Open standard for transmitting info b/w parties as a JSON object. Can be signed with a secret or public/private key pair.

### Use Cases

- `Auth` - After login, subsequent requests include JWT. provides access to routes, services, resources permitted with token.
- `Info exchange` - Use for secure transfer, since keys can be used to sign request.

### Structure

`header`.`payload`.`signature`

- 3 items separated by `.`
- Each is a JSON object that has been Base64URL encoded.

header example

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

payload example

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

> Contains "claims" which are statements about the entity (user) and additional data. Readable by anyone, don't include private info unless encrypted.

signature example

```bash
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

> Signature ensures the message wasn't changed. If signed with private key, also ensures the sender is known.

#### Debugger

[jwt.io Debugger](https://jwt.io/#debugger-io) can be used to decode, verify, and generate JWTs.

### Behavior

- User logs in and JWT returned
- When user accesses a protected route or resource, JWT sent
- JWT typically sent in Auth header wtih Bearer schema

![jwt flow](https://cdn2.auth0.com/docs/media/articles/api-auth/client-credentials-grant.png)

- JWT best for HTML and HTTP because of small size
- Ideal because JSON maps to objects
- Internet scale usage today

## In Django

[Source: How to Use JWT Authentication with Django REST Framework](https://simpleisbetterthancomplex.com/tutorial/2018/12/19/how-to-use-jwt-authentication-with-django-rest-framework.html)

### Access and Refresh Token

- `Access` short lives and expires within 5m or so
- `Refresh` expires in 24hrs, similiar to an auth session

header

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

payload

```json
{
  "token_type": "access",
  "exp": 1543828431,
  "jti": "7f5997b7150d46579dc2b49167097e7b",
  "user_id": 1
}
```

### Install & Setup

```bash
pip install djangorestframework_simplejwt
```

#### settings.py

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ],
}
```

#### project/urls.py

```python
from django.urls import path
from rest_framework_simplejwt import views as jwt_views

urlpatterns = [
    # Your URLs...
    path('api/token/', jwt_views.TokenObtainPairView.as_view(), name='token_obtain_pair'),
    path('api/token/refresh/', jwt_views.TokenRefreshView.as_view(), name='token_refresh'),
]
```

### Example

#### views.py

```python
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework.permissions import IsAuthenticated


class HelloView(APIView):
    permission_classes = (IsAuthenticated,)

    def get(self, request):
        content = {'message': 'Hello, World!'}
        return Response(content)
```

#### urls.py

```python
from django.urls import path
from myapi.core import views

urlpatterns = [
    path('hello/', views.HelloView.as_view(), name='hello'),
]
```

#### Get Token

with HTTPie (or curl, or use the UI)

```bash
http post http://127.0.0.1:8000/api/token/ username=vitor password=123
```

```json
{
    "access": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNTQ1MjI0MjU5LCJqdGkiOiIyYmQ1NjI3MmIzYjI0YjNmOGI1MjJlNThjMzdjMTdlMSIsInVzZXJfaWQiOjF9.D92tTuVi_YcNkJtiLGHtcn6tBcxLCBxz9FKD3qzhUg8",
    "refresh": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTU0NTMxMDM1OSwianRpIjoiMjk2ZDc1ZDA3Nzc2NDE0ZjkxYjhiOTY4MzI4NGRmOTUiLCJ1c2VyX2lkIjoxfQ.rA-mnGRg71NEW_ga0sJoaMODS5ABjE5HnxJDb0F8xAo"
}
```

#### Token Management

- Store `access` and `refresh` tokens client side (localstorage)
- Include access token in header of all requests
- After 5m, access token invalid
- Call `api/token/refresh` with refresh token
- Returns new access token
