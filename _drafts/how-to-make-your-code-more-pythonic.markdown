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

