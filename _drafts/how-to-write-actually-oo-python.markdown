---
layout: post
title:  "How to Write Actually Object Oriented Python"
date:   2017-01-29 00:00:00 +0530
categories: python summary
---

# How to Write Actually Object Oriented Python

Summary of [PyGotham 2014 Talk](https://www.youtube.com/watch?v=VUvEDg30FyY&index=62&list=WL)

- Procedural programming simple steps and recipie of things and steps to follow that are easy to wrap head round. 
- Object oriented is like working with blue prints
- OO maps your mental model to code
- Nothing in python that forces you to be consistency, for which you have to apply "Discipline" of your own
- Advantages of having OO Code:
	- Improve Maintainibility
	- Simplifies Testing
	- Simplifies Communication
- Design Principles SOLID {Applies to Functions, Classes, Modules}

## Singe Responsibility Principle
	- Code should have one and one reason to change
	- If you have one giant class with lot of functions has some disadvantages:
		- Big class and lot of code makes design more fragile
		- Test suite is going to be real large
		- Several people working on project might step onto each others toes
	- Solution: **Break a large class into smaller logical blocks**
	- Try to break Persistence and Business Logic
	- Basically your code should co-ordinate between different function that will allow better modularity

## Open/Close Principle
	- Code should be open to extension but closed to modification
	- Use factories need to be create objects for working with current class
	- Writing code so that it doesn't have to be rewritten again

## Liskov's Substitution Principle
	- Basically Enhanced Duck Typing
	- Anywhere you use a base class, you should be able to use a subclass and not know it 
	- Same methods, Same Inteface and Same Behaviour
	- Basically Subclass shouldn't have special case behaviour, it will render code that was working fine suddenly not working

## Interface Segregation Principle
	- Don't force client to use interface they don't need
	- When you're using objects, how many interfaces am I using? Because the more you're using, tightly coupled it is

## Dependency Inversion Principle
	- High level modules should not rely on low level modules. Both should rely on abstractions
	- E.g. I am writing a music player MusicPlayer which has playlist, load data, play_sound, update the sate etc.
	- State of playback from playlist, Reading Files, Audio Subsystem
	- Refacto to following classes:
		- MusicPlayer, AudioPlayer 
		- MusciPlayers: SoundCar and NetStream {Allows swapping at runtime}

## Bonus: Tell Don't Ask
	- Tell object to do their stuff, dont ask them for their data
	- Refactor code from
	{% highlight python %}
	def calculate(self):
		cost = 0
		for line_item in self.bill.items:
			cost += line_item.cost

	# Better way
	def calculate(self):
		cost = self.bill.total_cost()
	{% endhighlight %}
