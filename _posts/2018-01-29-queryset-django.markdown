---
layout: post
title:  "Summary: Django - I didn't know Queryset could do that"
date:   2018-01-28 00:00:00 +0530
categories: django summary
---

[Queryset](https://docs.djangoproject.com/en/1.11/ref/models/querysets/) are commonly used with Django's ORM. [I Didn't Know Querysets Could do That by Charlie Guo](https://www.youtube.com/watch?v=5y7vU52jOiQ) is a nice Django Con US 2016 talk that delves deeper into useage of QuerySet.

Following is summary notes of the talk

##  Basic Queryset method

- Methods that return QuerySet
	- all
	- filter
	- exclude
	- oder_by
	- reverse
	- distinct
	- annotate
- Methods that return model instance
	- get
	- first
	- last
	- latest
	- earliest
	- exists
	- count
	- aggregate
- `annotate` and `aggregate` both expect a expression
	- annotate will compute that for each item in the set. Returns each item with new property
	- aggregate will return a dictionary with that value for entire set
	- Django will create property if not provided
	```python
	products = products.annotate(Count('item'))
	products.first().item__count

	grand_total = orders.aggregate(Sum('total'))
	grand_total['total__sum']
	```
	- Tend to be faster than for loops

## Unbasis

- `select_related`: Allows getting related objects using single query via JOINs. 
- `prefetch_related`: Similar to `select_related` will do JOINs in Python
- `defer`: Allows to fetch all except specified query
- `only`: Allows to fetch selected fields only 
- `values`: Return dictionaries rather than instances of models
- `values_list`: Returns tuples that can be iterated over
- `in_bulk`: You pass it list of IDs and returns a dictionary of ID to Model Instance
- `bulk_create`: Does the job of creation/insertions in just a single query
- When you access FK relation django issues a DB query

# Conjunction Dysfunctional

When specifying attributes in filter method of QuerySet it generates SQL query with underlying AND construct

# How to handle OR queries:

- QuerySet is different from a list. On list you can't filter on it etc. 
- `Q` E.g. find all users with certain name keywords
```python
users = User.objects.filter(
	Q(first_name__icontains='kelly') |
	Q(last_name__icontains='kelly') |
	Q(email__icontains='kelly')
).order_by('last_name')
```
- You can create as complex of an expression as you want with bitwise OR and NOT

# Query Arithmetic

`F`, while `Q` represented query contraint. F represents an implicit refrence in DB calls

```python
item_sum = Sum(F('unit_price') * F('quantity'))
total = items.aggregate(amount=item_sum).get('amount', 0)
```

# `F` expressions

They are examples of Django's **Query Expressions**. This includes:

- Avg
- Count
- Max
- StdDev
- Sum
- Variance
- Coalesce
- Concat
- Greatest
- Least
- Length
- Lower
- Now
- Substr
- Upper

# Query expressions also include:

- F
- Aggregate
- Func
- Value
- ExpressionWrapper
- Conditional

# Writing custom query expressions, you have to define following methods:

- as_sql
- as_<vendorname>
- get_lookup
- get_transform
- output_field


# Writing RAW SQLs

If you're writing RAW SQL you should probably rethink what you're about to do
- EXTRA
```python
Order.objects.extra(
	select={"is_recent": "ordered_at > 2016-01-01"}
)
```

- Other options
	- select
	- where
	- tables
	- order_by
	- select_params
	- params

```python
Order.objects.raw("""
	SELECT * FROM ...
""")
```
```


