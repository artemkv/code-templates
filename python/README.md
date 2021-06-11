## "Hello World"

```python
print("Hello, World!")
```

## Variable assignment

```python
x = 5
```

## Assignment expressions

```python
def calculate_space_left(n):
    return 10 - n

if (space_left := calculate_space_left(3)) >= 5:
    print(f"enough to fit 5 more, {space_left} left")
```

## F-interpolation

```python
city = 'Barcelona'
cityFormatted = f'City: {city}' # City: Barcelona
```

## Raw strings

```python
raw = r'c:\temp\text.dat'
```

## Conditional logic

```python
if (x < 5):
    print("Less than 5")
else:
    print("Equal or greater than 5")
```

## Enumerating

```python
aa = ["one", "two", "three"]

for i, a in enumerate(aa):
    print(f"{i + 1}: {a}")
```

## Tuple unpacking

```python
t = (123, "aaa")
(n, s) = t
```

## Named tuples

```python
import collections

Person = collections.namedtuple('Person', 'name age')

bob = Person(name='Bob', age=30)

print(bob.name)
print(bob.age)
```

## Function

```python
def add(x, y):
    return x + y

sum = add(5, 3)
```

## Lambda

```python
add = lambda x, y: x + y
sum = add(2, 3)
```