# Game of Greed 2

[Source: Python Scope & the LEGB Rule: Resolving Names in Your Code](https://realpython.com/python-scope-legb-rule/)

## Scope

- `Global` - Avaliable to all code
- `Local` - Avaliable to other code within same scope (or nested within it)

## LEGB Scope

- `Local` or `Function` -- within a code block, function, or lambda. Only visibke within the function code.
- `Enclosing` or `Nonlocal` -- Nested functions only. Scope of the outter function.
- `Global` - Top most scope in program, script or module.
- `Build-in` - Created when you run a script or session. Automatically loaded.

## Modifying Scope

Keywords can be used to change the scope of a variable

### Global

- When you try to assign a global name in a function, you create a new local name in the function scope
- `global` keyword can be used to modify behavior

```python
>>> counter = 0  # A global name
>>> def update_counter():
...     counter = counter + 1  # Fail trying to update counter
...
>>> update_counter()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 2, in update_counter
UnboundLocalError: local variable 'counter' referenced before assignment
```

```python
>>> counter = 0  # A global name
>>> def update_counter():
...     global counter  # Declare counter as global
...     counter = counter + 1  # Successfully update the counter
...
>>> update_counter()
>>> counter
1
```

> Generally bad practice. Instead, try using a local variable by passing the global in as an argument.

### Nonlocal

- Similiar problem as above, but occurs in nested local scopes rather than global/local scope.
- Nonlocal names can't be access from inner functions
- Modify them with `nonlocal` keyword

```python
>>> def func():
...     var = 100  # A nonlocal variable
...     def nested():
...         nonlocal var  # Declare var as nonlocal
...         var += 100
...
...     nested()
...     print(var)
...
>>> func()
200
```
