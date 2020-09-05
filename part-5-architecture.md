# Part V - Architecture

## Chapter 15 - What is Architecture?

Software Architects should be programmers and should continue to
program. The continue taking on programming tasks while guiding the rest
of the team towards a design that maximizes productivity.
- Architects may not program as much as engineers, but they must
  continue doing so. They must experience the pains that their decisions
  cause.

The architecture of a software is the composition, shape, communication
mechanisms between the components that make the software. The purpose is
to facilitate **development, deployment, operation and maintenance**,
optimising to **minimize lifetime maintenance cost** and **maximize
programmer productivity.**
- The strategy is to **leave as many options open as possible**, for as
  long as possible.

The above entails that the *primary role* of architecture is NOT correct
behaviour of the system. Correct behaviour is a critical yet-secondary
priority.
- There are many systems out there that work correctly but that are very
  hard to maintain.

### Facilitate Development

The architecture must make it possible for the teams to organize as they
need without interfering with each other.

Jump ahead to
[Layers and Independent Develop-ability](#layers-and-independent-develop-ability)
to see how this is possible.

#### The Paradox of Small Teams

**Small teams** of say 5 developers can work effectively on a monolithic
system without well-defined components. Furthermore, they will probably
find the strict rules of architecture to reduce their productivity
during the early days of development. This is why so many systems lack
good architecture: They begun with none because the team was small and
did not want the impediment of a superstructure.

#### Large Teams

A system being developed by five different teams, each with seven
developers will not make progress until the system is divided into
well-defined components with stable interfaces.

If no one thinks about the architecture, the teams will naturally
gravitate to a 5-component architecture (one per team). This will likely
not be the best architecture for deployment, operation and maintenance.

### Facilitate Deployment

Ease of deployment of the **whole system with a single action** is often
overlooked, especially during early stages of development, and is a
critical concern of architecture.

The extreme case of neglecting ease of deployment is the blind early
adoption of a *"micro-service architecture"*. At first, the teams may
find it really easy to develop due to the strict boundaries and stable
interfaces, but deployment becomes a major challenge because of the need
for coordinated deploy between services, connection errors, etc.

Had architects considered ease of deployment early on, they might have
decided on fewer services, or a hybrid of network services and in
code-services that are more integrated.

### Facilitate Operation

Operation is seen from 2 optics in this section: **Operation Performance
in Hardware** and **Cognitive load to understand the what the software
does.** Architecture is more related to the second optic that to the
first one.

#### Operation Performance

For reasonably well programmed applications, performance problems can
almost always be solved by throwing more hardware at them. If the
architecture is good, it is rarely the cause of the performance problems
and does not get in the way of solving them.
- People are more expensive than hardware. Unless the performance is
  horrible, the cost of bad performance is typically much lower than the
  cost of hard develop-ability, deployment or maintenance.

Component arrangement to optimize performance should be treated
[as a detail whose decision's can be deferred](#good-architecture-keeping-options-open).
An architecture that maintains proper isolation of its components and
does not assume the means of communication between them makes it easy to
arrange the processing elements in a way that can be tuned for
performance: e.g. easily parallelize-able in different servers or
threads, independently deployable and scalable on different servers.

Jump ahead to
[Vertical layers and operation performance](#vertical-layers-and-operation-performance)
to see how this is possible.


#### Cognitive Load

**Cognitive easiness** of what the software does *is* the critical bit
from the operation point of view because it makes development and
maintainability easier.

### Facilitate Maintenance

Of all the aspects of a software system, maintenance is the most costly.

The main component of maintenance cost is in **speluking** and risk:
- **Speluking:** Digging through the existing software to figure out
  where to add a new feature or to correct a bug.
- **Risk:** convoluted software increases the risk of adding a bug while
  adding a new feature or correcting another defect.

A carefully though-through architecture vastly mitigates this costs and
**illuminates the pathway for future features.**

### Good Architecture: Keeping Options Open

> As we described in an earlier chapter, software has two types of
> value: the value of its behavior and the value of its structure. The
> second of these is the greater of the two because it is this value
> that makes software soft.

On a very high-level, software systems can be divided into 2 elements:
**policies** and **details.**
- **Policies:** business rules and procedures.
- **Details:** things that are necessary for humans and other systems to
  communicate with the policy but that not impact the policy at all
  (e.g. They include IO devices, databases, web systems, servers,
  frameworks, communication protocols, and so forth).

The goal of the architect is to create a shape for the software that
recognizes the policies as the main elements and keeping the details
irrelevant to them.

What are the options that we need to leave open for as much as possible?
*The details that don't matter*. Decisions on this can be delayed and
deferred. The longer you wait to make those decisions, the more
information you have to make the correct decision.

Some examples of leaving the options open:
- Selection of a DB does not need to happen in the early days of
  development. The policy should not care which kind of DB is used (or
  it may even use flat files).
  - This even goes as far as the query language and even the schema are
    technical details that have nothing to do with the policies
    (business rules).
- It is not necessary to choose the web server early. The *web* is a
  delivery mechanism that policy should be unaware of. Furthermore, the
  policy *could* be used in non-web systems without change.
- REST, GraphQL, SOA, micro-services. All of these things are interfaces
  to the outside world that the high-level policy is not concerned
  about. This decisions can be deferred.
- It is not necessary to adopt a dependency injection framework early in
  development, because the high-level policy should not care how
  dependencies are resolved.

Delaying and deferring the details also allows you to **experiment**
with say different databases, API styles or even frameworks to measure
performance and applicability without changing the policy.

What if your company has already committed to a detail? A good architect
pretends that the decision has not been made, and shapes the system such
that those decisions can still be deferred or changed for as long as
possible.

An amazing result of this type of thinking is that our programs become
**device and technology independent.**


## Chapter 16 - Vertical and Horizontal Layers and Independence

This chapter explores the idea of thinking about software systems as
horizontal and vertical layers and shows how that layered thinking
supports the architecture goals presented in the previous chapter:
Operation, Develop-ability, Deploy-ability (and implicitly
maintainability).

### Thinking in Layers

![thinking-in-layers.png](images/part-5/thinking-in-layers.png)

- **Vertical Layers:** related to what the system does (use cases). Use
  cases change for different reasons and different rates from each
  other, therefore, they should be protected from each other.
  - If we wanted to achieve perfect vertical layer decoupling, we need
    to separate the horizontal bits of each use case. For example, the
    *add-order* use case has its UI and DB code the is different from
    the
  - *delete-order* one.
- **Horizontal Layers:** related to the functional purpose of the layer
  in the code (e.g persistence).

### Vertical Layers and Operation Performance

In any application, some use cases are used far more often than others
or have stricter performance requirements. The concept of vertical
separation by use cases enables a powerful idea: if we absolutely needed
to support high throughput for a particular use case, we already have a
natural place where to cut without too much change as the software is
already modularized.

By making the vertical separation is strict without any assumption that
other use-cases are accessible via the same machine, use-cases can be
deployed and scaled independently as *services* / *micro-services*. See
[Decoupling Modes](#decoupling-modes) for more details.

This idea of vertical separation **does not mean** that *micro-services*
are the way to go. The point of this section is to illustrate that a
well though architecture allows us to treat defer decisions about
[Operation Performance](#operation-performance) and **keep our options
open for when we have more information.**


### Layers and Independent Develop-ability

A good horizontal decoupling in which the business rules don't know
about the UI allows different teams (say FE and BE) to work in the UI
and the business rules without affecting each other.

Additionally, if we have good vertical decoupling, we could also arrange
our teams to work on a `use case` basis and have, for example, a team
working on `add Order` that never interferes with the team working on
`deleteOrder`.

**The point is, as long as the horizontal and vertical layers exist, the
system will support the team organizational needs for the company,
regardless if they want to work as feature teams, component teams, layer
teams, some other variation, or if they need to change the team
structure over time.**


### Layers and Independent Deploy-ability

Horizontal layers give us the possibility of, for example, compiling the
UI or persistence layers of a use-case independently from its business
logic. Changing the UI version is just a matter of swapping the jar
file.
- With horizontal layering, you *could* also deploy the FE in different
  servers if you wanted to. Again, a good architecture allows you to
  make this decisions later.

Vertical layers give us a similar possibility, and optionally we could
take it to the point of deploying to different *micro-services* as shown
in the
[Vertical Layers and Operation Performance](#vertical-layers-and-operation-performance)
section. See [Decoupling Modes](#decoupling-modes) for more details.


### The Fear of Duplication Trap

Many architectures gets damaged because of the fear of duplication.

Duplication is generally considered a bad thing in software. However,
there is an important distinctions between 2 types of duplication:
- **True (bad) duplication** happens when the exact same code or logic
  is truly repeated in multiple places and every time a change happens,
  we need to change all places.
- **False (accidental) duplication** is where two apparently duplicated
  sections of code evolve along different paths, change at different
  rates and for different reasons.
  - Even if the code looks the same at some point in time, it will
    diverge over time.

Architectures get damaged when developers unify pieces of accidental
duplication because separating them as they take different paths is
really challenging.

Some examples of common accidental duplication scenarios that you should
avoid unifying:
- **Accidental duplication in vertical layers:** Similar views,
  algorithms, database queries or schemas across use cases.
- **Accidental duplication in horizontal layers:** the data structure of
  a database record near the persistence layer (e.g. the structure that
  represents a book record) is very similar to the data structure of a
  particular UI part (e.g. the view model for a book).
  - **Don't** pass the DB record to the UI. Create a separate view model
    and keep your layer decoupled.

### Decoupling Modes

**"Decoupling modes"** are different levels of actual decoupling we can
achieve by changing how components communicate with each other.

- **Source Level:** All the code is in one project in a single
  executable. The vertical and horizontal layers exist and the
  dependencies are controlled via the source code so that high-level
  components are protected from changes on low-level details. The
  components communicate via direct function calls. This is a well
  designed monolith.
- **Deployment Level:** Some of the layered components have been
  extracted to other projects and are distributed via a jars, gems or
  DLLs. The main program may include the jar and communicate with the
  component via direct function calls, or may run independently on the
  same machine and communicate via inter-process communication, sockets
  or shared memory.
- **Service Level:** Some of the components have been extracted to other
  projects and are deployed independently to potentially different
  servers or virtual machines. Components communicate with each other
  via network calls.

**Which one is best?**

It is hard to know at the beginning of the project and the best option
will change as the project matures. Therefore we need the architecture
to allow us to decide on this **later** without having to re-build the
whole system.

#### The pitfall of micro-services by default

It has become increasingly popular for teams to default to
micro-services since day 1 a project.

Dealing with distributed systems and network service boundaries is
difficult. This is a waste of effort and resources when there is no good
reason to have two uses cases running in different services.

Additionally, the *micro-services by default* mindset tends to make
people think that the vertical layering at the micro-service level is
sufficient and internally the micro-services' code are *big balls of
mud.* In reality, it is likely that the internal code also needs some
vertical layering.


#### Uncle Bob's Preferred Decoupling Mode Strategy

His preferred strategy is to push decoupling up to the point where
**service-level** *could* be performed without too much effort if
necessary.

Initially all the code lives in one project, separated by **source code
level** decoupling. This may be sufficient for the whole lifetime of the
project.

If deployment of development issues arise, he extracts whatever use
cases are causing the issue to **deployment-level** decoupling to fix
the issues.

If performance operational issues appear, he carefully chooses which
components to turn into **services**.

Overtime, if performance needs reduce, he reverts the **service-level**
decoupling to **deployment** or even **source code** level.

Changing decoupling modes is tricky and Uncle Bob is NOT advocating for
systems that can run in any decoupling mode just by changing a
configuration. The point he is making is that the architecture should
make the changes of decoupling mode (forward and backward) *possible.*
