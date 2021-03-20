# DRF Permissions

[Source: https://www.django-rest-framework.org/api-guide/permissions/](https://www.django-rest-framework.org/api-guide/permissions/)

- Determines access combined with auth and throttling
- User permissions checked at beginning on view
- Permissions defines as classes
- Return 403 Forbidden or 401 Unauthorized

## Object Level Permissions

- Typically a model instance
- Used with `.get_object()` is called
- To use, explicitly call `.check_object_permissions(request, obj)`
- Raises `PermissionDenied` or `NotAuthenticated`
- Generic view doesn't filter automatically, you'll need to filter query set

```python
def get_object(self):
    obj = get_object_or_404(self.get_queryset(), pk=self.kwargs["pk"])
    self.check_object_permissions(self.request, obj)
    return obj
```

## Setting Permissions Policy

Setting globally

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ]

    # default
    # 'rest_framework.permissions.AllowAny',
}
```

At the per-view level with `APIView`

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.views import APIView

class ExampleView(APIView):
    permission_classes = [IsAuthenticated]

    def get(self, request, format=None):
        content = {
            'status': 'request was permitted'
        }
        return Response(content)
```

Using `@api_view` decorator

```python
from rest_framework.decorators import api_view, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response

@api_view(['GET'])
@permission_classes([IsAuthenticated])
def example_view(request, format=None):
    content = {
        'status': 'request was permitted'
    }
    return Response(content)
```

## API Reference Notes

- `AllowAny` - unrestricted, ignores auth
- `IsAuthenticated` - deny to all unauth
- `IsAdminUser`
- `IsAuthenticatedOrReadOnly`
- `DjangoModelPermissions` - Auth and model perms
- `DjangoModelPermissionsOrAnonReadOnly` - Un-auth gets readonly
- `DjangoObjectPermissions`
- `Custom permissions`

## Example

Check IP is not on blocklist

```python
from rest_framework import permissions

class BlocklistPermission(permissions.BasePermission):
    """
    Global permission check for blocked IPs.
    """

    def has_permission(self, request, view):
        ip_addr = request.META['REMOTE_ADDR']
        blocked = Blocklist.objects.filter(ip_addr=ip_addr).exists()
        return not blocked
```

## Other Resources

- [Classy Django REST Framework](http://www.cdrf.co/)
- [Generic Views](https://www.django-rest-framework.org/api-guide/generic-views/)
