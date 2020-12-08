# Testing and Modules

## Testing

[Source: In Tests We Trust](https://code.likeagirl.io/in-tests-we-trust-tdd-with-python-af69f47e6932)

- `Unit Tests` pieces ofcode to test the input, output, and behavior of your primary code
- `Test Driven Development` strategy to think and write tests first

### Writing a Unit test

- Identify smallest test you can do against a function
- Descriptive test name; your "alive" documentation
- Test file name should match module name (e.g. gender.py and test_gender.py)
- Separate test files in own folder
- AAA Structure: Arrange, Act, Assert
  - Arrange: organize the input
  - Act: execute code being tested
  - Assert: validate expected output

> Example: Identify gender by name

```python
def test_should_return_female_when_the_name_is_from_female_gender():
    detector = GenderDetector()
    expected_gender = detector.run(‘Ana’)
    assert expected_gender == ‘female’
```

### Test Driven Development

> Checks design of the software first

1. Write test and make it fail (nothing to test yet!)
2. Write the feature and make the test pass
3. Refactor to inprove code quality

## Recursion

[Source: Recursion](https://www.geeksforgeeks.org/recursion/)

- `Recursion` - Process of a function calling itself directly or indirectly
- `Base case` - Provided solution to the simplest case. Continue solving until base case is reached.
- `Stack overflow` - Error occurs when base case never reached or not defined
- `Direct` - Function explicitly calls itself
- `Indrect` - Function calls another function (which may call another) before it is called again
- `Tailed` - When function calls itself as the last thing in the function
- `Disadvantages` - Larger space and time requirements
- `Advantages` - Simple, clean, best for tree traversals

> Example: Sum of n numbers

Non-recursive

  ```none
  f(n) = 1 + 2 + 3 +……..+ n
  ```

Recursive

```none
f(n) = 1                    n=1

f(n) = n + f(n-1)           n>1
```

Base case

```none
int fact(int n)
{
    if (n < = 1) // base case
        return 1;
    else    
        return n*fact(n-1);    
}
```

> Python example

```python
# A Python 3 program to
# demonstrate working of
# recursion
 
 
def printFun(test):
 
    if (test < 1):
        return
    else:
 
        print(test, end=" ")
        printFun(test-1)  # statement 2
        print(test, end=" ")
        return
 
# Driver Code
test = 3
printFun(test)
 
# This code is contributed by
# Smitha Dinesh Semwal
```

![recursion example](https://media.geeksforgeeks.org/wp-content/cdn-uploads/recursion.jpg)
