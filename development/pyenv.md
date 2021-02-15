---
description: pyenv cheatsheet
---

# pyenv

[pyenv](https://github.com/yyuu/pyenv)

* [pyenv-virtualenv](https://github.com/yyuu/pyenv-virtualenv)
* [Command reference](https://github.com/yyuu/pyenv/blob/master/COMMANDS.md)

### pyenv install

List available python versions:

```text
    $ pyenv install -l
```

Install Python 3.5.1:

```text
    $ pyenv install 3.5.1
    $ pyenv rehash
```

### pyenv versions

List installed versions:

```text
    $ pyenv versions
```

### pyenv local

Sets a local application-specific Python version:

```text
    $ pyenv local 2.7.6
```

Unset the local version:

```text
    $ pyenv local --unset
```

### pyenv-virtualenv

#### List existing virtualenvs

```text
    $ pyenv virtualenvs
```

### Create virtualenv

From current version with name "venv35":

```text
    $ pyenv virtualenv venv35
```

From version 2.7.10 with name "venv27":

```text
    $ pyenv virtualenv 2.7.10 venv27
```

### Activate/deactivate

```text
    $ pyenv activate <name>
    $ pyenv deactivate
```

### Delete existing virtualenv

```text
    $ pyenv uninstall venv27
```

### pipenv

#### install packages

```text
    $ pipenv install <package_name>
```



