# Python Standard Library 2024

## List Operations

### Remove Duplicates from list

```
mylist = list(dict.fromkeys(mylist))  # remove duplicates
```

### Slice Notation

{% embed url="https://stackoverflow.com/questions/509211/understanding-slice-notation#answer-509295" %}

## Iterables

### all() function

```
ages = [25, 30, 19, 40]
all_adults = all(age >= 18 for age in ages)  # Check if all ages are 18+
print(all_adults)  # Output: True
```

### any() function

```
scores = [50, 40, 90, 30]
has_passing_score = any(score >= 60 for score in scores)  # Check if at least one score is 60+
print(has_passing_score)  # Output: True (90 is >= 60)
```

### functools.reduce()

Applies a function cumulatively to the items in an iterable, reducing them to a single result.

Syntax:

```
from functools import reduce
reduce(function, iterable, initializer=None)
```

* `function`: A function that takes two arguments and returns a single value.
* `iterable`: The sequence to process
* `initializer`: (optional) A starting value.

```python
# Example: Summing a list
from functools import reduce

numbers = [1, 2, 3, 4, 5]
result = reduce(lambda x, y: x + y, numbers)
print(result) # output: 15
```

However, you should prefer to use built-in functions like `sum()`or `max()`where possible instead of \`reduce()\`. `reduce()`actually shines when you need custom aggregation logic that built-ins don't provide. For example:

**Example 1: Aggregating Nested Data: Summing Specific Fields in a List of Dicts**

```python
from functools import reduce

transactions = [
    {"id": 1, "amount": 100},
    {"id": 2, "amount": 250},
    {"id": 3, "amount": 50}
]

# Let's get the total amount of money in all accounts

total_amount = reduce(lambda account, transaction: account + transaction["amount"], transactions, 0)
print(total_amount) # Output: 400
```

**Example 2: Finding the Longest Word in a List**

```python
words = ["apple", "banana", "cherry", "blueberry"]

longest_word = reduce(lambda x, y: x if len(x) > len(y) else y, words)
print(longest_word) # output: blueberry
```

### zip() function

The zip() function is used to combine multiple iterables (lists, tuples, etc.) **element-wise** into tuples.

Basic Syntax:

```python
zip(iterable1, iterable2, ...)
```

* Takes two or more iterables
* Pairs elements positionally into tuples
* Stops at the shortest iterable.

**Example 1: Basic Combining of Lists**

```python
names = ["Alice", "Bob", "Charlie"]
ages = [25, 30, 35]

zipped = zip(names, ages)
print(list(zipped))

# Output: [("Alice", 25), ("Bob", 30), ("Charlie", 35)]

# Pairs elements by index: ("Alice", 25), ("Bob", 30), etc.
```

**Example 2: Handling different-length iterables**

```python
names = ["Alice", "Bob"]
ages = [25, 30, 35]  # Extra element

zipped = zip(names, ages)
print(list(zipped))  
# Output: [('Alice', 25), ('Bob', 30)]
```

* Notice how it stops at the shorttest iterable (ignoring 35).
* To prevent truncation, use \`itertools.zip\_longest()\`:

```python
from itertools import zip_longest

zipped = zip_longest(names, ages, fillvalue="N/A")
print(list(zipped))  
# Output: [('Alice', 25), ('Bob', 30), ('N/A', 35)]
```

**Example 3: Using `zip`in Loops to iterate over multiple lists together**

```python
names = ["Alice", "Bob"]
ages = [25, 30]

for name, age in zip(names, ages):
  print(f"{name} is {age} years old.")
  
# Output:
# Alice is 25 years old.
# Bob is 30 years old.
```

**Example 4: Creating Dictionaries with `zip()`(more common)**&#x20;

```python
keys = ["name", "age", "city"]
values = ["Alice", 30, "New York"]

person = dict(zip(keys, values))
print(person)  
# Output: {'name': 'Alice', 'age': 30, 'city': 'New York'}
```

When to use `zip()`:

* When pairing elements from multiple iterables
* When iterating over multiple lists simultaneously
* When creating dictionaries from key-value lists.

Avoid if:

* You need padding for unequal lists - Use `zip_longest` from itertools instead.
* You need indexed access - Consider `enumerate()` instead.

### enumerate() - when you need an index

**Basic Syntax**

```python
enumerate(iterable, start=0)
```

* **`iterable`** → A list, tuple, string, or other iterable.
* **`start`** (optional) → The starting index (default is `0`).

**TLDR**

The `enumerate` function is used to add an index (counter) to an iterable, easier to track element psoitions when iterating.

```python
names = ["Alice", "Bob", "Charlie"]

for index, name in enumerate(names):
    print(index, name)

# Output:
# 0 Alice
# 1 Bob
# 2 Charlie

```

Basically, do `enumerate()`instead of the below, which is less readable:

```python
names = ["Alice", "Bob", "Charlie"]

for i in range(len(names)):
    print(i, names[i])
```

***

## itertools

### groupby()

### accumulate()

### product()

### combinations()



## Structured Data

### collections.namedtuple

* Best for lightweight, immutable data structures
* When you only need named fields and don't need methods or mutability

```notebook-python
from collections import namedtuple
# Create a new Person type with fields name, age, and city.
Person = namedtuple("Person", ["name", "age", "city"])

# Create instances
p1 = Person(name="Alice", age=30, city="New York")
p2 = Person("Bob", 25, "Los Angeles")

# Access fields by name
print(p1.name)
print(p1.age)
print(p1.city)
```

##

## Classes

### Clean Collections with classes <a href="#clean-collections-with-classes" id="clean-collections-with-classes"></a>

* `Book` with a readable `__repr__`
* `Books` as a collection that supports:
  * Iteration (`__iter__`)
  * Indexing (`__getitem__`)
  * Getting length (`__len__`)
  * String representation (`__repr__`)
  * Adding books (`add` method)

Copy

```
class Book:
    def __init__(self, title: str, author: str, year: int):
        self.title = title
        self.author = author
        self.year = year

    def __repr__(self):
        return f"Book(title={self.title!r}, author={self.author!r}, year={self.year})"


class Books:
    def __init__(self, books=None):
        self._books = list(books) if books else []

    def add(self, book: Book):
        self._books.append(book)

    def __iter__(self):
        return iter(self._books)

    def __getitem__(self, index):
        return self._books[index]

    def __len__(self):
        return len(self._books)

    def __repr__(self):
        return f"Books({self._books})"


if __name__ == '__main__':
    # Example Usage
    b1 = Book("1984", "George Orwell", 1949)
    b2 = Book("Brave New World", "Aldous Huxley", 1932)
    b3 = Book("Fahrenheit 451", "Ray Bradbury", 1953)

    library = Books([b1, b2])
    library.add(b3)

    for book in library:
        print(book)

    print(library[1])  # Get book at index 1
    print(len(library))  # Get total number of books
```
