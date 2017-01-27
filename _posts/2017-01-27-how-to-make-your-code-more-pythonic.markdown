---
layout: post
title:  "How to make your code more Pythonic?"
date:   2017-01-27 00:00:00 +0530
categories: python summary
---

We've come across couple of good Python videos that will help you take you code to next level. 

# Designing Poetic APIs by Erik Rose

[Pycon 2014 Video](https://www.youtube.com/watch?v=JQYnFyG7A8c) is a good investment of 37 minutes of your time. Following is summary we've compiled from the video:

## Don't Be An Architecture Astronaut
- Best libraries are extracted and not invented

## Consistency
- User's spend 90% of time calling other people's APIs
- Sticking to commonly used conventions can help
- E.g. Macintosh Human Interface Design is a good example of how consistency in terms of Keyboards and Menus were extracted
- API Design is similar to UI Design and principles from UI Design can be applied to API Design 
- `get(key, default) is better than fetch(default, key)` because it sticks with current conventions of Python
- Layout all options and select the best one
- If you're going to be wiered, be self consistent
- Red flags for Consistency:
	- Frequent references to your own documentation
	- Feel syntactically correct {Make sure novely pays off than shows off}

## Brevity
- Try to make common things shorts
- Red flags for Brevity:
	- Copy and Pasting same piece of code frequently
	- Typing something irrelevant
	- Long arg list, suggest lack of sane defaults

## Composability
- Being able to reuse code and use in different setting
- Aka Flexibility, Loose Coupling
- Red flags for Composability
	- Classes with lot of state
	- Deep inheritance heirarchy 
	- Violations of Law of Dementer
	- Mocking in test {too many dependencies as means of Mocking is not a good idea}
	- Options {a lot of them}

## Plain Data
- Avoid using complex/custom data structures. Use as many as standards data structures
- Try to use as many build-in data structures
- Red flags for Plain Data
	- If users take your data and convert it to "something", output should be that "something"
	- Instantiating objects to pass to another one
	- Rewriteing language-provided things

## Groviness
- Allow users to spend more time in groviness
- Avoid nonsense representations
- Fail shallowly
- Resource acquisition in initialization - {Required explicitness, error if no required argument are provided}
- Compelling Examples
- Red falgs for Groviness
	- Representable nonsense
	- Invariants that aren't
	- Lack of clear starting point
	- Long, complicated documentation

## Safety:
- Walls are there for preveting hurting yourself and others
- Red flags for Safety
	- Docs that say "remember to"... or "make sure you..."
	- Surprisingly people will blame themselves {Which points to bad design}


# Transforming Code into Beautiful, Idiomatic Python by Raymond Hettinger

[48 minutes Youtube Video](https://www.youtube.com/watch?v=OSGv2VnC0go) is another interesting one. Here is the summary

## When you see this, do that instead
- Replace traditional index manipulation with Python's core looping idioms
- Learn advanced techniques with for-else clause and two arguments of iter()
- Improve your craftsmanship and aim for clean, fast, idiomatic Python code
- Looping over a range of numbers
	``` python
	for i in [0, 1, 2, 3, 4, 5]:
		print i**2

	# Better way
	for i in range(6):
		print i**2

	# Faster way
	# Ugly way, in Python 3K this is renamed to xrange
	for i in xrange(6):
		print i**2
	```
- Looping over a collection
	```python
	colors = ['red', 'green', 'blue', 'yellow']
	
	for i in range(len(colors)):
		print colors[i]

	# Better way
	for color in colors:
		print color
	```
- Looping backwards
	```python
	colors = ['red', 'green', 'blue', 'yellow']
	
	for i in range(len(colors)-1, -1, -1):
		print colors[i]	

	# This is faster and beautiful
	for color in reversed(colors):
		print color
	```
- Looping over collection and indicies 
	```python
	colors = ['red', 'green', 'blue', 'yellow']
	
	for i in range(len(colors)):
		print i, colors[i]

	# Better way
	for i, color in enumerate(color):
		print i, color
	```
- Looping over two collections at once
	```python
	names = ['raymond', 'rachel', 'matthew']
	colors = ['red', 'green', 'blue', 'yellow']

	n = min(len(names), len(colors))
	for i in range(n):
		print names[i], "->", colors[i]

	# Better way
	# But takes more memory
	for name, color in zip(names, colors):
		print name, "->", color

	# Idiomatic way
	# Consumes less memory
	for name, color in izip(names, colors):
		print name, "->", color
	```
- Looping over sorted order
	```python
	colors = ['red', 'green', 'blue', 'yellow']

	for color in sorted(colors):
		print color

	for color in sorted(colors, reverse=True):
		print color
	```
- Custom sort order
	```python
	colors = ['red', 'green', 'blue', 'yellow']

	def compare_length(c1, c2):
		if len(c1) < len(c2): return -1
		if len(c1) > len(c2): return 1
		return 0

	print sorted(colors, cmp=compare_length)

	# Better way
	print sorted(colors, key=len)
	```
- Call a function until a sentinal value. Note: [Partial](https://docs.python.org/2/library/functools.html) take a function with one or more arguments and returns a function with fewer argument.

	```python
	blocks = []
	while True:
		block = f.read(32)
		if block == '':
			break
		blocks.append(block)

	# Better way
	blocks = []
	# iter function takes two arguments
	# 1. Function you call over and over again
	# 2. Sentinel value
	for bloc in iter(partial(f.read, 32), ''):
		blocks.append(block)
	```
- Distinguish multiple exit point in loops
	```python
	def find(seq, target):
		found = False
		for i, value in enumerate(seq):
			if value == target:
				found = True
				break
		if not found:
			return -1
		return i

	# Better way
	def find(seq, target):
		for i, value in enumerate(seq):
			if value == target:
				found = True
				break
		else:
			return -1
		return i
	```

## Dictionary Skills
- Mastering dictionaries is fundamental Python skill
- They are fundamental tool for expressing relationships, linking, counting and grouping
- Looping over dictionary keys
	```python
	d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

	for k in d:
		print k
	```
- **DONT mutate while iterating over things**
- Looping over dictionary keys and values
	```python

	for k, v in d.items():
		print k, "->", v

	# Better way
	for k,v in d.iteritems():
		print k, "->", v
	```
- Construct a dictionary in pairs
	```python
	names = ['matthew', 'rachel', 'raymond']
	colors = ['blue', 'green', 'raymond']

	d = dict(izip(names, colors))

	d = dict(enumerate(names))
	```
- Counting with dictionaries
	```python
	colors = ['red', 'green', 'red', 'blue' 'green', 'red']

	d = {}
	for color in colors:
		if color not in d:
			d[color] = 0
		d[color] += 1

	# Better way 
	d = {}
	for color in colors:
		d[color] = d.get(color, 0) + 1

	# Idiomatic way
	from collections import defaultdict
	d = defaultdict(int)

	for color in colors:
		d[color] += 1
	```
- Grouping with dictionaries
	```python
	names = ['raymond', 'rachel', 'matthew', 'roger', 'betty', 'melissa', 'judith', 'charlie']
	d = {}
	for name in names:
		key = len(name)
		if key not in d:
			d[key] = []
		d[key].append(name)

	# Better way
	d = {}
	for name in names:
		key = len(name)
		d.setdefault(key, []).append(name)

	# Better way
	d = defaultdict(list)
	for name in names:
		key = len(name)
		d[name].append(key)
	```
- Is a dictionary popitem() atomic?
	```python
	d = {'matthew': 'blue', 'rachel': 'green', 'raymond': 'red'}

	# Pop item is atomic so it can be used between threads
	while d:
		key, value = d.popitem()
		print key, "->", value
	```
- Linking dictionaires (Or chaining them)
	```python
	# Some dictionary
	default = {}
	d = defaults.copy()
	d.update(os.environ)
	d.update(command_line_args)

	d.ChainMap(command_line_args, os.environ, defaults)
	```

## Improving Clarity
- Positional arguments and indicies are nice
- Keywords and names are better
- The first way is convinient for the computers
- The second corresponds to how human's think

- Clarify function calls with keyword arguments (Good activity for someone who doesn't know the codebase)
	```python
	twitter_search('@obama', False, 20, True)

	# More readable
	twitter_search('@obama', retweets=False, numtweets=20, popular=True)
- Clarify multiple return values with named tuples
	```python
	doctest.testmod()
	(0, 4)

	# This is more readable and result of NamedTuple
	doctest.testmod()
	TestResults(failed=0, attempted=4)
	
	# How named tuples are used
	TestResults = namedtuple('TestResult', ['failed', 'attempted'])
	```
- Unpacking Sequences
	```python
		p = 'a', 'b', 'c', d
		fname = p[0]
		lname = p[1]
		age = p[2]
		email = p[3]

		fname, lname, age, email = p
	```
- Update multiple state variables
	```python
	def fibonacci(n):
		x = 0
		y = 1
		for i in range(n):
			print x
			t = y
			y = x + y
			x = t

	# Better version
	def fibonacci(n):
		x, y = 0, 1
		for i in range(n):
			print x
			x, y = y, x+y
	```

## Tuple packing and unpacking
- Don't under-estimate the advantages of updating state variables at the same time
- It eliminates an entire class of errors due to out-of-order updates
- It allows high level thinking: "chunking"

## Decorators and Context Managers
- Helps separate business logic and administrative logic
- Clean, beautiful tools for factoring code and imprving code reuse
- Good naming is essential
- Remember the Spiderman rule: With great power comes great responsibility
- Using decorators to factor-out administrative logic
	```python
	def web_lookup(url, saved={}):
		if url in saved:
			return saved[url]
		page = urllib.urlopen(url).read()
		saved[url] = page
		return page


	# Better way
	@cache
	def web_lookup(url):
		return urllib.urlopen(url).read()

	# Logic for decorator
	def cache(func):
		saved = {}
		@wraps(func)
		def newfunc(*args):
			if args in saved:
				return newfunc(*args)
			result = func(*args)
			saved[*args] = result
			return result
		return newfunc
	```
- How to open and close files
	```python
	f = open('data.txt')
	try:
		data = f.read()
	finally:
		f.close()


	# Better way, closes file automatically
	with open('data.txt') as f:
		data = f.read()
	```
- Factor-out temporary context
	```python
	try:
		os.remove('somefile.tmp')
	except OSError:
		pass

	# Better wat
	with ignored(OSError):
		os.remove('somefile.tmp')
