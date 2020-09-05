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

Different team structures imply different architectural decisions.

#### Small Teams

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

For reasonably well programmed applications, performance problems can
almost always be solved by throwing more hardware at them and often the
architecture has little to do with the performance issue (unless you go
crazy with too many unneeded mirco-services).
- People are more expensive than hardware. Unless the performance is
  horrible, the cost of bad performance is typically much lower than the
  cost of hard develop-ability, deployment or maintenance.

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
