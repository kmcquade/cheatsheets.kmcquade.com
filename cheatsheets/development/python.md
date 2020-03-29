---
description: Python related commands that I frequently use
---

# Python

### Click's Shell completion

```text
eval "$(_POLICY_SENTRY_COMPLETE=source_zsh policy_sentry)"

# https://click.palletsprojects.com/en/7.x/bashcomplete/#activation
```

### Convert list of strings to all lowercase

```text
actions_list = [x.lower() for x in actions_list]
```

### Lists

Sort list in alphabetical order:

```text
mylist.sort()
```

### Remove duplicates from list:

```text
mylist = list(dict.fromkeys(mylist))  # remove duplicates

```

### Capitalize the first character of a string

```text
def capitalize_first_character(some_string):
    """
    Description: Capitalizes the first character of a string
    :param some_string:
    :return:
    """
    return " ".join("".join([w[0].upper(), w[1:].lower()]) for w in some_string.split())
```

### Count number of times a string appears in a list

```text
list.count(x)
# returns the number of times x appears in a list
# http://docs.python.org/tutorial/datastructures.html#more-on-lists
```

### Virtualenv 

```text
# Python 3
virtualenv  --python=/usr/local/bin/python3 ./.venv
source .venv/bin/activate

# Python 2.7
virtualenv  --python=/usr/local/bin/python2.7 ./.venv
source .venv/bin/activate
```



### PyPi upload

From your code directory that contains the `setup.py` script:

```text
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

```text
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



### Install python version with pyenv on Mac <a id="install-python-version-with-pyenv-on-mac"></a>

See this GitHub issue: [https://github.com/pyenv/pyenv/issues/1219](https://github.com/pyenv/pyenv/issues/1219)

```text
CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" pyenv install -v 3.7.0
```



