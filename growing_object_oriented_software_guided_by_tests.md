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

<br />

## Chapter 2: TDD with objects

#### Tell, don't ask
Tell-Don't-Ask is a principle that reminds us that rather than asking an object for data and then acting on that data ourselves, we should instead tell an object what to do. This encourages to move behavior into an object to go with the data. In other words, we should place data and behaviour related to that data in the same component.

#### But sometimes ask
The above principle should not be applied blindly in all cases. Sometimes it makes sense to ask:
```java
if (carriage.hasSeatsAvailableWithin(percentReservedBarrier)) ...
```

#### Mock objects
In a unit test, the target object is the object we are testing.

A mock object is a substitute for a neighbour of the target object. To create a mock object, we simply specify how we expect the target object to communicate with its mock neighbour. We call these specifications *expectations*. During the test, the mock objects assert that they have been called as expected.

The neighbours don't need to exist when we are writing the unit test. We can actually use the opportunity of writing the test to think about and define the contracts of those neighbours, defining an interface for each of them. Our mock objects will implement these interfaces. As you develop the rest of the system, you will fill in real implementations for each of these interfaces.

The essential structure of a test is:
* create any required mock objects.
* create any real objects, including the target object.
* specify how you *expect* the mock objects to be called by the target object.
* call the triggering method(s) on the target object.
* assert that any resulting values are valid and that all the expected calls have been made.

<br />
<br />

# Part 2: The process of TDD

## Chapter 4: Kick-starting the test-driven cycle

#### Introduction
Is it recommended to do TDD from the very first moment a project? Yes. Deploying and testing right from the start of a project forces the team to understand how their system fits into the world. Start with "build, deploy and test" on a nonexistent system sounds odd, but it's essential.

The risks of leaving it for later are just too high. Many projects have been cancelled after many months of work because either they could not be reliably deployed or because new features required months of manual testing and the error rates were too high.

Once we have our first test in place, subsequent tests will be much quicker to write.

#### First, test a walking skeleton
First, work out how to build, deploy and test a "walking skeleton", then use that infrastructure to write the acceptance tests for the first meaningful feature. After that, everything will be ready for the team to develop the rest of the system with tdd.

A "walking skeleton" is an implementation of the thinnest possible slice of real functionality that we can automatically build, deploy and test end-to-end. It should include just enough of the automation, the major components, and communication mechanisms to allow us to start working on the first feature.

When building the walking skeleton we don't need much detail, just an overall structure, a picture of what major system components will be needed and how they will communicate. We make the smallest number of decisions we can to kick-start the tdd cycle.

On an ideal situation, the team releases regularly to a real production system so that stakeholders can respond to how well the system meets their needs. Since the team has a thorought set of regressions tests, the team can safely adapt to any changes from the client. No tests are perfect, but in practice a substantial test suite allows us to make major changes safely.

#### Expose uncertainty early
One of the points of approaching a new project with tdd from the very beginning is that you encourage chaos to happen early in the project instead of later in the project.

It is fairly common in projects to leave the testing and the creation of the building and deployment pipeline for a later stage. This means that things move forward rather quickly at the beginning, but they are very likely to become stressful and chaotic when you:
* want to put your components together and realize that they don't talk to each other as they should
* start testing your system manually and discover many issues, which you fix causing other stuff to break
* start writing acceptance tests and realize that it is hard as the system was not designed with testing in mind
* when creating the deployment pipeline, you find out many unexpected obstacles

Starting with tdd first, you take care of all of those uncertainties first, so that the rest of the trip is more stable and predictable. All the mundane but fragile tasks such as deployment and upgrades will have been automated and performed many times so that they "just work". This means that the project will start very slow, as you need to spend an uncertain amount of time:
* thinking about all your components
* designing them in a way that makes them easy to test and
* building the automated pipeline that deploys the system into a production-like environment and runs all the acceptance tests

Those are tricky activities which you want to tackle early in the project.