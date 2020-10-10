# Clean Architecture

By:
[Robert Cecil Martin](https://en.wikipedia.org/wiki/Robert_C._Martin)

This is a detailed summary of the Clean Architecture book. The summary
covers every chapter of the book. However, I believe the core ideas of
the book can be picked up by reading a smaller sub-set of chapters.

You will find different "reading pathways" below that will help you
explore or refresh the concepts in the book depending on how deep you
want to go.


## Pathway 1: I want to know everything!

- [Preface](00-preface.md)
- [Part I - Introduction](part-1-introduction.md)
- [Part II - Starting with the Bricks: Programming Paradigms](part-2-starting-with-the-bricks-programming-paradigms.md)
- [Part III - Design Principles](part-3-design-principles.md)
- [Part IV - Component Principles](part-4-component-principles.md)
- Part V - Architecture
  - This is the main part of the book. I've divided it into two
    sub-parts to assist reading.
    - [Subpart 1](part-5-1-architecture.md)
    - [Subpart 2](part-5-2-architecture.md)
- [Part VI - Details](part-6-details.md)


## Pathway 2: I want the core ideas

I believe that the sections presented below distill the most important
ideas.

Note that the book is full of nuanced rich details on **when** and **how
much** to invest in architecture according to your project's
characteristics. It also covers **how** to soundly compromise some of
the architectural boundaries for the sake of simplicity.

Understanding these nuances is very important to develop your
**architectural critical thinking** and is one of the main points of the
book. Unfortunately, none of these detailed ideas are captured in the
plethora of blog posts and youtube videos about Clean Architecture, so I
recommend you read at least the following sections.

- Part II - Starting with the Bricks: Programming Paradigms
  - [Chapter 5 - Understand why the dependency inversion is important](part-2-starting-with-the-bricks-programming-paradigms.md#dependency-inversion-this-is-the-important-thing)

- Part III - Design Principles
  - [Chapter 7 - The Single Responsibility Principle](part-3-design-principles.md#chapter-7---the-single-responsibility-principle)

- Part V - Architecture
  - [Chapter 15 - Good architecture is about keeping options open](part-5-1-architecture.md#good-architecture-keeping-options-open)
  - [Chapter 16 - Vertical and Horizontal Layers and Independence](part-5-1-architecture.md#chapter-16---vertical-and-horizontal-layers-and-independence)
  - [Chapter 17 - The Plugin Architecture - The Foundational Idea of Clean Architecture](part-5-1-architecture.md#the-plugin-architecture---the-foundational-idea-of-clean-architecture)
  - [Chapter 19 - Policy and Level](part-5-1-architecture.md#chapter-19---policy-and-level)
  - [Chapter 20 - Business Rules](part-5-1-architecture.md#chapter-20---business-rules)
  - [Chapter 21 - Screaming Architecture](part-5-1-architecture.md#chapter-21---screaming-architecture)
  - [Chapter 22 - The Clean Architecture](part-5-2-architecture.md#chapter-22---the-clean-architecture)
  - [Chapter 24 - Partial Boundaries](part-5-2-architecture.md#chapter-24---partial-boundaries)
  - [Chapter 25 - The Cost of Implementing VS Ignoring Boundaries](part-5-2-architecture.md#chapter-25---the-cost-of-implementing-vs-ignoring-boundaries)
  - [Chapter 26 - The Main Component, The Ultimate Detail](part-5-2-architecture.md#chapter-26---the-main-component-the-ultimate-detail)
  - [Chapter 27 - Services and Microservices](part-5-2-architecture.md#chapter-27---services-great-and-small)
  - [Chapter 28 - The Test Boundary](part-5-2-architecture.md#chapter-28---the-test-boundary)

- Part VI - Details
  - [Chapter 30 - The Database is a detail](part-6-details.md#chapter-30---the-database-is-a-detail)
  - [Chapter 31 - The Web is a detail](part-6-details.md#chapter-31---the-web-is-a-detail)
  - [Chapter 32 - Frameworks are details](part-6-details.md#chapter-32---frameworks-are-details)
  - [Chapter 33 - Case Study: Video Sales](part-6-details.md#chapter-33---case-study-video-sales)
  - [Chapter 34 - The missing chapter - Actual implementation details of an architecture](part-6-details.md#chapter-34---the-missing-chapter---actual-implementation-details-of-an-architecture)


## Pathway 3: I only want to know about the architectural pattern

In that case just watch
[this 1 hr youtube video](https://www.youtube.com/watch?v=o_TH-Y78tt4)
and read the chapters below.

Note that you will probably be missing an important point of the book as
you won't learn about the bits that develop your **architectural
critical thinking**. If you miss this point, you may (incorrectly) find
the ideas too idealistic, far removed from programming reality and even
dogmatic.

- [Chapter 16 - Vertical and Horizontal Layers and Independence](part-5-1-architecture.md#chapter-16---vertical-and-horizontal-layers-and-independence)
- [Chapter 22 - The Clean Architecture](part-5-2-architecture.md#chapter-22---the-clean-architecture)
- [Chapter 33 - Case Study: Video Sales](part-6-details.md#chapter-33---case-study-video-sales)


## How will I benefit from this book?

**Disclaimer:** this is my opinion. There is no reference about these
topics in the book.

###  If you are just starting out your programming journey

If you are still getting comfortable with building applications using a
framework like Rails, Django, Spring or Laravel, you probably won't
appreciate the full depth of the ideas yet.

I recommend you put this on your reading list and come back later. The
ideas in here won't become dated.


### If have already worked on non-example applications but are just starting to consider software architecture as "a thing"

If it is still early in your career but you have already built at least
one production application on your own or with a small team, you will
likely find that part of the clean architecture ideas map with some
patterns you have developed organically.

Probably, you will also find that ideas like
[Vertical and Horizontal Layers](part-5-1-architecture.md#chapter-16---vertical-and-horizontal-layers-and-independence)
finally give some structure to the burning questions you already have
about how to organise your code.

I recommend you go for **pathway 2** and if you like what you see read
the whole thing.


### If you are proficient at developing software "the framework's way"

If you are very comfortable with one framework, fully embrace it's way
of organising code and have found workarounds to reduce every possible
problem to *"the framework's structure"* (e.g. everything **has to
become** a `model` or `view` or `controller`), you will find the clean
architecture ideas challenging and possibly disruptive to the way you
are used to work.

I very strongly recommend you read it (**pathway 2 or 1**). You will
gain a very different perspective about building software and
representing business logic in code.


### If you are already fighting the framework

You will gain some insight on how to code in a way you actually control
your code (as opposed to the framework controlling it) and on using the
framework as tool in the bits that is really needed.

There may still be a bit of framework wrestling involved, but at least
you will now know how to limit the blast radius of the fight.

Go for **pathway 2.**


### If you have already lost control of a large scale project

If you already work with multiple people maintaining a
[Big Ball of Mud](http://www.laputan.org/mud/), this book will help you
and your team build a common understanding on how to build new features
while keeping them isolated from the *big ball of mud*. Hopefully, it
will also give you some ideas on how to progressively refactor the
important bits from the ball.

Go for **pathway 2 or 1.**


### If you have been bitten by the dark side of micro-services

If you ended up with a **distributed monolith** and are having difficulties
coordinating deployments between services that are coupled, you will
learn that **micro-services** is NOT an architecture, it is a
[Decoupling Mode](part-5-1-architecture.md#decoupling-modes).

The book will help you develop a better critical thinking on when
micro-services make sense and how to organize code inside a monolith
that is ready to be extracted as a micro-service if the operational
conditions demand it.

Go for **pathway 2.**


### If you are a seasoned veteran that just wants to know what is it about

Go for **pathway 3.**


### If like the history of software and computing

The book is packed with interesting context on the history of
programming languages and hardware. Go for **pathway 1.**


## Credits and Final notes

The summary might be full of typos, formatting errors and some spelling
mistakes. I was not optimising for an editorial quality; these are just
the quick notes I took while reading the book. If you want to fix
something please open an issue or send a PR and I'll happily change it.

I've re-used and augmented some of the diagrams in the book to explain
the concepts more clearly. All credit for the diagrams and ideas should
go to Mr. Martin.

Mr. Martin has been involved in multiple controversies lately. My
intentions with this repo are to help others on their software
architecture journey and to provide an objective summary of the book's
content. Please don't associate me in these controversies for
summarizing a book.


