# Part I - Introduction

## Chapter 1 - What is Design and Architecture?

> The bigger lie that developers buy into is the notion that writing messy code makes them go fast in the short term, and just slows them down in the long term.

- **What is the difference between design and architecture?** There is no difference between them.
	- **Architecture** is often referred to as the high-level structure, whereas **design** as the low-level details. However, they both form a continuum. You cannot have one without the other.
- **What is the Goal of architecture?** 
 	- To minimise the human resources required to build and maintain the required system.


## Chapter 2 - A tale of two values
> Every software system provides two different values to the stakeholders: behaviour and structure... Unfortunately, they (developers) often focus on the lesser of the two values, leaving the software system eventually valueless.


#### Behaviour
- Features that make or save money.
- Fixing bugs.

#### Architecture (Structure)
- The ability to change easily.
- The difficulty of the change should be proportional to the *scope* of the change.

#### The Greater Value
> • If you give me a program that works perfectly but is impossible to change, then it won’t work when the requirements change, and I won’t be able to make it work. Therefore the program will become useless. 

> • If you give me a program that does not work but is easy to change, then I can make it work, and keep it working as requirements change. Therefore the program will remain continually useful.

Robert M. proposes the following prioritisation framework:

- 1. Urgent and important > 2. Not urgent and important > 3. Urgent and not important > 4. Not urgent and not important
- Features are urgent, but often not important.
- Architecture is important, but never particularly urgent.
- The mistake that organisations often make is elevating items in position 3, to position 1.  Allowing urgent but not important features take precedence over not urgent but important architectural matters.
