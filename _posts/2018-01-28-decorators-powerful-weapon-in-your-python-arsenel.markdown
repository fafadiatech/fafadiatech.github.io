---
layout: post
title:  "Summary: Decorators - A Powerful Weapon in your Python Arsenal"
date:   2018-01-28 00:00:00 +0530
categories: python summary
---

Decorators and Generators are one of the most appealing features of Pythons. Colton Myers gave a talk at [PyCon 2014](https://www.youtube.com/watch?v=9oyr0mocZTg) on decorators. Following is summary of that talk

# What is a Decorator?

This is what a decorator looks like

```python
@my_decorator
def my_awesome_function():
    pass
```

## Decorators wrap functions

1. Add functionality 
1. Modify behavior
1. Perform setup/teardown
1. Diagnostics {timing, etc}

## What is a function

- Everything in Python is an object, hence even **functions are objects**
- Functions can also create another functions

```python

# this is called closure
def make_printer(word):
    def inner():
        print(word)
    return inner

p = make_printer('such wow')
p()
```

- Decorators are also closures, they can also be created by classes

```python

# this is no-op decorator
def my_decorator(wrapped):
    def inner(*args, **kwargs):
        return wrapped(*args, **kwargs)
    return inner


# decorator is called syntactic sugar
@my_decorator
def myfun():
    pass
```

- Slightly complicated example

```python
def shout(wrapped):
    def inner(*args, **kwargs):
        print("BEFORE")
        ret = wrapped(*args, **kwargs)
        print("AFTER")
        return ret
    return inner

@shout
def myfunc():
    print("such wow!")

# Output that you will see after calling myfunc is
# >>> myfunc()
# BEFORE
# such wow!
# AFTER
# >>>
```

- Good decorators are versatile
    - Allow to re-use functionality across multiple function
- `*args` and `**kwargs` together take any number of positional and/or keyword arguments
    - `*args` as a list
    - `**kwargs` as dictionary
- When we run `myfunc.__name__` it returns `inner`
- This can be fixed by adding `inner.__name__ == wrapped.__name__` before `return inner`

## wrapt library

- [wrapt](http://wrapt.readthedocs.io/en/latest/) is a nice library that can be used to create decorators

```python
import wrapt

@wrapt.decorator
def pass_through(wrapped, instance, args, kwargs):
    return wrapped(*args, **kwargs)

@pass_through
def function():
    pass
```

- Take a note of `instance` variable this allows you to wrap classes and objects

## Decorators with arguments

Here is an example

```python

def skipIf(conditional, message):
    def dec(wrapped):
        def inner(*args, **kwargs):
            if not conditional:
                return wrapped(*args, **kwargs)
            else:
                print(message)
        return inner
    return dec

@skipIf(True, 'I hate doge')
def myfunc():
    print("very print")

# Output
# >>> myfunc()
# I hate doge
```

- Rewrite of similar functionality with wrapt

```python
import wrapt

def with_arguments(myarg1, myarg2):
    @wrapy.decorator
    def wrapped(wrapped, instance, args, kwargs):
        return wrapped(*args, **kwargs)
    return wrapped

@with_arguments(1, 2)
def function():
    pass

```

## Counting function calls

Following is an example of how to count number of times a function has been called

```python

def count(wrapped):
    def inner(*args, **kwargs):
        inner.counter += 1
        return wrapped(*args, **kwargs)
    inner.counter = 0
    return inner

@count
def myfunc():
    pass
```

- Functions are objects, we can define new variable `counter` variable

## Timing function calls

```python
import time
def timer(wrapped):
    def inner(*args, **kwargs):
        t = time.time()
        ret = wrapped(*args, **kwargs)
        print(time.time() - t)
        return ret
    return inner

@timer
def myfunc():
    print("so example!")
```