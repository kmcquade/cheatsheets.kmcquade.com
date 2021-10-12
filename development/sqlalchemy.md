# Python SQLAlchemy

## SQLAlchemy query with OR/AND/like common filters

Some of the most common operators used in filter() method SQLAlchemy

## equals:

```
query.filter(User.name == 'leela')
```

## not equals:

```
query.filter(User.name != 'leela')
```

## LIKE:

```
query.filter(User.name.like('%leela%'))
```

## IN:

```
query.filter(User.name.in_(['leela', 'akshay', 'santanu']))

# works with query objects too:

query.filter(User.name.in_(session.query(User.name).filter(User.name.like('%santanu%'))))
```

## NOT IN:

```
query.filter(~User.name.in_(['lee', 'sonal', 'akshay']))
```

## IS NULL:

```
filter(User.name == None)
```

## IS NOT NULL:

```
filter(User.name != None)
```

## AND:

```
from sqlalchemy import and_
filter(and_(User.name == 'leela', User.fullname == 'leela dharan'))

#or, default without and_ method comma separated list of conditions are AND

filter(User.name == 'leela', User.fullname == 'leela dharan')

# or call filter()/filter_by() multiple times

filter(User.name == 'leela').filter(User.fullname == 'leela dharan')
```

## OR:

```
from sqlalchemy import or_
filter(or_(User.name == 'leela', User.name == 'akshay'))
```

## match:

```
query.filter(User.name.match('leela'))
```
