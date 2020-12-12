# FileIO & Exceptions

## Files

[Source: Read & Write Files in Python](https://realpython.com/read-write-files-python/)

FIles consist of:

1. `Header` - metadata (e.g. name, type)
2. `Data` - content
3. `End of File` - character at end

### Line Endings

`\r\n` for windows

```none
Pug\r\n
Jack Russell Terrier\r\n
```

`\r` for unix or mac

```none
Pug\r
\n
Jack Russell Terrier\r
\n
```

### Character Encodings

- `ASCII`
- `Unicode`

### Opening / Closing a File

Open

```python
file = open('dog_breeds.txt')
```

Close

- Important even if an error occurs

```python
try:
    # Further file processing goes here
finally:
    reader.close()
```

> Using with will close automatically

```python
with open('dog_breeds.txt', 'r') as reader:
    # Further file processing goes here
```

#### Options

- `r` read (default)
- `w` write
- `rb` or `wb` binary mode

### File Types

- Text (.txt -> returns `_io.TextIOWrapper` type)
- Buffered Binary (.txt --> `BufferedReader` or `BufferedWriter`)
- Raw (.txt -> `_io.FileIO`)

### Reading and Writing Open Files

- `read(size=-1)` - Number of bytes, or -1 for all
- `readline(size=-1)` - Number of character, or -1 for all
- `readlines()` - Remaining lines returned as a list

### Iterating Over File Lines

readline()

```python
 with open('dog_breeds.txt', 'r') as reader:
     # Read and print the entire file line by line
     line = reader.readline()
     while line != '':  # The EOF char is an empty string
         print(line, end='')
         line = reader.readline()
```

readlines()

```python
 with open('dog_breeds.txt', 'r') as reader:
     for line in reader.readlines():
         print(line, end='')
```

file

```python
 with open('dog_breeds.txt', 'r') as reader:
     # Read and print the entire file line by line
     for line in reader:
         print(line, end='')
```

### Writing files

- `.write(string)`
- `.writelines(seq)`

```python
    # Note: readlines doesn't trim the line endings
    dog_breeds = reader.readlines()

with open('dog_breeds_reversed.txt', 'w') as writer:
    # Alternatively you could use
    # writer.writelines(reversed(dog_breeds))

    # Write the dog breeds to the file in reversed order
    for breed in reversed(dog_breeds):
        writer.write(breed)
```

### Appending

```python
with open('dog_breeds.txt', 'a') as a_writer:
    a_writer.write('\nBeagle')
```

### Working with two files

```python
d_path = 'dog_breeds.txt'
d_r_path = 'dog_breeds_reversed.txt'
with open(d_path, 'r') as reader, open(d_r_path, 'w') as writer:
    dog_breeds = reader.readlines()
    writer.writelines(reversed(dog_breeds))
```

### Resources

[Reading and Writing CSV Files in Python](https://realpython.com/python-csv/)
[Working With JSON Data in Python](https://realpython.com/python-json/)

## Exceptions

[Source: Python Exceptions: An Introduction](https://realpython.com/python-exceptions/)

### vs. Syntax Errors

- Syntax error: parse detecs incorrect statement
- Exceptions: Syntax is correct, but results in an error

### Raising an exceptions

Throw an exceptions (standard or custom) if certain condition occurs:

```python
x = 10
if x > 5:
    raise Exception('x should not exceed 5. The value of x was: {}'.format(x))
```

Results in:

```python
Traceback (most recent call last):
  File "<input>", line 4, in <module>
Exception: x should not exceed 5. The value of x was: 10
```

### AssertionError

Assert that a specfic condition is met, instead of waiting for failure:

```python
import sys
assert ('linux' in sys.platform), "This code runs on Linux only."
```

Results in:

```python
Traceback (most recent call last):
  File "<input>", line 2, in <module>
AssertionError: This code runs on Linux only.
```

### Try and except

Try some code, and catch an exception if it occurs

```python
try:
    linux_interaction()
except AssertionError as error:
    print(error)
    print('The linux_interaction() function was not executed')
```

```python
try:
    with open('file.log') as file:
        read_data = file.read()
except:
    print('Could not open file.log')
```

- Try clause executed up until first exception
- Respond inside except or the exception handler
- Cnticipate multiple exceptions and responses
- Avoid using bare except

### Else

Runs in absense of error:

```python
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    print('Executing the else clause.')
```

```python
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
```

### Finally

Use finally to clean up after code execution

```python
try:
    linux_interaction()
except AssertionError as error:
    print(error)
else:
    try:
        with open('file.log') as file:
            read_data = file.read()
    except FileNotFoundError as fnf_error:
        print(fnf_error)
finally:
    print('Cleaning up, irrespective of any exceptions.')
```

### Summary

- `raise` to throw an exception at any time
- `assert` enables to verify certain condition is
- `try` all statements executed until an exception
- `except` to catch / handle exception(s) in try clause
- `else` run only when no exceptions in try clause
- `finally` to execute code to always run regardless of exception
