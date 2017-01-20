---
layout: post
title:  "Tips from High Peformance Django Book"
date:   2017-01-25 00:00:00 +0530
categories: python django summary
---

1. Use same DB vendor on both Production and Development, SQLite is not a good option because of its Locking mechanisms
2. Separate out settings:
	* settings.base: A shared setting across all environments
	* setting.dev: Tweaks to get your local project working
	* settings.production: Tweaks to get your production settings working
3. Store sensitive information like SECRET_KEY, DATABASE_URL in environmental variables. You can also store in a .ini/.env file and have manage scripts load setting from there
	* For e.g. `export $(cat .env | grep -v ^# | xargs)`
	* Accessing environment variables 
	``` python
	import os
	SECRET_KEY = os.environ.get('SECRET_KEY')
	```
4. Storing and Accessing sensitive information from environmental variables might make sense for *strings**, but about boolean and objects? 
	```python

   import ast
   import os
   DEBUG = ast.literal_eval(
       os.environ.get('DEBUG', 'True'))
	
	# "/path/templates1,/path/templates2" # converts to a tuple
	TEMPLATE_DIRS = ast.literal_eval(
       os.environ.get('TEMPLATE_DIRS'))
	```
5. One last helpful tip we use to prevent us from running with the wrong settings is to symlink it in each environment to local.py. Then we modify manage.py to use this file by default.
6. [djang-debug-toolbar](https://github.com/jazzband/django-debug-toolbar) is useful for debugging
7. To reduce number of queries while using Djago's ORM use `select_related` and `prefetch_related` where traversal of foreign key relationship after initial lookup is required E.g.
	```python
	 # one query to post table
	post = Post.objects.get(slug='this-post') # one query to author table
	name = post.author.name
	```

	can be transformed to
	```python
	post = (Post.objects.select_related('author') .get(slug='this-post'))
	```
8. Similar case
	```python
	post_list = Post.objects.all()
	{% for post in post_list %} {{ post.title }}
		By {{ post.author.name }}
		in {{ post.category.name }}
	{% endfor %}

	# Above can be rewritten as
	post_list = (Post.objects.all().select_related('author', 'category'))
    ```
9. Counter part of select_related is prefetch_related which given a child pre-fetches parent objects
10. Look for queries that are repeated. In some scenarios it will be cheaper to take advantage of the cached query and do the additional filtering in memory:
	```python
	post = Post.objects.get(slug='this-post')
	author = Author.objects.get(pk=post.author_id)

	post = (Post.objects.prefetch_related('tags').get(slug='this-post'))

	all_tags = post.tags.all()
	active_tags = post.tags.filter(is_active=True)

	# this is where caching is useful
	active_tags = [tag for tag in all_tags if tag.is_active]
	```
 11. [prefetch_related](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#prefetch-related) should be used when possible
 12. Queries where we're using WHERE queries frequently should use [db_index=True](https://docs.djangoproject.com/en/1.10/ref/models/fields/#db-index)
 13. Indexing on multiple columns should be handled using [index_together](https://docs.djangoproject.com/en/1.10/ref/models/options/#index-together)
 14. Django has built in deployment checklist using following command `python manage.py check --deploy`, [more documentation](https://docs.djangoproject.com/en/1.10/howto/deployment/checklist/)
 15. Avoid using .all() on a table with large number of rows. Instead use [:20] i.e limit queryset to 20 rows while fetching results. Things can be paginated
 16. Replace .exists() in place of `if something.count() > 0` this will give a good performance increase
 17. Build "fat models" and "skinny views" i.e all business logic should be implemented in models.
 18. [cached_property](https://docs.djangoproject.com/en/1.10/ref/utils/#django.utils.functional.cached_property) should be used for computations that are expensive within models
 19. QuerySet methods likse [defer](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#django.db.models.query.QuerySet.defer), [only](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#only), [values](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#values) and [value list](https://docs.djangoproject.com/en/1.10/ref/models/querysets/#values-list) are useful to avoid fetching un-necessary data
 20. Other DB tricks for Speeding up:
 	* Adding DB Read-Only Replicas
 	* [Raw Queries](https://docs.djangoproject.com/en/1.10/topics/db/sql/#executing-raw-queries) could be written at expense of chance of SQL Injection
 	* Denormalization
 	* Sharding (Use it only if necessary)
 21. `jinja` templates may be sawpped in place of django templates. 
 22. [Russian Doll Caching](http://blog.remarkablelabs.com/2012/12/russian-doll-caching-cache-digests-rails-4-countdown-to-2013) is a technique borrowed from RoR world. Basically having nested cache calls with different expiratons. Since whole template won't expire at once bits and pieces would be rendered on each request. [cache](https://docs.djangoproject.com/en/1.10/topics/cache/#template-fragment-caching) template tag comes to rescue.

 	```python
	{% cache MIDDLE_TTL "post_list" request.GET.page %} {% include "inc/post/header.html" %}
		<div class="post-list">
		{% for post in post_list %}
	 		{% cache LONG_TTL "post_teaser_" post.id post.last_modified %} 
	 			{% include "inc/post/teaser.html" %}
			{% endcache %} 
		{% endfor %}
	</div>
	{% endcache %} 	
 	```
 23. Good defaults for Cache is:
 	* SHORT -> 10 mins
 	* MEDIUM -> 30 mins
 	* LONG -> 7 days
 24. More complex cache implementation could require writing a custom cache template tag
 25. Offload tasks that take time to `Celery`
 26. Frontend optimization can be done using [django-compressor](https://django-compressor.readthedocs.io/en/latest/) or [django-pipelines](https://django-compressor.readthedocs.io/en/latest/)
 27. Part of application that are heavy on JS can use [Require.js](http://requirejs.org/)
 28. Compress Images
 29. Serve Images from CDN
 30. Continous Integration
 	- Coverage
 	- Pylint
 	- Flake8
 31. Django caching can be configured with Redis/Memcache. Redis seems to be a better option. 
 32. SESSION_ENGINE by default it is DB where session information is stored. Redis would be a good option where SESSION_ENGINE should point to. [Tutorial](http://michal.karzynski.pl/blog/2013/07/14/using-redis-as-django-session-store-and-cache-backend/) covers in detail steps to achieve it. 
 33. CONN_MAX_AGE is time in seconds till Django maintains connection with the DB for **every** request. 300 is a good default
 34. [Per View Caching](https://docs.djangoproject.com/en/1.10/topics/cache/#the-per-view-cache) is also possible with Django
 35. Lock Down SSH
 36. [ab](https://httpd.apache.org/docs/2.4/programs/ab.html)is a good tool for doing sanity load testing. Advanced testing should be done using `jmeter` a good [Tutorial](https://lincolnloop.com/blog/load-testing-jmeter-part-1-getting-started/) is recommended