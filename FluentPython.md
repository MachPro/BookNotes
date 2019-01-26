# Fluent Python

## Python Data Model

Python intepreter uses special methods to perform special operation, such as iteration, operator overloading, object creation and destruction. 

```python
# get item by index
def __getitem__(position)
# create object
def __init__()
# overload add operator
def __add__(other)
```

#### String Representation

```python
# repr should match the source code to create the object being represented
def __repr__()
# str should return a human readable string to user
def __str__()
```

**\_\_repr\_\_** will be called if no **\_\_str\_\_** exists

#### Boolean Value of Object

```python
def __bool__()
def __len__()
```

if **\_\_bool\_\_** not implemented, **\_\_len\_\_** will be called, if that returns zero, *bool* returns *False*

## Sequences

#### Overview of built-in sequences

We can group all the built-in sequences data structures together based on two standards.

The first standard is complexity.

* Container Sequences (Complex sequences)

  list, tuple, collections.deque

* Flat Sequences (Simple sequences)

  str, bytes, bytearray, memoryview, array.array

The second standard is immutability.

* Immutable sequences

  tuple, str, bytes

* Mutable sequences

  list, bytearray, array.array, collections.deque, memoryview

#### List Comprehension & Generator Expression

Listcomp: operations to build a new list

Generator expression: similar to listcomp, but can be used to create all sequences

```python
symbol = 'abcde'
# list comp, enclosed in brackets
cs = [i for i in symbol]
# generator expr, enclosed in parentheses
tuple(i for i in symbol)
```

#### Tuple

##### Tuple Unpacking

Tuple unpacking can be used for parallel assignment, swap variables.

Use start (\*) in tuple unpacking

##### Named Tuple

```python
from collections import namedtuple
Board = namedtuple('Board', ['name', 'price'])
catan = Board(name='Catan', price=10)

data = ('Carcarssone', 10)
# instantiate a named tuple from iterable
Board._make(data)
# return ordered dict
catan._asdict()
```
