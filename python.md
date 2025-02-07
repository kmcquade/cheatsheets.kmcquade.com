---
description: Python related commands that I frequently use
---

# Python

### Slice Notation

[https://stackoverflow.com/questions/509211/understanding-slice-notation#answer-509295](https://stackoverflow.com/questions/509211/understanding-slice-notation#answer-509295)

### Override Pip Index URL

```
export PIP_EXTRA_INDEX_URL=https://pypi.python.org/simple/
```

### Pip3 Freeze uninstall all packages

```
pip3 freeze | xargs pip3 uninstall -y
```

### Click's Shell completion

```
eval "$(_POLICY_SENTRY_COMPLETE=source_zsh policy_sentry)"

# https://click.palletsprojects.com/en/7.x/bashcomplete/#activation
```

### Convert list of strings to all lowercase

```
actions_list = [x.lower() for x in actions_list]
```

### Lists

Sort list in alphabetical order:

```
mylist.sort()
```

### Remove duplicates from list:

```
mylist = list(dict.fromkeys(mylist))  # remove duplicates

```

### Get values of particular key in list of dictionaries using list(map(itemgetter()))

```
desired_output = {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "ExampleSid1",
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        },
        {
            "Sid": "ExampleSid2",
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
result = list(map(itemgetter("Sid"), output.get("Statement")))
print(result)

# output will equal:
# ['ExampleSid1', 'ExampleSid2']
```

### Pipenv - install from Git

```
pipenv install -e git+https://github.com/kmcquade/policy_sentry.git@fix/query-functions-with-all-params#egg=policy_sentry
```

### Capitalize the first character of a string

```
def capitalize_first_character(some_string):
    """
    Description: Capitalizes the first character of a string
    :param some_string:
    :return:
    """
    return " ".join("".join([w[0].upper(), w[1:].lower()]) for w in some_string.split())
```

### Count number of times a string appears in a list

```
list.count(x)
# returns the number of times x appears in a list
# http://docs.python.org/tutorial/datastructures.html#more-on-lists
```

### Sort Dictionary by Values

```
from operator import itemgetter

my_dict = sorted(my_dict, key=itemgetter("Type", "Principal", "PolicyType", "PolicyName"))
print(my_dict)
```

### Virtualenv&#x20;

```
# Python 3
virtualenv  --python=/usr/local/bin/python3 ./.venv
source .venv/bin/activate

# Python 2.7
virtualenv  --python=/usr/local/bin/python2.7 ./.venv
source .venv/bin/activate
```



### PyPi upload

From your code directory that contains the `setup.py` script:

```
## Pipenv
pipenv shell
pipenv install

## Create the package files
python3 -m pip install --upgrade setuptools wheel
python3 setup.py sdist bdist_wheel

## PyPi test server
python3 -m pip install --upgrade twine
python3 -m twine upload --repository-url https://test.pypi.org/legacy/ dist/*
python3 -m pip install --index-url https://test.pypi.org/simple/ --no-deps policy_sentry

## PyPi Prod
python3 -m pip install --upgrade twine
python3 -m twine upload dist/*
python3 -m pip install policy_sentry
```

### Pep8

```
# Show ONLY instances of missing-function-docstring
pylint policy_sentry --disable=all -e missing-function-docstring

# Show instances of everything EXCEPT missing-function-docstring
pylint policy_sentry --disable=all -e missing-function-docstring

# Pycodestyle and autopep8 seem like they don't address everything that can come up from Pylint. Still useful though
pycodestyle --quiet --statistics policy_sentry
autopep8 --in-place --aggressive --aggressive  --select=E501 -r policy_sentry --max-line-length 120
```



### Things to add

* chomp
* split
* list comprehensions



### Install python version with pyenv on Mac <a href="#install-python-version-with-pyenv-on-mac" id="install-python-version-with-pyenv-on-mac"></a>

See this GitHub issue: [https://github.com/pyenv/pyenv/issues/1219](https://github.com/pyenv/pyenv/issues/1219)

```
CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install -v 3.7.0
```



### Create Homebrew formula for Python program

```
mkdir python-brewer-test
python3 -m venv ./venv
pip install python-brewer
pip install -U policy_sentry
# Then follow the pybrew commands from their web page.
# https://pypi.org/project/python-brewer/

pybrew \
    -n "Policy Sentry" \
    -d "IAM Least Privilege Policy Generator" \
    -H https://policy-sentry.readthedocs.io \
    -g https://github.com/salesforce/policy_sentry.git \
    -r https://files.pythonhosted.org/packages/1d/26/d461385d60da16e828c8397c06f414e6d2b410c7ff70d71247c625cefdd9/policy_sentry-0.8.0.5.tar.gz  \
    policy-sentry \
    policy_sentry.rb
```

### Pip freeze requirements

```
pip3 freeze > requirements.txt
```

## Clean Collections with classes

* `Book` with a readable `__repr__`
* `Books` as a collection that supports:
  * Iteration (`__iter__`)
  * Indexing (`__getitem__`)
  * Getting length (`__len__`)
  * String representation (`__repr__`)
  * Adding books (`add` method)

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
