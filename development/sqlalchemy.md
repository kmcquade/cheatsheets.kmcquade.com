# Python SQLAlchemy

## SQLAlchemy query with OR/AND/like common filters

Some of the most common operators used in filter\(\) method SQLAlchemy

## equals:

```text
query.filter(User.name == 'leela')
```

## not equals:

```text
query.filter(User.name != 'leela')
```

## LIKE:

```text
query.filter(User.name.like('%leela%'))
```

## IN:

```text
query.filter(User.name.in_(['leela', 'akshay', 'santanu']))

# works with query objects too:

query.filter(User.name.in_(session.query(User.name).filter(User.name.like('%santanu%'))))
```

## NOT IN:

```text
query.filter(~User.name.in_(['lee', 'sonal', 'akshay']))
```

## IS NULL:

```text
filter(User.name == None)
```

## IS NOT NULL:

```text
filter(User.name != None)
```

## AND:

```text
from sqlalchemy import and_
filter(and_(User.name == 'leela', User.fullname == 'leela dharan'))

#or, default without and_ method comma separated list of conditions are AND

filter(User.name == 'leela', User.fullname == 'leela dharan')

# or call filter()/filter_by() multiple times

filter(User.name == 'leela').filter(User.fullname == 'leela dharan')
```

## OR:

```text
from sqlalchemy import or_
filter(or_(User.name == 'leela', User.name == 'akshay'))
```

## match:

```text
query.filter(User.name.match('leela'))
```

