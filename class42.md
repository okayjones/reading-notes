# Pythonisms

## Dunder Methods

[Source: Enriching Your Python Classes With Dunder (Magic, Special) Methods](https://dbader.org/blog/python-dunder-methods)

- Predefined methods
- Emulates built-in type methods

### Object Initialization (__init__)

```python
class Account:
    """A simple account class"""

    def __init__(self, owner, amount=0):
        """
        This is the constructor that lets us create
        objects from this class
        """
        self.owner = owner
        self.amount = amount
        self._transactions = []
```

```bash
>>> acc = Account('bob')  # default amount = 0
>>> acc = Account('bob', 10)
```

### Object Representation (__str__, __repr__)

- `__repr__` Official string representation; how you would make an object of the class
- `__str__` Informal nicely printed version for the end user

```python
class Account:
    # ... (see above)

    def __repr__(self):
        return 'Account({!r}, {!r})'.format(self.owner, self.amount)

    def __str__(self):
        return 'Account of {} with starting amount: {}'.format(
            self.owner, self.amount)
```

> Access object name programatically with `self.__class__.__name__`

```bash
>>> str(acc)
'Account of bob with starting amount: 10'

>>> print(acc)
"Account of bob with starting amount: 10"

>>> repr(acc)
"Account('bob', 10)"
```

### Iteration

#### __len__ & __getitem__

```python
class Account:
    # ... (see above)

    def __len__(self):
        return len(self._transactions)

    def __getitem__(self, position):
        return self._transactions[position]
```

```bash
>>> len(acc)
5

>>> for t in acc:
...    print(t)
20
-10
50
-20
30

>>> acc[1]
-10
```

#### __reversed__

```python
def __reversed__(self):
    return self[::-1]
```

```bash
>>> list(reversed(acc))
[30, -20, 50, -10, 20]
```

### Operator Overloading

- `__eq__` allows equal to comparison
- `__lt__` allows less than comparison
- `functools.total_ordering` shortcut, avoid implementing greater than

```python
from functools import total_ordering

@total_ordering
class Account:
    # ... (see above)

    def __eq__(self, other):
        return self.balance == other.balance

    def __lt__(self, other):
        return self.balance < other.balance
```

#### Merging

```python
def __add__(self, other):
    owner = '{}&{}'.format(self.owner, other.owner)
    start_amount = self.amount + other.amount
    acc = Account(owner, start_amount)
    for t in list(self) + list(other):
        acc.add_transaction(t)
    return acc
```

### Callable Objects

```python
class Account:
    # ... (see above)

    def __call__(self):
        print('Start amount: {}'.format(self.amount))
        print('Transactions: ')
        for transaction in self:
            print(transaction)
        print('\nBalance: {}'.format(self.balance))
```

```bash
>>> acc = Account('bob', 10)
>>> acc.add_transaction(20)
>>> acc.add_transaction(-10)
>>> acc.add_transaction(50)
>>> acc.add_transaction(-20)
>>> acc.add_transaction(30)

>>> acc()
Start amount: 10
Transactions:
20
-10
50
-20
30
Balance: 80
```

### Context Manager Support

- Allows use of `with`

```python
    # ... (see above)

    def __enter__(self):
        print('ENTER WITH: Making backup of transactions for rollback')
        self._copy_transactions = list(self._transactions)
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('EXIT WITH:', end=' ')
        if exc_type:
            self._transactions = self._copy_transactions
            print('Rolling back to previous transactions')
            print('Transaction resulted in {} ({})'.format(
                exc_type.__name__, exc_val))
        else:
            print('Transaction OK')

    def validate_transaction(acc, amount_to_add):
        with acc as a:
            print('Adding {} to account'.format(amount_to_add))
            a.add_transaction(amount_to_add)
            print('New balance would be: {}'.format(a.balance))
            if a.balance < 0:
                raise ValueError('sorry cannot go in debt!')
```

```bash
acc4 = Account('sue', 10)

print('\nBalance start: {}'.format(acc4.balance))
validate_transaction(acc4, 20)

print('\nBalance end: {}'.format(acc4.balance))

Balance start: 10
ENTER WITH: Making backup of transactions for rollback
Adding 20 to account
New balance would be: 30
EXIT WITH: Transaction OK
Balance end: 30
```

```bash
acc4 = Account('sue', 10)

print('\nBalance start: {}'.format(acc4.balance))
try:
    validate_transaction(acc4, -50)
except ValueError as exc:
    print(exc)

print('\nBalance end: {}'.format(acc4.balance))

Balance start: 10
ENTER WITH: Making backup of transactions for rollback
Adding -50 to account
New balance would be: -40
EXIT WITH: Rolling back to previous transactions
ValueError: sorry cannot go in debt!
Balance end: 10
```

> Example rolls back update if transaction causes negative balance

## Iterators

[Source: Python Iterators: A Step-By-Step Introduction](https://dbader.org/blog/python-iterators)

```python
class BoundedRepeater:
    def __init__(self, value, max_repeats):
        self.value = value
        self.max_repeats = max_repeats
        self.count = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.count >= self.max_repeats:
            raise StopIteration
        self.count += 1
        return self.value
```

```python
class InfiniteRepeater(object):
    def __init__(self, value):
        self.value = value

    def __iter__(self):
        return self

    def __next__(self):
        return self.value

    # Python 2 compatibility:
    def next(self):
        return self.__next__()
```

## Generators

[Source: What Are Python Generators?](https://dbader.org/blog/python-generators)

- Simpler definition for generating a value
- Primarily meant for implementing iterators


```python
def bounded_repeater(value, max_repeats):
    for i in range(max_repeats):
        yield value
```

```bash
>>> for x in bounded_repeater('Hi', 4):
...     print(x)
'Hi'
'Hi'
'Hi'
'Hi'
```
