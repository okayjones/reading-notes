# Automation

## RegEx

[Source: Python Regular Expressions Tutorial](https://www.datacamp.com/community/tutorials/python-regular-expression-tutorial)

### Basic matching

1. import re
2. `re.match(pattern, sequence)`
   - pattern is the raw string literal
   - interpreted exactly as is  
3. Returns match object or none

```python
import re

pattern = r"Cookie"
sequence = "Cookie"
if re.match(pattern, sequence):
    print("Match!")
else: print("Not a match!")
```

### Wildcards

Special characters that are not matched but provide additional functionality

```python
# '.' matches any character
re.search(r'Co.k.e', 'Cookie').group()
```

| Character(s) | What it does |  |  |  |
|-|-|-|-|-|
| . | A period. Matches any single character except the newline character. |  |  |  |
| ^ | A caret. Matches a pattern at the start of the string. |  |  |  |
| \A | Uppercase A. Matches only at the start of the string. |  |  |  |
| $ | Dollar sign. Matches the end of the string. |  |  |  |
| \Z | Uppercase Z. Matches only at the end of the string. |  |  |  |
| [ ] | Matches the set of characters you specify within it. |  |  |  |
| \ | ∙ If the character following the backslash is a recognized escape character, then the special meaning of the term is taken. ∙ Else the backslash () is treated like any other character and passed through. ∙ It can be used in front of all the metacharacters to remove their special meaning. |  |  |  |
| \w | Lowercase w. Matches any single letter, digit, or underscore. |  |  |  |
| \W | Uppercase W. Matches any character not part of \w (lowercase w). |  |  |  |
| \s | Lowercase s. Matches a single whitespace character like: space, newline, tab, return. |  |  |  |
| \S | Uppercase S. Matches any character not part of \s (lowercase s). |  |  |  |
| \d | Lowercase d. Matches decimal digit 0-9. |  |  |  |
| \D | Uppercase D. Matches any character that is not a decimal digit. |  |  |  |
| \t | Lowercase t. Matches tab. |  |  |  |
| \n | Lowercase n. Matches newline. |  |  |  |
| \r | Lowercase r. Matches return. |  |  |  |
| \b | Lowercase b. Matches only the beginning or end of the word. |  |  |  |
| + | Checks if the preceding character appears one or more times. |  |  |  |
| * | Checks if the preceding character appears zero or more times. |  |  |  |
| ? | ∙ Checks if the preceding character appears exactly zero or one time. ∙ Specifies a non-greedy version of +, * |  |  |  |
| { } | Checks for an explicit number of times. |  |  |  |
| ( ) | Creates a group when performing matches. |  |  |  |
| < > | Creates a named group when performing matches. |  |  |  |

### Grouping

- Select part of matching text
- Use `( )` to form groups in the expression
- Doesn't impact match
- Use `<>` for named groups

```python
statement = 'Please contact us at: support@datacamp.com'
match = re.search(r'([\w\.-]+)@([\w\.-]+)', statement)
if statement:
  print("Email address:", match.group()) # The whole matched text
  print("Username:", match.group(1)) # The username (group 1)
  print("Host:", match.group(2)) # The host (group 2)

statement = 'Please contact us at: support@datacamp.com'
match = re.search(r'(?P<email>(?P<username>[\w\.-]+)@(?P<host>[\w\.-]+))', statement)
if statement:
  print("Email address:", match.group('email'))
  print("Username:", match.group('username'))
  print("Host:", match.group('host'))
```

### Greedy / Non-Greedy

- `Greedy` - matches as much of the string as possible (default)
- `None-greedy` - matches as little as possible using `?`

```python
pattern = "cookie"
sequence = "Cake and cookie"
heading  = r'<h1>TITLE</h1>'

# greedy
re.match(r'<.*>', heading).group()
# '<h1>TITLE</h1>'

# non-greedy
re.match(r'<.*?>', heading).group()
# '<h1>'
```

### Helpful functions

- `search()` - returns first match
- `group()` - returned the matches string
- `match()` - match at beginning of the string
- `findall(pattern, string, flags=0)` - find all, return as string
- `finditer(string, [position, end_position])` - find all, return match obj iter (stores extra)
- `compile(pattern, flags=0)` - create regex object (to reuse instead of string)
- `sub(pattern, repl, string, count=0, flags=0)` - substitute
- `subn(pattern, repl, string, count=0)` - substitute and returns tuple (new, # of matches)
- `split(string, [maxsplit = 0])` - split at match, return list
- `start()` - Returns starting index of match
- `end()` - Returns ending index of match
- `span()` Returns tuple (start, end)

### Flags

Additional expression behavior to specify

- `re.IGNORECASE` (I)
- `re.DOTALL` (S)
- `re.MULTILINE` (M)
- `VERBOSE` (X)

## shutil

[Source: shutil](https://pymotw.com/3/shutil/)

High-leve file operations

### Copying files

- `copyfile()` - opens the file to copy

    ```python
    import glob
    import shutil

    print('BEFORE:', glob.glob('shutil_copyfile.*'))
    # BEFORE: ['shutil_copyfile.py']

    shutil.copyfile('shutil_copyfile.py', 'shutil_copyfile.py.copy')

    print('AFTER:', glob.glob('shutil_copyfile.*'))
    # AFTER: ['shutil_copyfile.py', 'shutil_copyfile.py.copy']
    ```

- `copy()` - copies to file, or create in directory

    ```python
    import glob
    import os
    import shutil

    os.mkdir('example')
    print('BEFORE:', glob.glob('example/*'))

    shutil.copy('shutil_copy.py', 'example')

    print('AFTER :', glob.glob('example/*'))
    ```

- `copytree()` - recursively copies source tree to destination

### Finding Files

- `which()` - searches for a named file

### Archives

- `get_archive_formats()` - get allowable types based on modules/libs
- `make_archive()` - Archive content
