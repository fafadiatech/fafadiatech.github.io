---
layout: post
title:  "How to Be More Effective with Functions"
date:   2017-01-15 00:00:00 +0530
categories: python summary
---

[Effective Python](https://www.safaribooksonline.com/library/view/effective-python-59/9780134034416/) is one the good books that is thought of to be Python 202. Once you've got the hang of basics, you'd like to loook at ways to improve the code. This is exactly what this books provides.

[Bret Saltkin](https://twitter.com/haxor?lang=en) gives a nice talk about this during PyCon 2015, titled [How to Be More Effective with Functions](https://www.youtube.com/watch?v=WjJUPxKB164). Following is summary of that talk

## Agenda

1. Reduce visual noise with variable positional arguments 
1. Provide optional behaviou with keyword arguments
1. Enforce clarity with keyword-only argument
1. Consider generators instead of returing lists
1. Be defensive when iterating over arguments

## Reduce visual noise with variable positional arguments

This helps reduce visual clutter. Lets look at how the following snippet can be refactored

```python
def log(message, values):
    if not values:
        print(message)
    else:
        out = ', '.join(str(x) for x in values)
        print('%s: %s' % (message, out))

log("My numbers are", [1, 2])
log("Hi there", [])
```

Using positional argument we can do it as

```python
def log(message, *values):
    if not values:
        print(message)
    else:
        out = ', '.join(str(x) for x in values)
        print('%s: %s' % (message, out))

log("My numbers are", 1, 2)
log("Hi there")

# Alternative synxtax to achieve same
# thing
nums = [1, 2, 3]
log("My numbers are", *nums)
```