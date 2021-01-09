# Game of Greed 4

## Dunder Methods

[Source: Enriching Your Python Classes With Dunder (Magic, Special) Methods](https://dbader.org/blog/python-dunder-methods)

- Special set of predefined methods to enrich classes
- Allows you to emulate built in behavior

### Some Examples

- `__len__` for length
- `__str__` for printing (human readable)
- `__getitem__` for list slicing
- `__init__` to construct an object
- `__repr__` string representation

#### Str and Repr

```python
class Account:
    # ... (see above)

    def __repr__(self):
        return 'Account({!r}, {!r})'.format(self.owner, self.amount)

    def __str__(self):
        return 'Account of {} with starting amount: {}'.format(
            self.owner, self.amount)
```

```python
>>> str(acc)
'Account of bob with starting amount: 10'

>>> print(acc)
"Account of bob with starting amount: 10"

>>> repr(acc)
"Account('bob', 10)"
```

#### Len and Iteration

```python
class Account:
    # _transations is a list of numbers

    def __len__(self):
        return len(self._transactions)

    def __getitem__(self, position):
        return self._transactions[position]
    
    def __reversed__(self):
    return self[::-1]
```

#### Comparison

Import total_ordering to avoid implementing EVERY comparison method.

```python
from functools import total_ordering

@total_ordering
class Account:
    # ... (see above)

    def __eq__(self, other): #equal
        return self.balance == other.balance

    def __lt__(self, other): # less than
        return self.balance < other.balance
```

#### Adding / Merging

To merge to accounts, implement add:

```python
    owner = '{}&{}'.format(self.owner, other.owner)
    start_amount = self.amount + other.amount
    acc = Account(owner, start_amount)
    for t in list(self) + list(other):
        acc.add_transaction(t)
    return acc
```

#### Make it callable like a function

Not a standard practice, better to have a function on the class do this.

```python
class Account:
    # ... (see above)

    def __call__(self):
        print('Start amount: {}'.format(self.amount))
        print('Transactions: ')
        for transaction in self:
            print(transaction)
        print('\nBalance: {}'.format(self.balance))

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

#### Context Manager and `With`

 > A context manager is a simple “protocol” (or interface) that your object needs to follow so it can be used with the with statement. Basically all you need to do is add `__enter__` and `__exit__` methods to an object if you want it to function as a context manager.

If balance goes negative, rollback to previous state:

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
```

Helper method to validate transaction

```python
def validate_transaction(acc, amount_to_add):
    with acc as a:
        print('Adding {} to account'.format(amount_to_add))
        a.add_transaction(amount_to_add)
        print('New balance would be: {}'.format(a.balance))
        if a.balance < 0:
            raise ValueError('sorry cannot go in debt!')
```

## Probability

[Source: Tutorial: Basic Statistics in Python — Probability](https://www.dataquest.io/blog/basic-statistics-in-python-probability/)
