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

- Your use cases should not know or care about the way data is
  stored.
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

Performance is architecturally significant, but it can be
entirely encapsulated and separated from the business rules. Getting
data in an out of the data store quickly should be handled outside of
the business rules in the lower level data access mechanisms.
