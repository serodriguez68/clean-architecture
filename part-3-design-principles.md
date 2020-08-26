# Part III - Design Principles

> Good software systems begin with clean code. On the one hand, if the
> bricks aren't well made, the architecture of the building doesn't
> matter much. On the other hand, you can make a substantial mess with
> well-made bricks. This is where the SOLID principles come in.

- SOLID tells us how to arrange functions and data structures into
  classes / modules, and **how those classes / modules should be
  interconnected**. SOLID is therefore applied at the mid-level (class /
  module level).
  - There are other sets of principles for the component level and the
  high-level architecture. We will study these later.
- SOLID is not limited OOP. In the context of SOLID, a "class / module"
  is a grouping of functions and data (all software have this grouping,
  whether it is called a `class` a `module` or something else). We will
  refer to this grouping as class or a module interchangeably in this
  part.
  - For many languages and teams a `module` is coded in a single `source
    file`, but this doesn't have to be the case.
- SOLID goals: produce mid-level software structures that:
  - Tolerate change
  - Are easy to understand
  - Are the basis of components that can be used in many systems (are
    reusable).

#### Executive Summary

- **Single Responsibility:** a module only has one reason to change.
  Problems occur because we put code that different actors depend on
  into close proximity. The SRP says to *separate the code that
  different actors depend on.*
- **Open-Closed Principle:** behaviour of the system can be changed by
  adding new code rather than changing existing one.
- **Liskov Substitution Principle:** interchangeable parts adhere to a
  contract that allows them to be substituted without the user of the
  part having to change.
- **Interface Segregation Principle:** don't depend on things that you
  don't use (i.e. only depend on the interface that you need).
- **Dependency Inversion Principle:** Low-level modules depend on
  high-level ones, by adhering to interfaces the high-level modules
  defines. Not the other way around.

**In the next chapters we discuss the architectural implications of
these principles.**

## Chapter 7 - The Single Responsibility Principle

- SRP is NOT: *"Every module should do one thing".*
  - Note that there is a lower level rule that states that functions
    should only do one thing. That rule still holds, but it is NOT the
    SRP.
- SRP is: *"A module should have one, and only one, reason to change"*
  - *"Reason to change"* can be interpreted as a **group** of *users* or
    *stakeholders* for which the module was built for and that would
    want the module to be changed in the same way. This **group** is
    also referred to as **actor**
  - Another form of SRP: *"A module should be responsible to one, and
    only one, actor."*
  - Another form of SRP: *"Problems occur because we put code that
    different actors depend on into close proximity. The SRP says to
    separate the code that different actors depend on.*


### Violation of SRP Symptom 1: Accidental duplication

Here is an example that illustrates accidental duplication.

![srp-violation-accidental-duplication][srp-violation-accidental-duplication]


### Violation of SRP Symptom 2: Merge Conflicts

An example following the `Employee` class above:
- An engineer working for the CTO decides to edit the `Employee` class
  to modify something related to `#save`.
- Simultaneously, another engineer from another team working for the
  COO's also edits the same class to modify something related to
  `reportHours`.
- Without knowing both engineers are creating a merge conflict. Merge
  conflicts are a risky thing that can affect all **actors** that depend
  on the affected module.


### Solutions to SRP Violations

All solutions involve moving the functions to different classes. Here
are examples of two common approaches.

#### Approach 1: Separate all Methods from Data and Introduce a Facade

![srp-violation-solution-1][srp-violation-solution-1]

#### Approach 2: Use class as Facade

![srp-violation-solution-2][srp-violation-solution-2]

#### Common Objections and Rebuttal

- Objection: "But every class will only contain one function."
- Rebuttal: That is not true. It will contain one (or a few) **public**
  methods, but possibly many private methods. Small interfaces reduce
  blast radius and promote good design.


## Chapter 8 - The Open-Closed Principle

This section presents the OCP as goal: build systems that are easy to
extend without requiring high cost of change.

The interesting bits of this chapter are a series of inter-related ideas
that explain *how* to achieve the goal.

Let's start by clarifying some of the diagram conventions:

- Any type of arrow in the direction A -> B means: A explicitly mentions
  the class name of B. This means that A is vulnerable from changes made
  to B. This also means that B is *protected* from changes made to A.
  Note that this is also known as a *source code dependency.*
  - **Arrows point towards the components that we want to protect from
    change.**
- An arrow with an open head means a *using* relationship. For example:
  A -> B means that A *uses* B to do its work.
- An arrow with a triangular head means *implements* or *inherits*. For
  example: A &#8702; B means A *implements* B.
- Double lines (==) denote a **component** boundary. A **component** is
  a collection of classes / modules that have the same reason to change
  (the single responsibility principle).
- `<I>` are interfaces and `<DS>` are data structures.

The following diagram will help us illustrate the ideas to achieve the
goal. Note that the arrows on the hierarchy diagram on the right only
indicate the direction of protection (i.e. the *using* and *inherit*
relationships don't apply to the hierarchy diagram).


![ocp-components-protection-hierarchy][ocp-components-protection-hierarchy]


### Idea 1: Separate things that should change for different reasons into `components`

Basically apply the *Single Responsibility Principle* at the component
level.


### Idea 2: Create a hierarchy of protection based on the level of the component

Think what should be protected from what and point the arrows in the
direction of the component that needs the most protection. The higher
the level, the more protection it needs.

- Business Rules / Interactors / Use Cases should be high in the
  protection hierarchy.
- Low-level details like views should be the most vulnerable.


### Idea 3: Use dependency inversion to enforce the hierarchy and ensure unidirectional boundaries

In some cases, the flow of execution will not go in the same direction
as the intended dependency hierarchy. Use dependency inversion in those
cases to control the direction of the source code dependency.

In the diagram above, the `Financial Report Generator` needs to use the
`Database` to do its job, going against the intended protection
hierarchy. However, we can introduce an `<Interface>` on the `Financial
Data Mapper` to invert the source code dependency direction.

It is also important to make sure that all the lines cross the
boundaries in the same direction. Dependency Inversion can also be used
to enforce this.


## Chapter 9 - The Liskov Substitution Principle

In the early days, LSP was used as a thinking mechanism to guide the use
of subclass inheritance. **Now it has turned into a much broader
principle that helps define interfaces at the architectural level.**

By *Interface* here we include:
- Static typed Java-style interfaces
- Dynamically typed Ruby style classes that share the same method
  signatures
- Sets of web services that respond to the same REST interface.
- Basically any way of interchanging an algorithm by invoking some
  function with the same name and passing the same structure of
  arguments.

### Symptoms of LSP violations

LSP is complied when the caller of a substitutable function / REST call
does not have to have any knowledge of the *thing* that implements that
function. It should be able to just invoke the function / REST call with
a pre-agreed signature and everything should work.

When LSP is violated, code like this starts to appear in the callers:

```ruby
if something_that_is_violating_lsp.name == 'acme.com'
 # Call a different method or format the parameters differently for acme 
else
 # Call the agreed interface method with the parameters for everyone else 
end
```

This type checking introduces significant complexity to the code base
and should be avoided by trying to stick to the interface and signature
as much as possible...

...however sometimes that is not possible

### A Possible Solution: Configuration Data

Storing configuration information somewhere (database, config file, flat
file) is a great way to deal with non-complying substitutable types
while still avoiding the if statement.

For example:

| URI      | Dispatch Format                                |
|:---------|:-----------------------------------------------|
| acme.com | /pickupAddress/%s/pickupTime/%s/dest/%s        |
| *.*      | /pickupAddress/%s/pickupTime/%s/destination/%s |

## Chapter 10 - The Interface Segregation Principle

### ISP at the programming language level
![isp-in-programming-languages.png][isp-in-programming-languages]

At the programming language level, only **statically typed** languages
are vulnerable to ISP violations. Dynamic languages, don't have to do
recompilation and hence are not vulnerable to this.

> This is the primary reason why dynamic languages create systems that
> are more flexible and less tightly coupled than statically typed
> languages.

### ISP at the architectural level
ISP is something to consider at the architectural level regardless of
the language being used. At the architectural level we are dealing with
depending on other pieces of software like libraries and frameworks.

The general wisdom is:
> Depending on something that carries baggage that you don’t need can
> cause you troubles that you didn't expect.

## Chapter 11 - The Dependency Inversion Principle

The basic version of the DIP tells us that our code should depend on
abstractions and not on concrete implementations.
- In static languages this means that our class should depend on
  `Interfaces`, `Abstract Classes` and other abstractions.
- On dynamic languages, it is harder to point out what a concrete
  implementation is. However, keep in mind that your code should depend
  on the *methods* the other classes implement and not on the objects
  being members of a particular class.

**Why all this?** Because when we depend on a stable abstraction (say a
Java `Interface`) and the interface changes, all concretions that
implement it are guaranteed to be updated. Additionally, we can easily
make changes in a concrete implementation without having to change the
interface or any of the classes that use it.

However, there is more nuance to this since it is impossible to get rid
of all DIP violations. Every software at some point needs to depend on a
concrete implementation. Depending on *non-volatile* concretions like a
Ruby / Java standard class is not a problem. It is when we depend on
*volatile* modules that are in active development that we should depend
on abstractions.

There are 3 specific coding practices that can help us get closer to the
DIP:

### Coding practice 1: Don't refer to volatile concrete classes
- Applies to static and dynamic languages.
- This also strongly constraints the creation of objects and typically
  enforces the use of `Abstract Factories`.

#### `Abstract Factories`
Initializing a new object requires our code to directly depend on the
concrete class of that object. For example:

```ruby
class Application
  def initialize(service: nil)
    # Our code is depending on ConcreteImpl
    @service = service || ConcreteImpl.new
  end
end
```

The `Abstract Factory` pattern helps our high-level code to depend only
on abstractions and creates an **architectural boundary** that separates
the abstract from the concrete.

![abstract-factories][abstract-factories]

### Concrete practice 2: Don't inherit from volatile concrete classes

### Coding practice 3: Don't override concrete functions




[srp-violation-accidental-duplication]: ./images/part-3/srp-violation-accidental-duplication.png
[srp-violation-solution-1]: ./images/part-3/srp-violation-solution-1.png
[srp-violation-solution-2]: ./images/part-3/srp-violation-solution-2.png
[ocp-components-protection-hierarchy]: ./images/part-3/ocp-components-protection-hierarchy.png
[isp-in-programming-languages]: images/part-3/isp-in-programming-languages.png
[abstract-factories]: images/part-3/abstract-factories.png
