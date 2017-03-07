# SQLAlchemy ORM for Beginners

## SQLAlchemy 

- Core - Schema centric
- ORM - User Model

## Intalling

`pip install sqlalchemy`

## Connecting and establishing a session

```python
from sqlalchemy import create_engine

engine = create_engine('sqlite:///:memory:')
```

## Establishing a session

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
session = Session()
```

## Model Base

- Declarative Base
```python

from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()
```

## Cookie Model

```python
from sqlalchemy import Column, Integer, Numeric, String

class Cookie(Base):
    __tablename__ = 'cookies'

    cookie_id = Column(Integer, primary_key=True)
    cookie_name = Column(String(50), index=True)
    cookie_recipie_url = Column(String(255))
    cookie_sku = Column(String(55))
    quantity = Column(Integer())
    unit_cost = Column(Numeric(12, 2))
```

When defining a Model it should have 4 properties
1. It should inherit from declarative base
1. It should have `__tablename__`
1. Has to have one or more column
1. Has one `primary_key`

## Persisting our table

`Base.metadata.create_all(engine)`

## Adding a cookie

```python
cc_cookie = Cookie(
                    cookie_name='choclate chip',
                    cookie_recipe_url='http://something.com',
                    cookie_sku='CC01',
                    quantity=12,
                    unit_cost=0.50)
session.add(cc_cookie)
session.commit()
```

NOTE: Since there is no PK, it will assume that this is a new record

Flush is like assuming all the steps that we generally do would get done

## Accessing the attributes

```python
print(cc_cookie.cookie_id)
```

## Bulk Inserts

```
session.bulk_save_objects([c1, c2])
session.commit()
```

Single transaction with multiple inserts. 

## All the cookies

```python
cookies = session.query(Cookie).all()
```

## All the cookies - Iterator
```python
for cookie in session.query(Cookie):
    print(cookie)
```

## Particular Attributes

```python
print(session.query(Cookie.cookie_name, Cookie.quantity).first())
```

## Order By
```python
for cookie in session.query(Cookie).order_by(Cookie.quantity):
    print('{:3} - {}'.format(cookie.quanitity, cookie.cookie_name))
```

## Descending
```python
from sqlalchemy import desc
for cookie in session.query(Cookie).order_by(desc(Cookie.quantity)):
    print('{:3} - {}'.format(cookie.quanitity, cookie.cookie_name))
```

## Limiting
```python
query = session.query(Cookie).order_by(Cookie.quantity).limit(2):
print([result.cookie_name for result in query])
```

## Database Functions
```python
from sqlalchemy import func
inv_count = session.query(func.sum(Cookie.quantity)).scalar()
print(inv_count)
```

`func` knows what kind of DB its connected to and calls respective modules for connected DB

## Labeling

```python
rec_count = session.query(func.count(Cookie.cookie_name).label('inventory_count')).first()
``

## Filtering

```python
record = session.query(Cookie).filter(Cookie.cookie_name == 'cc').first()
```

## Updating Cookies

```python
query = session.query(Cookie)
cc_cookie = query.filter(Cookie.cookie_name == 'choclate chip').first()
cc_cookie.quantity = 200
session.commit()
```

## Deleting Cookies
```python
query = session.query(Cookie)
query = query.filter(Cookie.cookie_name == 'peanut butter')

dcc_cookie = query.one()
session.delete(dcc_cookie)
session.commit()
```

### Relationships

```python
from datetime import datetime
from sqlalchemy import DateTime, ForeignKey, Boolean
from sqlalchemy.orm import relationship, backref

class User(Base):
    ...

class Order(Base):
    user = relationsip("User", backref-backref('orders', order_by=))
```

## Persist Them
`Base.metadata.create_all(engine)`