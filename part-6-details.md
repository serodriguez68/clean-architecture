# Part VI - Details

## Chapter 30 - The Database is a detail

The *data model* **is** highly significant to your architecture and most
likely is part of your **entities**. However, the *database tool* to
access your data is a low level detail that is **not** an architectural
element.

### Relational Databases

Relational databases are elegant and mathematically sound technology.
However, while relational tables represent a convenient form of data
storage and access, there is nothing architecturally significant about
arranging data in rows within tables.

- Your use cases should not know or care about the way data is stored.
- Knowledge of the tabular structure should be restricted to the data
  access (aka gateways) layer of the
  [clean architecture](part-5-2-architecture.md#chapter-22---the-clean-architecture).
- Many frameworks allow database rows and tables to be passed around the
  system. This is an architectural error that couples use cases,
  business rules and even the UI to the structure of the data.
- Your use cases should depend on your **data model**, which most likely
  is a set of rich objects or data structures like linked lists, trees,
  hash tables, queues, etc. We almost always map the tabular data into
  other data structures and very rarely work with rows and tables
  directly on our use cases or business rules.


### What about performance?

Performance is architecturally significant, but it can be entirely
encapsulated and separated from the business rules. Getting data in an
out of the data store quickly should be handled outside of the business
rules in the lower level data access mechanisms.


## Chapter 31 - The Web is a detail

It has a long history of pendulum swings from centralizing computing
power (e.g. server rendered pages), to distributing it (e.g. front end
apps) and back. **As architects, we want to shield our business form
those swings and be able to adapt to the swings.**

The philosophical bottom line is **the GUI is a detail and the web is a
GUI.** The web is nothing more than an IO device and hence we should try
to isolate it from our business logic.

The practical bottom line is that with ultra-rich web UIs that
constantly communicate with the servers, aiming for full device
independence in all interactions might be unpractical.

A more practical approach is to reason about the interactions between
the GUI and the application as if they were divided into two types:
- Small and constant interactions that happen to build up and **complete
  the set of inputs** required to run a use case (e.g. drag and drops to
  sort things, async loading of drop down data, autocompletes)
- Business critical interactions that happen when the **complete input
  data is sent to a use case**, the use cases processes the data and
  returns an output to the GUI.

Trying to achieve device independence for the first type of interactions
is probably unpractical and not business critical. On the other hand, it
is easier to do so for the second type and luckily our precious business
logic (use cases) can be separated from the GUI in this case.


## Chapter 32 - Frameworks are details

Frameworks are powerful and useful; but they are not architectures.

Use frameworks but try not to marry them. If you marry into a framework,
make sure that it is a conscious decision and that the team understands
that it will be part of you application forever (until you re-write it).

### An asymmetric marriage

You relationships with frameworks is asymmetric in nature: as engineers
we make an enormous commitment to them by coupling our business logic to
the framework. But, the framework maintainers don't know us and probably
won't steer the framework to solve our problems if problems happen.

#### The risks of marrying into a framework

- Frameworks architecture is often not very clean and tend to violate
  the dependency rule. Your project should follow the structure that
  your code needs, not the one the framework needs.
- Frameworks often require you to tightly couple your business logic
  code to them by subclassing or mixing in framework classes or modules
  into your `Entities` and `Use Cases`. Once you do that, the framework
  is not coming back out of your app.
- The framework might help you with early development of features. But
  once your product matures and outgrows the framework, you will start
  fighting it.
- The framework might evolve into a direction that you don't find
  useful, it might get abandoned or you might want to switch. Once you
  couple to it, you are stuck upgrading to new versions even if you
  don't need them.

### Use frameworks but do not marry them

Treat frameworks as details and limit its use to the outer layers of the
[clean architecture.](part-5-2-architecture.md#chapter-22---the-clean-architecture)
That is, don't let them into your `Use Cases` or `Entities`.

If you *need* to let them in, don't derive a framework class, derive a
proxy instead that you control and that acts as a *plugin* to the
business rules.

### Unavoidable marriages

There are things you will need to marry. Just make sure it is *decision*
and you understand the extend of the commitment. For example, you will
need to mary the standard library for any programming language you use.
