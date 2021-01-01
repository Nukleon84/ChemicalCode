---
layout: post
title: "Pyflowsheet - Part 2: Clean Code and Refactoring"
author: "Jochen Steimel"
categories: journal
tags: [cleancode pyflowsheet python softwarengineering]
image: pyflowsheet_image.png
---


So now that a week has passed between the initial release of the pyflowsheet package has passed, maybe it is time to reflect on the code. 
In my opinion, it is the duty of the software developer, to deliver the best possible code that he or she is able to create. Does this mean that the pyflowsheet code is already good? Of course not! Nobody get’s it right the first time. That is why refactoring is so important in software engineering to ensure that code quality is high, and we do not create an unmaintainable mess. Or, as some other person would say: create legacy code at the speed of typing.

Since many years I have been a fan of the clean-code movement: starting my journey with the German branches (as propagated by Stefan Lieser and Ralf Westphal) to discovering the American school around Robert C. “Uncle Bob” Martin I was always intrigued with the ideas taught by “Clean Code”. Making code re-usable, even after years. Making code evolvable (because let’s face it: requirements WILL change in the future). Making code transferable to other authors.

During the rapid prototyping development phase of pyflowsheet (the alpha version was written in 4 days before Christmas) a few “code-smells” crept into repository. Things that I knew were bad, but couldn't figure out a way to do correctly (at the time of coding).

## About Clean Code
But before we go into the details of these smells, maybe we should recapitulate what Clean code should embody. In the namesake book, Uncle Bob recites a few anecdotes from his colleagues:
* Bjarne Stroustrup (inventor of C++) tells us that code should be “elegant and efficient”. He goes on to say the “clean code does one thing well”. And that “its performance is close to optimal so as not to tempt people to make the code messy with unprincipled optimizations”.
* Michael Feathers said “Clean code always looks like it was written by someone who cares”.
* Ron Jeffries (of Extreme Programming fame) states that clean code “runs all the tests, contains no duplicates, expresses all the design ideas and minimizes the number of code entities”.
* 	Ward Cunningham (inventor of Wiki) summarizes the topic succinctly “You know you are working on clean code when each routine you read turns out to be pretty much what you expected”

Taking these definitions into account, it becomes quite apparent that the pyflowsheet codebase (at the current state) does in no way embody clean code. But fret not, as mentioned earlier “nobody gets it right the first time”, so we still have a chance to fix it (before it is too late). 

The code is still quite young and small, so changes can be implemented easily. There are no users “in the wild” which one could anger with breaking API changes, and even if they are, the alpha state of the software makes it clear that changes are to be expected. Once the software reaches 1.0 status, changes will be MUCH more difficult to implement, so I better get started fast!

## The problems of the current design
The most obvious code smells are the following:
* There are no tests. Cleary, this is inexcusable and should be fixed ASAP. But as I was writing the code as a distraction, I had no clear understanding of any requirements, and coded “at free will”. Now that I have a better understanding of the domain, I can add tests for each method to make sure that my refactoring does not break the system. As Uncle Bob rightly says, testable code is the foundation of clean code, because otherwise you have no guarantee that your changes don’t accidentally make your product worse.
* The code duplication with horizontal and vertical vessels. This decision was made before I implemented rotation and should now be rolled back into a single design: a vessel, that can be rotated into any direction
* The handling of internals. Currently internals (like tubes, reactive beds, trays) are added to the unit operations with a simple string. This is bad in so many ways that I want to focus a bit on this issue in the next paragraph.
This is a code extract from the draw-method of the Vessel class.

```python
if self.internals and self.internals.lower() == "tubes":
    for i in range(1, 5):
        x = self.position[0] + self.size[0] / 5 * i
        start = (x, self.position[1] + capLength)
        end = (x, self.position[1] + self.size[1] - capLength)
        ctx.line(start, end, self.lineColor, self.lineSize)

if self.internals and self.internals.lower() == "bed":
    start = (self.position[0], self.position[1] + capLength)
    end = (
        self.position[0] + self.size[0],
        self.position[1] + self.size[1] - capLength,
    )
    ctx.line(start, end, self.lineColor, self.lineSize)

    start = (self.position[0] + self.size[0], self.position[1] + capLength)
    end = (self.position[0], self.position[1] + self.size[1] - capLength)
    ctx.line(start, end, self.lineColor, self.lineSize)
```
## Violations of the principles of Clean Code
The current design around unit internals violates several principles of SOLID code:
* Single Responsibility-Principle (SRP): The draw-method unit operation class (and its derived subclasses) is responsible, not only for drawing the unit symbol itself, but also all variations of internals.
* Open-Closed-Principle (OCP): Currently, there is no way to add new internals to a unit without changing the source-code of the draw-method. This is bad, because it does not allow the user to quickly add new functionality to the library but requires the package maintainer to add all the internals.
* Interface-Segregation-Principle (ISP): To allow access to the graphical properties of the internals, they need to be added to the public interface (or contract) of the unit operation class. This will, over time, lead to this class getting more bloated and turn into the feared “god class” of the system, a huge blob that contains all possible fields and data structures, and is impossible to maintain or change.
* In addition, it also violates an even more basal rule of clean-code: Don’t repeat yourself (DRY). In the current version of the code, the code fragments for drawing beds are repeated in the vessel and the column class, and will creep into any other related class that should have an X drawn on top of the basic shape.
* Last but not least, the current design runs into the problem of combinatorial explosion. Take for example a simple reactor. It could have internal or external heating coils. It could have an internal mixer. In the current design, that would already necessitate four strings (“internalcoil”, “externalcoil”, “internalcoil-mixer”, “externalcoil-mixer”) to cover the possible combinations. And it only gets worse from here.

## How to improve the code?
Now that the issues of the current design are understood, what will be the solution?

The obvious solution to the problem in which the state or identity influences the behavior is of course Polymorphism. In the improved (I dare not say final) design, internals shall be encapsulated in small classes, that describe how the symbols should be drawn on the diagram. In addition I will apply the [Composite pattern](https://en.wikipedia.org/wiki/Composite_pattern) to model the whole-part relationship between a Unit Operation and the internals.

```python
 def draw(self, ctx):

        #private method that handles the drawing of the "pill" shape
        self._drawVesselShell(ctx)

        # Composite Pattern 
        for internal in self.internals:
            internal.draw(ctx)

        super().draw(ctx)
        return
```

Instead of having a series of if-conditions that test what should be drawn on the image, the draw-method of the unit operation will call the draw-methods of all the internals and delegate the act of drawing to those composite elements. By drawing the internals from within the draw-method of the unit operation, the same rotation transform will also be applied to the internals.

For example, a vessel instance would in future be created with the following function call:

```python
Vessel("U10","Vessel (with tubes)", position=(200,300), size=(80,40),internals=[TubesInternals] )
```

Notice the list literal at the end: this will allow the user to *stack* different internals onto each other to create combinations unforeseen by the library developer. In addition, users can create new derived classes from the BaseInternal class and implement their own drawing logic for icons not supported by the pyflowsheet package.

The code to create a drawing of a fermenter could look like the following: create a vertical vessel with external coils, an impeller stirrer and a sparger for gas diffusion.

```python
fermenter = Vessel("U10","Bio-Fermenter", position=(200,300), size=(40,100), internals=[ExternalCoils, Impeller, Sparger] )
```

I now have to figure out how the geometric information from the parent unit operation shall be propagated to the internals, and how to handle vertical and horizontal rotation for the vessel icon. I will implement the new system for internals in the coming weeks and it will probably lead to a new release of the library on the PyPI package manager.

## Summary

The quest for clean code is never finished. It is more a craving for an ideal state, but one must accept that it cannot be realistically obtained. What we can do instead, is treat our software as we treat our physical technical systems: Attend to them regularly, switch out broken parts, buff up the paint and leave the system in a better state then we found it in. Writing code is not a fire-and-forget activity, but requires regular maintenance and care.

