# Python Standard Library 2024

There are some functions or syntax things in the Python Standard Library that I might not use every day. Rather than forcing myself to Google them, it's easier to just keep a cheat sheet for when I need them. Some easier ones are included for comprehensiveness as well.

## List Operations

### Slice Notation

{% embed url="https://stackoverflow.com/questions/509211/understanding-slice-notation#answer-509295" %}

### Difference between sorted() and .sort()

sorted() creates a new list without modifying the original list.

1. `sorted()`Works on any iterable (lists, tuples, dictionaries, etc.)
2. Can be used in expressions since it returns a value.
3. **It's best when you want to keep the original list unchanged.**

.sort(): Modifies the list in-place (Does NOT return a new list)

1. Does NOT return a new list (returns `None`).
2. Only works on lists (not on tuples, sets, etc.).
3. **It's best when you don't need the original order and want to save memory.**

### Remove Duplicates from list

```
mylist = list(dict.fromkeys(mylist))  # remove duplicates
```

***

## Iterables

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

**TLDR on enumerate()**

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

**Converting enumerate() to a Dictionary**

```python
names = ["Alice", "Bob", "Charlie"]
name_dict = dict(enumerate(names, start=1))
print(name_dict)
```

Output:

```python
{1: 'Alice', 2: 'Bob', 3: 'Charlie'}
```

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

***

## itertools

### groupby()

Groups consecutive elements in an iterable based on a key function.

Syntax:

```python
from itertools import groupby

groupby(iterable, key=None)
```

* `iterable` - the sequence to be grouped
* `key` (optional) - A **function** to determine how elements are grouped

**Important**: `groupby()` only groups **consecutive** elements, so the input should be sorted by the **key first** if you want meaningful groups.

**Example: Grouping Employees by Department**

```python
from itertools import groupby

employees = [
    {"name": "Alice", "dept": "HR"},
    {"name": "Bob", "dept": "IT"},
    {"name": "Charlie", "dept": "HR"},
    {"name": "Dave", "dept": "IT"},
    {"name": "Eve", "dept": "Finance"}
]

# Sort by department before using groupby
employees.sort(key=lambda x: x["dept"])

grouped = groupedby(employees, key=lambda x: x["dept"])

for dept, group in grouped:
  print(dept, list(group))


# Output:
# Finance [{'name': 'Eve', 'dept': 'Finance'}]
# HR [{'name': 'Alice', 'dept': 'HR'}, {'name': 'Charlie', 'dept': 'HR'}]
# IT [{'name': 'Bob', 'dept': 'IT'}, {'name': 'Dave', 'dept': 'IT'}]
```

### accumulate()

Compute cumulative sums (or apply other operations) over an iterable. It's useful when **previous values affect future results**. Here are practical scenarios:

**Example 1: Running Total in a Bank Account (Cumulative Sum)**

Use Case: A person deposits money over time, and we need to track the running balance.

```python
from itertools import accumulate

deposits = [100, 200, -50, 300, -100]  # Deposits (+) and withdrawals.
balances = list(accumulate(deposits))

print(balances)
# Output: [100, 300, 250, 550, 450]
```

As you can see, it efficiently calculates **running totals** for financial data.

**Example 2: Tracking Highest Stock Price Over Time (Cumulative Max)**

**Use case**: A stock trader wants to track the highest stock price so far.

```python
from itertools import accumulate
import operator

stock_prices = [100, 105, 98, 110, 107, 120]
max_prices = list(accumulate(stock_prices, operator.max))

print(max_prices)
# Output: [100, 105, 105, 110, 110, 120]
```

As you can see, it **quickly tracks the all-time high price** in a time series.

**Example 3: Cumulatie Discounts in an Online Shopping Cart**

**Use Case**: A store applies progressive discounts, and we want to track the **effective discount** on total price.

```python
from itertools import accumulate
import operator

discounts = [5, 10, 2, 8] # Percentage discounts applied sequentially
effective_discounts = list(accumulate(discounts, operator.add)

print(effective_discounts)
# Output: [5, 15, 17, 25]
```

**Explanation:**

* First discount: `5%`
* Next: `5% + 10% = 15%`
* Next: `15% + 2% = 17%`
* Next: `17% + 8% = 25%`

As you can see, it helps **track cumulative effects of discounts** or progressive fees.

**Example 4: Tracking Calories Consumed Over the Day**

**Use Case:** a fitness tracker logs calorie intake, and we want to compute a running total.

```python
from itertools import accumulate

calories = [300, 500, 200, 400, 150]
cumulative_calories = list(accumulate(calories))

print(cumulative_calories)
# Output: [300, 800, 1000, 1400, 1550]
```

**Explanation:**

* Breakfast: `300`
* Lunch: `800` (300 + 500)
* Snack: `1000` (800 + 200)
* Dinner: `1400` (1000 + 400)
* Late snack: `1550` (1400 + 150)

As you can see, it automatically calculates **progressive intake totals.**

**Example 5: Tracking Minimum Temperature of the Dauy**

**Use Case:** A weather app logs temperature readings and wants to track the **lowest temperature recorded so far.**

```python
from itertools import accumulate
import operator

temperatures = [78, 74, 80, 72, 69, 75]
min_temperatures = list(accumulate(temperatures, operator.min))

print(min_temperatures)
# Output: [78, 74, 74, 72, 69, 69]

```

### product()

The `product()`function from itertools computes the **Cartesian product** of input iterables, meaning it generates all possible **combinations of elements** from multiple iterables.

Syntax:

```python
from itertools import product
product(iterable1, iterable2, ..., repeat=1)
```

* `iterable1, iterable2, ...`- the input iterables
* `repeat=n`  - if provided, repeats the input iterable `n`times

**Example 1: Cartesian Product of Two Lists**

```python
from itertools import product

colors = ["red", "blue"]
sizes = ["S", "M", "L"]

combinations = list(product(colors, sizes))
print(combinations)
```

```python
[('red', 'S'), ('red', 'M'), ('red', 'L'),
 ('blue', 'S'), ('blue', 'M'), ('blue', 'L')]

```

As you can see, **each color is paired with each size** → Cartesian product of `{red, blue} × {S, M, L}`.

**Example 2: Generating All Possible Clothing Variations**

**Use Case:** An e-commerce site sells T-shirts in different **colors, sizes, and brands**. We need to generate all possible product combinations.

```python
from itertools import product

colors = ["Red", "Blue", "Black"]
sizes = ["S", "M", "L"]
brands = ["Nike", "Adidas"]

products = list(product(colors, sizes, brands))

for item in products:
    print(item)
```

Output:

```python
('Red', 'S', 'Nike')
('Red', 'S', 'Adidas')
('Red', 'M', 'Nike')
...
('Black', 'L', 'Adidas')
```

As you can see, this is **useful for generating all possible variations of products.**

**Example 3: Generating Test Cases for Software Testing**

**Use Case:** A **QA engineer** needs to test a website in **multiple browsers, operating systems, and screen resolutions**.

```python
browsers = ["Chrome", "Firefox", "Edge"]
os_versions = ["Windows", "macOS", "Linux"]
resolutions = ["1920x1080", "1366x768"]

test_cases = list(product(browsers, os_versions, resolutions))

for case in test_cases:
    print(case)
```

Output:

```python
('Chrome', 'Windows', '1920x1080')
('Chrome', 'Windows', '1366x768')
('Chrome', 'macOS', '1920x1080')
...
('Edge', 'Linux', '1366x768')
```

**Example 4: Brute-forcing a 4-digit PIN (repeat argument)**

```python
from itertools import product

digits = "0123456789"
pin_combinations = product(digits, repeat=4)

for pin in pin_combinations:
    print("".join(pin))
```

**Output:**

```
0000
0001
0002
...
9999
```

**Example 3: Generating All Possible Binary Pairs (repeat)**

```python
binary = [0, 1]
combinations = list(product(binary, repeat=3))
print(combinations)
```

Output:

```python
[(0, 0, 0), (0, 0, 1), (0, 1, 0), (0, 1, 1),
 (1, 0, 0), (1, 0, 1), (1, 1, 0), (1, 1, 1)]
```

**Use `product()` when:**

* You need **all possible combinations of multiple lists**.
* You're generating **test cases** with different input values.
* You're working with **grid-based problems** (e.g., chessboards, matrices).
* You need to **brute-force passwords or PINs** (e.g., all 4-digit PINs).

### combinations()

Generates all possible subsets of a given length from an iterable, without repitition and order doesn't matter.

Syntax:

```python
from itertools import combinations

combinations(iterable, r)
```

***

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
