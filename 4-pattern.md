Design Patterns
===

If you have THIS situation, then THIS programming technique might help.

System Modelling
---

Better to design first, then implement

* abstractions: what classes will I have
* relationship between classes

Formal technique: UML(Unified Modelling Language)

A UML class is a box

name of class | Vec
--- | ---
<ul><li>fields(optional)</li><li>-private +public</li></ul> | <ul><li>- x:integer</li><li>- y:integer</li></ul>
method | <ul><li>- getX</li><li>+ getY</li></ul>

Singleton Pattern
---

We have a class C and we want only one instance (object) to be created (throughout life of the program)
Example: database connection, error log

See provided example code

Observer Pattern
---

Publish-Subscribe model

Some entities which publishes/generates data: The Subject

Other entities which subscribe to data to react/consume it: The Observer

See provided example code
