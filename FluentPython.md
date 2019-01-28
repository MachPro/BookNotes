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

Tuple unpacking can be used for parallel assignment, variables swap.

```python
a, b = (1, 2)
b, a = a, b
```

Use start (\*) in tuple unpacking

```python
def add(*args):
    sum = 0
    for i in args:
        sum += i

t = (1,2,3)
add(*t)

# use * to store excess items
a, *b = t
```

##### Named Tuple

*namedtuple* is a factory that produces subclasses of *tuple* with field names and a class name

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

#### Slice and Ellipsis

The notation a : b : c is only valid within [] when used as index or subscript, and it produces a slice object slice(a,b,c)

... is a shortcut for the Ellipsis object

```python
# if x is a four-dimensional list
# x[i, ...] is equal to x[i, :, :, :]
```

##### Assignment to slices

```python
l = list(range(10))
# slice can be used to remove and insert elements
l[2:6] = [11,12]
# now l is [0, 1, 11, 12, 6, 7, 8 ,9]
del l[7:8]
# now l is [0, 1, 11, 12, 6, 7, 8]
```

#### Operators on Sequences

\+ and \* operator on sequences will create a new sequence

```python
a = [1, 2]
b = a * 3
# b is [1, 2, 1, 2, 1, 2]
```

For expression like **a \* n**, if a is sequence containing mutable items, then the result will hold reference to the same **a** for n times, any change to the original object will induce change to the result

```python
a = [['_'] * 2] * 2
# now a is [['_', '_'], ['_', '_']]
a[0][1] = '*'
# now a becomes [['_', '*'], ['_', '*']]
# True
a[0] is a[1]
```

Using list comp will help do the right thing

```python
a = [['_' * 2] for _ in range(2)]
a[0][1] = '*'
# now a is [['_', '*'], ['_', '_']], since it has two different inner reference
```

#### Augmented Assignment on Sequences

Special method for operator += is \_\_iadd\_\_. If \_\_iadd\_\_ is not implemented, Python will use \_\_add\_\_ instead.

For immutable object, the \_\_iadd\_\_ method is not defined. Its \_\_add\_\_ function will be invoked, and a new object will be created.

For mutable object, if the  \_\_iadd\_\_ method is defined as normal, the value of it will be changed, but the identity remains the same.

```python
l = [1, 2]
id1 = id(l)
l += [3, 4]
id2 = id(l)
# True
id1 == id2

t = (1, 2)
id1 = id(t)
t += (3, 4)
id2 = id(t)
# False
id1 == id2
```

#### Sort

*list.sort* will sort the lis in place and return None.

*sorted* will copy the original list and return it.

#### Search with bisect

bisect and bisect_right will find the last index the target can be inserted into and return the index.

bisect_left will find the first index the target can be inserted into and return the index

```python
import bisect

l = [1, 2, 4, 4, 5, 7]
target = 4
# 4
bisect.bisect(l, target)
# 2
bisect.bisect_left(l, target)
```

insort can find the index and insert target into the list.

#### Other Sequences

##### Array

When only used to store numbers, array is more efficient than list.

Array provides fast loading and saving data function such as *frombytes* and *tofile*

##### Memory View

Memory view is inspired by NumPy, it will change the way multiple bytes are read or written as units

##### Deque

**Thread-safe** double-ended queue.

## Dictionary and Set

#### Dict Comprehension

A *dictcomp* is similar to *listcomp*, which creates a *dict* instance by producing key:value pair from any iterable

```python
ITEM_MAPPING = [(1, "Catan"), (2, "Carcassone"), (3, "Pandemic")]
d = {item_id: name, for item_id, name in ITEM_MAPPING}
```

#### Update dict on missing key

```python
d = {}
d.setdefault(key, 1)
# it is equal to the following lines
if key is not in d:
    d[key] = 1
```

#### Lookup dict on missing key

##### defaultdict

*defaultdict* will use a factory method to create an empty object for missing key and return it when *\_\_getitem\_\_* is invoked

```python
import collections
# use list as factory method, empty list will be created when no keys found
dd = collections.defaultdict(list)
# output: empty list
dd[1]
# Only when __getitem__ being invoked (get item by []), it will create empty list
# Otherwise, None will be returned
# True
dd.get(2) is None
```

##### _\_missing\_\_

if *_\_missing\_\_* is implemented, then *\_\_getitem\_\_* will call it whenever a key is not found, instead of raising *KeyError*

```python
class NoMissingDict(dict):
    def __missing__(self, key):
        print("cannot find key", key)

nmd = NoMissingDict()
nmd[1]
```

#### Variations of dict

##### OrderedDict

Keys in insertion order

##### ChainMap

A list of mappings

##### Counter

Mapping holds an integer for each key, update to an existing key will increase to its count

#### UserDict

Designed as a base class to be implemented by user

*UserDict* does not inherit fropm *dict*, but it holds an internal *dict* reference called *data*

```python
import collections

class MyDict(collections.UserDict):

    def __contains__(self, key):
        return str(key) in self.data

    def __setitem__(self, key, item):
        self.data[str(key)] = item
```

#### Immutable Dict

*MappingProxyType*, a proxy for the underlying mapping, we cannot change the proxy directly, but we can modify the original mapping, and the change can be seen through the proxy

```python
from types import MappingProxyType
d = {1: 'A'}
d_proxy = MappingProxyType(d)
# d_proxy[1] = 'A'
# we cannot change the proxy directly like the following line
# d_proxy[2] = 'B'
# but we can modify the underlying mapping
d[2] = 'B'
# now d_proxy[2] = 'B'
```

#### Set

```python
# create set using {1}, {1, 2}
s = {1}
# cannot create empty set using {}, that represents dict
# we need to write set()
s = set()
```

##### Set Operation

```python
s1 = {1, 2, 3, 4, 5}
s2 = {1, 3}
e = 1

# intersection
s1 & s2
# union
s1 | s2
# difference
s1 - s2
# xor
s1 ^ s2
# predicate operation
s1 <= s2
e in s1
```

Keys must be hashable object in set and dict.

*dict* and *set* has high memory overhead, but is very efficient for search.

#### Hashable

A hashable object must satisfy:

- support *hash()* , the return value must be the same during the lifetime of the object
- support *eq()*
- if *a*==*b* is *True*, then *hash(a)* == *hash(b)* must be *True*

Flat data types like *str*, *bytes*, *int* are all hashable

*frozenset* is also hashable because all its elements must hashable by definition

*tuple*, even it is immutale, can only be hashable when all its elements are hashable

