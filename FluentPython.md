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
