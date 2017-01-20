---
layout: post
title:  "Summary of Readability Counts"
date:   2017-01-19 11:15:12 +0530
categories: python django summary
---

Writing code that is readable your peers require some extra effort apart from documentation. I found an interesting Video from Djangocon 2016 on [Readability Counts](https://www.youtube.com/watch?v=NvkC5UBJqeY&t=37s). [Terry Hunner](https://twitter.com/treyhunner) talks wonderfully on the matter.


Textbook definition from [Wikipedia](https://en.wikipedia.org/wiki/Readability) "Readability is the ease with which a reader can understand a written text."

## Why does readability matter?
* You read core often than you write
* Not all developers are mortal
* To change code you will need to read and understand it first
* Lot easier to onboard people when they can read the code

## Structuring your code
* Long lines are note easy to read despite it not being an issue on bigger screen today
* Subtleties between Line length and Text width
* **Line length**: number of characters in one line of text
* **Text width**: line length without indentations

## Wrapping Lines
* Split codes in logical blocks

## Function Calls
* Document a Style Guide for all projects on indentation {Ideally, though we don't often do it}
* Explicit conventions

## Summary
* Keep your text width narrow (60 character)
* Do not rely on automatic line wrapping
* Insert line breaks with readability in mind
* Document your code structure and then **stick to them**

## Naming things
* If a concept is important it needs names, it is used for communication
* Naming things is hard because describing them is hard
* Summarize that description using name
* Use long variable names if needed
* Worry about accuracy not the length of the name
* Replace index access with names e.g. "i, j"
* Refactor a loaded if statement to its own function. This will make code more readable

	{% highlight python %}
	def detect_anagrams(word, candidates):
	    anagrams = []
	    for candidate in candidates:
	        if (sorted(word.upper()) == sorted(candidate.upper())
	                and word.upper() != candidate.upper()):
	            anagrams.append(candidate)
	{% endhighlight %}

can be refactored to 

	{% highlight python %}
	def detect_anagrams(word, candidates):
	    anagrams = []
	    for candidate in candidates:
	        if is_anagram(word, candidate):
	            anagrams.append(candidate)
	{% endhighlight %}

Note: the **is_anagram** function

* Reading code **aloud** help to find how descriptive your code is
* When using comments it might suggest you might need more meaningful variable names E.g.

	{% highlight python %}
	def is_anagram(word1, word2):
	    word1, word2 = word1.upper(), word2.upper()
	    are_different_words = (word1 != word2)
	    have_same_letters = (sorted(word1) == sorted(word2))
	    return have_same_letters and are_different_words
	{% endhighlight %}

## So Many Functions
* Break down complex functions into helper functions

	{% highlight python %}
	def update_appointment_types(self):
	    """Delete/make appt. types and set default appt. type"""
	    self._delete_stale_appointment_types()
	    self._create_new_appointment_types()
	    self._update_default_appointment_type()
	{% endhighlight %}
* In general, try to write a self-documenting code

## Programming idioms
* Special purpose constructs can reduce complexity
* When making one list from another use List Comprehensions
* Refactor try and except with a Context Manager
* Class: If you're finding same data to multiple functions, think about making a class

## Readability Checklist
* Can I modify line breaks to improve clarity?
* Can I create a variable name for unnamed code?
* Can I add a comment to improve clarity?
* Can I turn a comment into a better variable name? 
* Can I use more specific programming construct?
* Have I stated detailed preference in a style guide? (The more decisions you push to style guide more brain power can be used for actual work)

### Hire Python Developers

We're a team of **experience** Django developers. Our Python team is 15 members strong. In our 7 years of operations we've delivered project to clients in UK, USA and India. We do take on Remote projects that matches your timezone.