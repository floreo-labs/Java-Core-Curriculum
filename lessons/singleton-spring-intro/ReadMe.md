# A Singleton Patter Deep Dive, Static in Java and Spring

## Objectives
Engineers will: 
- Lear how object instances are managed
- Discover a deeper understanding of Singletons
- Static Keyword in Java

## Resources 

- [Singleton Pattern Explained](https://www.youtube.com/watch?v=hUE_j6q0LTQ)
- [Static in Java](https://www.javatpoint.com/static-keyword-in-java)
- [What is the Spring framework really all about?](https://www.youtube.com/watch?v=gq4S-ovWVlM)

## Lecture

In the previous (Design Patterns)[https://github.com/floreo-labs/Java-Core-Curriculum/blob/master/lessons/design-patterns/ReadMe.md] lesson we learned that Singletons are "An object which will have only a single instance through out the entire life of a program. Typically used when you must synchronize access to a critical section of code."

Another useful defintion would be, a Singleton design pattern is used when you want to have only one instance of a given class.

It is a creational design pattern wherein we deal with the creation of objects.


In object-oriented design, It's very important for some classes to have only one instance. That's because they represent something unique, something that's one of its kind.

Let's see some real-world examples of Singletons from the Java language to understand what that means -

1.  [java.lang.Runtime](https://www.callicoder.com/): Java provides a `Runtime` class that represents the current runtime environment in which an application is running. The application can interface with its runtime environment using this class.

    Since the `Runtime` environment is unique, There should only be one instance of this class.

2.  [java.awt.Desktop](https://www.callicoder.com/): The `Desktop` class allows Java applications to launch a URI or a file with the applications that are registered on the native Desktop like the user's default browser, or mail client.

    The native Desktop and the associated applications are one-of-a-kinds. So there must be only one instance of the `Desktop` class.

Some real world, non-code examples of Singletons would be: 

1. A key to a very important safe

2. A Superbowl ring

3. A state license plate ID

Here's an example in Java where we make a Singleton Universe Object during the Big Bang:

```java
public class Universe {

    //only a single instance of Universe exists 
    private static Universe instance;

    private String name;
    private int numberOfGalaxies;

    private Universe(String name, int numberOfGalaxies) {
        this.name = name;
        this.numberOfGalaxies = numberOfGalaxies;
    }

    //Universe object can only be accessed via getInstance() and only a single instance exists
    public static Universe getInstance(String name, int numberOfGalaxies) {
        if(instance == null) {
            instance = new Universe(name, numberOfGalaxies);
        }
        return instance;
    }



 //  normal getters
    public String getName() {
        return this.name;
    }

    public int getNumberOfGalaxies() {
        return this.numberOfGalaxies;
    }

//    normal setter
    public void setName(String aNewName) {
        this.name = aNewName;
    }

}
```

And then the Universe instance gets used in the BigBang class:

```java
public class BigBang {

    public BigBang(){}

    public Universe makeUniverse(String name, int numberOfGalaxies){
        
        //the lack of new keyword and capitalized class reference tells us this is a static method
        return Universe.getInstance(name, numberOfGalaxies);
    }

}

```
### Now lets review the static keyword in Java

In Java, a *static* member is a member of a class that isn't associated with an instance of a class. Instead, the member belongs to the class itself. As a result, you can access the static member without first creating a class instance.

The two types of static members are static fields and static methods:

- Static field: A field that’s declared with the static keyword, like this:

```java

private static int ballCount;

```
The position of the static keyword is interchangeable with the positions of the *visibility keywords* (private and public, as well as protected). As a result, the following statement works, too:

```java
static private int ballCount;

```

-   Static method: A method declared with the static keyword. Like static fields, static methods are associated with the class itself, not with any particular object created from the class. As a result, you don't have to create an object from a class before you can use static methods defined by the class.

    The best-known static method is main, which is called by the Java runtime to start an application. The main method must be static, which means that applications run in a static context by default.

    One of the basic rules of working with static methods is that you can't access a nonstatic method or field from a static method because the static method doesn't have an instance of the class to use to reference instance methods or fields.

Lets refer back to the Singleton Universe for a static method example:

```java

public static Universe getInstance(String name, int numberOfGalaxies) {
        if(instance == null) {
            instance = new Universe(name, numberOfGalaxies);
        }
        return instance;
    }

```

### How do Singletons tie in with Spring

As your application grows managing your object instantion across multiple classes and methods with just pure Java code becomes a hassle and your source code will soon be prone to errors and [Memory Leaks](https://www.baeldung.com/java-memory-leaks), which is why many developers consider the use of Singletons at all in your code as a code smell. There are very few instances where you would want to write your own singleton objects (maybe for an HTTP client). To solve this problem of managinig your object creation and thus your memory management and software performance, developers made libraries and frameworks like Spring to handle it, hence why DI is such a hallmark of the Spring Framework.

To get a better sense of how Spring helps solve this developer issue, watch this video here: [What is the Spring framework really all about?](https://www.youtube.com/watch?v=gq4S-ovWVlM)

