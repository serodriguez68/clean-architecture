# Part II - Starting with the Bricks: Programming Paradigms

> To date, there have been three such paradigms. For reasons we shall
> discuss later, there are unlikely to be any others.

## Chapter 3 - Paradigm Overview

- **Structured Programming:** Removed `goto` statements and replaced
  them with more disciplined `if/then/else` and `do/while/until`
  constructs.
- **Object-Oriented Programming:** Removed `function pointers`.
- **Functional Programming:** driven by immutability removed
  `assignment` of state.

These 3 paradigms were discovered within 10 years (1958 - 1968). Many
decades have passed and no new paradigms have been added. Robert M.
claims that it is likely that these are the only ones we will discover.

Note that all 3 breakthroughs in programming paradigms were achieved by
**removing / restricting** something. Restriction (also referred to by
Martin as *discipline*) is a **good** thing for software programming and
architecture.

## Chapter 4 - Structured Programming

Today we all are structured programmers not by choice. Most modern
languages do not have a `goto` construct.

### Mathematical Proof

- Structured Programming was born out of the idea of being able to
  mathematically prove that a program was correct.
- While trying to come with a program proof framework, *Dijkstra*
  discovered that some type of `goto` statements made programs
  unprovable and labelled them as *harmful*. This idea was widely
  adopted and the `goto` construct disappeared from most languages.
- `goto` was replaced with the `if/then/else` and `do/while/until`
  constructs which didn't make programs mathematically *"unprovable"*.
- The constructs stuck, but the practice of using mathematical proof to
  ensure program correctness is not used because it is too hard.

### Tests

> We show (software) correctness by failing to prove incorrectness,
> despite our best efforts.


- Instead of mathematical proof, we write tests to check if a program is
  **incorrect**. At some point, we deem the program *correct enough* for
  our purposes.
- Proofs of incorrectness (tests) can only be applied to *provable*
  programs. If a program is not provable (due to the use of `goto` for
  example), it cannot be considered correct no matter the amount of
  tests it has.
- Architecture-wise, we can apply restrictive disciplines to make
  modules and components easily testable and therefore easy to prove
  *correct enough.*
  - This is similar in spirit to the restrictions that **Structured
    Programming** imposed in languages, but at a much higher-level.
  - This **architecture restrictive disciplines** is what we will study
    in the chapters to come.

## Chapter 5 - Object Oriented Programming

> The basis of a good architecture is the understanding and application
> of the principles of object-oriented design (OO).

What does *Object Oriented* actually mean? OO languages must support
three things: *encapsulation*, *inheritance* and *polymorphism*.

### OO gave us Encapsulation? (not really)

**Encapsulation** is being able to group a cohesive set of data and
functions and make an only make a *subset* of those visible to the
outside world.

- Encapsulation is not a concept that is exclusive to OO. In fact, C has
  very strong encapsulation.
- Modern OO languages provide **weaker** forms of encapsulation than the
  ones we had in C. For example, Ruby allows you to declare methods as
  `private`, but the language itself provides mechanisms for
  circumventing that. (so does Java and C#)
- As a result, it is up to the programmers to be well-behaved and not
  circumvent encapsulated information.

### OO gave us Inheritance? (sort of)

- Prior to OO, C already had some mechanisms in which you could fake
  inheritance.
- OO did give us more convenient forms of inheritance, but the *idea* of
  having strict supersets of functionality and delegating to those was
  already there.

### OO gave us Polymorphism? (sort of, but it made it safe to use)

**Polymorphism** is the idea of being able to call the same function by
name, but have different implementations depending on the runtime
context (aka Interfaces in the Java world, or Duck typing in Ruby land).

- C had **polymorphism** from the beginning, so it is not really an OO
  concept.
- But OO did gave us a much convenient and **safe** way of achieving
  polymorphism without having to use **function pointers.**
  - **function pointers** are very dangerous. Bugs related to function
    pointer programming errors are very hard to track.

#### Dependency Inversion (this is the important thing)

> **Polymorphism enables Dependency Inversion. Dependency Inversion is a
> very powerful concept that allows us to treat and design other
> software parts as plugins. This is the core of clean architecture.**

![Dependency Inversion][dependency-inversion]

OO gave the programmers the possibility to control the direction of
source code dependencies independently from the direct of execution
(flow of control).

**This has profound architectural implications.** This means we can
**decide** what depends on what By doing the right decisions we can
decouple our code to a point where we:

- Can **compile and deploy** the different modules of the code
  **independently** (e.g. independent gems or jar files).
- If they can be deployed separately, then they can be **developed
  independently** (safe team independence by design).

Martin gives an example of the "Business Rules" code being decoupled
from both the UI and the Database. Both the UI and the Database act as
*plugins* to the business rules.

![dependency-inversion-ui-db-example][dependency-inversion-ui-db-example]

<!-- Images -->
[dependency-inversion]:./images/part-2/dependency-inversion.png
[dependency-inversion-ui-db-example]:./images/part-2/dependency-inversion-ui-db-example.png

## Chapter 6 - Functional Programming


### Immutability and Architecture

All race conditions, deadlocks and concurrent update problems are caused
due to mutable variables. As an architect, understanding how and when
immutability is practicable is a powerful tool to build programs that
are robust under concurrency.

Immutability is practicable if certain compromises are made:

#### Compromise 1: Segregation of mutable and immutable components

Immutable components communicate with mutable ones. Mutable ones store
things in `transactional memory` (memory slots that are protected
against concurrent updates with some type of transaction scheme).
Architects should push as much processing possible into immutable
components and take out mutability as much mutable code as possible from
the immutable ones.

#### Compromise 2: Event Sourcing

This is easier to illustrate with an example:

> **Mutable Bank**: Imagine a banking application that maintains the
> account balances of its customers. It mutates those balances when
> deposit and withdrawal transactions are executed.

> **Immutable Event Sourced Bank**: Instead of storing the account
> balances, we store only the transactions. Whenever anyone wants to
> know the balance of an account, we simply add up all the transactions
> for that account, from the beginning of time. This scheme requires no
> mutable variables.

The Immutable Bank is unsustainable because, over time, the number of
transactions will grow and calculating the balance will become too
expensive. This is why it is said that strict functional programming
requires *infinite storage and processing power.*

However, the benefits of immutability are still valuable to pursue and
we can try to *delay* mutability as much as possible. We can use tricks
like caching calculations every midnight so during the day we only need
to add the transactions from the current day.

In event sourcing, nothing ever gets deleted or updated. Only new
records are created and the current state is the result of projecting
all records.


