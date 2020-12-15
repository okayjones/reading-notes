# Class 04

## Classes and Objects

[Source: Classes and Objects](https://www.learnpython.org/en/Classes_and_Objects)

- `Objects` - Encapsulation of variables and functions
- `Classes` - Template for objects

```python
class MyClass:
    variable = "blah"

    def function(self):
        print("This is a message inside the class.")

myobjectx = MyClass()

```

- Access object variable: `myobjectx.variable`
- Access object function: `myobjectx.function()`

## Thinking Recursively

[Source: Thinking Recursively](https://realpython.com/python-thinking-recursively/)

- Divide problem into subproblem
- Call itself to solve them

### Structure

- Base case
- Recursive case

### Example

n!

```python
def factorial_recursive(n):
    # Base case: 1! = 1
    if n == 1:
        return 1

    # Recursive case: n! = n * (n-1)!
    else:
        return n * factorial_recursive(n-1)
```

### Stack

- Each recursive call adds to call stack
- Added until hit base case
- Solve base case and worth back through stack

![stack gif](https://files.realpython.com/media/stack.9c4ba62929cf.gif)

### State

- Each calls runs in own execution context
- State needs to be maintained through calls

Options

1. Thread state through each call

   ```python
    def sum_recursive(current_number, accumulated_sum):
    # Base case
    # Return the final state
    if current_number == 11:
        return accumulated_sum

    # Recursive case
    # Thread the state through the recursive call
    else:
        return sum_recursive(current_number + 1, accumulated_sum + current_number)
   ```

2. Maintain state inn global scope

   ```python
   # Global mutable state
    current_number = 1
    accumulated_sum = 0


    def sum_recursive():
        global current_number
        global accumulated_sum
        # Base case
        if current_number == 11:
            return accumulated_sum
        # Recursive case
        else:
            accumulated_sum = accumulated_sum + current_number
            current_number = current_number + 1
            return sum_recursive()
     ```

### Data structures

- List
- Tree
- Dict

List Sum Example:

```python
    # Base case
    if input_list == []:
        return 0

    # Recursive case
    # Decompose the original problem into simpler instances of the same problem
    # by making use of the fact that the input is a recursive data structure
    # and can be deﬁned in terms of a smaller version of itself
    else:
        head = input_list[0]
        smaller_list = input_list[1:]
        return head + list_sum_recursive(smaller_list)
```

```python
>>> list_sum_recursive([1, 2, 3])
6
```

## Pytest

[Source: Python Testing with pytest: Fixtures and Coverage](https://www.linuxjournal.com/content/python-testing-pytest-fixtures-and-coverage)

### Fictures

- Allows data to be shared across tests
- Use `pytest.fixture` decorator

Function

```python
def reverse_lines(f):
   return [one_line.rstrip()[::-1] + '\n'
           for one_line in f]
```

Fixture

- provides data to test
- similair to defining global variable
- acts like a variable (not invoked with ())
- executes when test runs

```python
@pytest.fixture
def simple_file():
   return StringIO('\n'.join(['abc', 'def', 'ghi', 'jkl']))
```

- or, decide how often it runs

```python
@pytest.fixture(scope='module')
def simple_file():
   return StringIO('\n'.join(['abc', 'def', 'ghi', 'jkl']))
```

### Coverage

Indicates a reasonable degree of test coverage (cases to test)

- package: `pytest-cov`
- coverage report: `pytest --cov=mydirectory`
- make it  readable: `coverage html`
  - open `htmlcov\index.html`
