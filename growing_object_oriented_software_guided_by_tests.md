# Growing object-oriented software, guided by tests

# Part 1: Introduction

## Chapter 1: What is the point of TDD?

#### Feedback as a fundamental tool
A team needs activity cycles to learn about the system they are building and its use and then apply that learning back into the system.
Every deployment made by the team is an opportunity to check their assumptions against reality. Deploying allows you to measure how much progress you are making, detect any errors and correct them, and adapt the current plan accordingly.
There are many feedback cycles that the team can benefit from: pair programming, unit tests, acceptance tests, daily meetings, iterations, releases and so on.
The sooner we can get feedback about any aspect of the system, the better.

#### Practices that support change
If you want to grow your system reliably and have a system that can adapt well enough to unanticipated changes, you need two things:
* Automated tests that protect you against breaking existing features when adding a new one. The team will feel more secure about adding or changing features and they will be informed very early on of them breaking something.
* Code that is easy to understand and easy to change. Programmers spend much more time reading code than writing it. Therefore code should be optimized for reading. Simplicity requires effort, so constant refactor is needed to improve design and simplicity, to remove duplication and to ensure that code expresses what it does. Automated tests protect you through the process.

It is very important to have a set of automated tests that make you feel confident that it is hard to break the system.

#### TDD in a nutshell
1. Write a failing test.
2. Write code to make test pass.
3. Refactor to make code as simple as possible.

Writing tests first has these benefits:
* It makes you think and clearly define the acceptance criteria for the next piece of work: what am I about to do? when am I done with it? what are the possible inputs? what should be the output for each of those inputs?
* encourages to write loosely coupled components so they can easily be tested in isolation as well as combined together at a higher level.

After writing the code, running those tests give you immediate feedback to detect errors when the context is fresh in your mind.

#### Refactoring, think local, act local
Refactoring means changing the internal structure of an existing body of code without changing its behaviour. The point is to improve the code so that it is a better representation of the features it implements, making it more maintainable.

Each refactoring is a step small enough to be easy to understand and to be safe. After each small step, the programmer runs the tests to ensure the system is still working.

Many small improvements can lead to significant structural improvements.

Refactoring is not the same as *redesign*, which is changing a large-scale structure.

#### The golden rule of TDD: Never write new functionality without a failing test

#### The bigger picture
When we are implementing a feature, we start by writing a failing acceptance test, not a class unit test. When the acceptance test fails, it demonstrates that the system still does not implement the feature. When it passes, we are done.

Underneath the acceptance test, we follow the unit level test / implement / refactor cycle.

The whole cycle looks like this:
![alt tag](https://raw.githubusercontent.com/jordi-chacon/book_summaries/master/pngs/TDD-loop.png)

#### End-to-End testing
Acceptance tests should exercise the system through its external access points. Avoid calling the system's internal code whenever possible.

But end-to-end testing is more than merely running the acceptance tests. End-to-End testing is about having an automated build, usually triggered by someone pushing to the repository, that checks out the latest version, compiles, runs the unit tests, packages the system, performs a production-like deployment into a realistic environment and finally exercises the system via the acceptance tests. If this automated build succeeds, the system is deployable.

#### External and internal quality
The external quality of a system is how well the system meets the needs of its customer and users (is it functional, reliable, available, responsive?). *Running* end-to-end tests tells us about the external quality of our system. *Writing* end-to-end tests tells us how well we understand the domain. End-to-end tests tell us nothing about how well we have written our code though.

The internal quality of a system is how well it meets the needs of its developers and administrator (it is easy to understand, easy to change, etc). It is what lets us cope with continual and unanticipated change in a safe and predictable manner. *Writing* unit tests gives us a lot of feedback about the quality of our code, the easier it is to write a unit test for a class, the better its design is. *Running* unit tests tells us that we haven't broken any class, but they don't give us enough confidence about the system working as a whole.

For a class to be easy to unit-test, the class must have explicit dependencies that can easily be substituted and clear responsibilities that can easily be invoked and verified. In other words, the code must be loosely coupled and highly cohesive. When you have got this wrong we find unit tests difficult to write or understand, so writing tests firsts give us valuable immediate feedback about our design.

When our code makes it difficult to write unit tests, we are tempted not to write them. But we should use such difficulties as opportunities to investigate why the test is hard to write and refactor the code to improve its structure. This is called "listening to the tests".

Elements are *coupled* if a change in one forces a change in the other.

An element's *cohesion* is a measure of whether its responsibilities form a meaningful unit. It refers to the degree to which the internals of an element belong together. Classes with "high" coherence are easier to maintain.