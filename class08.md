# Game of Greed 3

## List Comprehensions

[Source: List Comprehensions in Python](https://www.pythonforbeginners.com/basics/list-comprehensions-in-python)

Allows you to easily iterate over an existing list and create a new list based on some criteria.

Without List Comprehensions

```python
new_list = []
for i in old_list:
    if filter(i):
        new_list.append(expressions(i))
```

With List Comprehension

```python
new_list = [expression(i) for i in old_list if filter(i)]
```

### Syntax

Old

```python
[ expression for item in list if conditional ]
```

New

```python
[ expression for item in list if conditional ]
```

### Examples

#### Square a list

```python
# You can either use loops:
squares = []

for x in range(10):
    squares.append(x**2)
 
print squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Or you can use list comprehensions to get the same result:
squares = [x**2 for x in range(10)]

print squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

#### Multiply a list

```python
list1 = [3,4,5]
 
multiplied = [item*3 for item in list1] 
 
print multiplied 
[9,12,15]
```

#### Parsing a file

```python
# Then create the filter by using list comprehension:

fh = open("test.txt", "r")

result = [i for i in fh if "line3" in i]

print result
# Output: [‘this is line3‘]
```

#### Using in a function

Function to double something

```python
# Create a function and name it double:
def double(x):
  return x*2

# If you now just print that function with a value in it, it should look like this:
>>> print double(10)
20
```

Apply to list

```python
>>> [double(x) for x in range(10)]

print double
[0, 2, 4, 6, 8, 10, 12, 14, 16, 18]

# You can put in conditions:

>>> [double(x) for x in range(10) if x%2==0]
[0, 4, 8, 12, 16]

# You can add more arguments:

>>> [x+y for x in [10,30,50] for y in [20,40,60]]
[30, 50, 70, 50, 70, 90, 70, 90, 110]
```
