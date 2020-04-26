- title: Design Patterns
- tags: java, android, app

# Objectives
- Learn common design patterns in Programming


# Lecture
Today we will discuss design patterns for code. Design patterns are patterns that can be used to solve a generic problem.

- [Scala Blog Post](http://www.lihaoyi.com/post/OldDesignPatternsinScala.html)

Though the above linked post talks specifically about Scala, the design patterns are relevant to Java, and any other programming
as well.

Design patterns are utilized to explain a process of manipulating data in a program, with a defined start and end point. 
They are the equivalent of recipes, regarding how data moves through a program.

For example, you are now familiar with a state machine. Specifically, you all have interacted with a Mealy state machine, which
has an output determined by its current state and the value of current inputs. There is also the [Moore state machine](https://en.wikipedia.org/wiki/Moore_machine)
which has outputs determined by only its current state.

## Singleton
An object which will have only a single instance through out the entire life of a program. 
Typically used when you must synchronize access to a critical section of code.
* Advantages - Simple to understand
* Disadvantages - Is not modular, hard to replace in the future, 
* Bobby's personal opinion - I rarely use singletons, unless I have a single entity that must have synchronized access, like a database.
  Every new programmer first learns singletons, abuse it, and some never realize when it should not be used.
  When using a singleton in Java, it is **EXTREMELY** important to realize how your referencse and variables interacts with the singleton. 
  
* How to create: Make a private constructor, and a public static method that returns a static instance of the class.

## Builder
An object which is used to faciliate creating a more complicated object. This pattern allows a user to specify fewer arguments than
are necessary for instantiating an object, and will return the more complicated object with appropriately set default values.
* Advantages - Simplifies complicated code
* Disadvantages - Pain to code, or modify in the future
* How to create: Make a class that has default values for the class to be built, saved in fields. For each field, create a setter.
Add a build() method that instantiates and returns the desired object.

## Factory
An object which defines needed objects, and how they interact, but does not define the implementation of those objects. A factory allows
a programmer to define when certain methods are called on a given object, while the object decides how to perform that task.
* Advantages - flexible, modular, easy to extend
* Disadvantages - Impossible to determine how implementing code operates
* How to create: Make a class that has abstract getters for some fields. Use those getters to interact with concrete objects implmenting
the methods needed for operation.

## Observer
An object that tracks listeners for changes in state, and publishes them to all listeners as necessary. This is a very popular design
pattern to be used in modern programming, and is often found in event-based programming models
* Advantages - modular, flexible, easy to extend, modify, scalable
* Disadvantages - Depending on implementation details, could be hard to debug.
* How to create: For each class that has data that can be observed, make a dynamic list to store all observers for that data. 
  Create a add and remove function to append or remove observers from the list. Whenever data exist that should be sent to the observers,
  call the appropiate method on each observer in the list.
