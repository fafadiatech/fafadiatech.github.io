# Optimizing your webapp by using Django Debug Toolbar

- Django is collection of tools, but tool can be used in good or bad ways. 
- Django ORM is set of the tool
- Unicorns don't exists: Tools can never be perfect, so we need to understand that there will be one tool that will understand me
- Django ORM is an abstraction layer
	- It takes us away from messy details
	- It is risky because it takes us away from messy details
	- Don't forget that you're far from ground
- The QuerySet API, keep in mind
	- They are Lazy: QuerySet doesn't evaldate until it needs to
	- They are Immutable: Never itself changes. 
- Each operations return a new QuerySet none hits the database
- Cases where following queryset hits the database:

```python
queryset = list(queryset)

queryset = queryset[:]

for model_object in queryset:
	do_something(...)
```
- If you can't measure it, you never know it there is a problem
- Since Model Objects and Query Set are lazy they are not evaluated until needed. What this implies is Foreign Key relations that are accessed in Templates are not loaded upfront
- `select_related()`
	- `select_related` works by creating a SQL join and including the fields of the fields of the related object in SELECT statement
	- For this reason, select_related gets the related objects in the same database query
	- However, to avoid the much larger result set that would result from joining acorss as 'many' relationship, select_related is limited to single-valued relationships - foreign key and one-to-one
- `prefetch_related()`
	- `prefetch_related` pre-fetch those relations and does join in Python, which is more efficient. 
	- in addition to the foreign key and one-to-one relations
	- It also supports prefetching of GenericRelation and GenericForeignKey
