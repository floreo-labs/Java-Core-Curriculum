# Dependencies and Dependency Injection

## Objectives
Engineers will learn: 
- what dependency means in programming
- how to identify dependency injection (DI) techniques in Java
- to understand the pros and cons of DI

## Resources 

-  [Tulesko - Why DI](https://www.martinfowler.com/articles/injection.html)
- [Design Patterns Explained – Dependency Injection](https://stackify.com/dependency-injection/)

## Lecture

From Wikipedia: 

"In software engineering, dependency injection is a technique whereby one object (or static method) supplies the dependencies of another object. A dependency is an object that can be used (a service)."

![Dependency visual](https://cdn-media-1.freecodecamp.org/images/1*0P-1JhnUaZeobDUAajIbhA.jpeg)

### Why should we use dependency injection?

Suppose we have a car class which contains objects such as wheels, engine, etc.

When we first think of the building the Car via Java code we may start to think the Car class is responsible for creating all of the objects that make up a car. Now, what if we decide to ditch GoodYear wheel in the future and want to use Yokohama Wheels?

We will need to recreate the car object with a new Yokohama dependency which can get really tedious if we decide to change more things about the car. 

You can think of DI as the middleman in our code who does all the work of creating the preferred wheels object and providing it to the Car class.

It makes our Car class independent from creating the objects of Wheels, Battery, etc.

#### There are basically three types of dependency injection:

1.  **constructor injection:** the dependencies are provided through a class constructor.

```java

public class Car{

    Engine engine;
    //constructor providing dependency (We've been using DI this whole time!!)

    public Car(Engine engine){
        this.engine = engine
    }

}


```


2.  **setter injection:** the client exposes a setter method that the injector uses to inject the dependency.

```java

public class Model{

    int id;
    String name;

    //constructor
    public Model(){

    }

    public void setId(int num){
        this.id = num
    }

    public void setName(String str){
        this.name = str;
    }
}

```

3.  **interface injection:** the dependency provides an injector method that will inject the dependency into any client passed to it. Clients must implement an interface that exposes a [setter method](https://en.wikipedia.org/wiki/Setter_method) that accepts the dependency.

```java

public class Car implements EngineMountable {

    private Engine engine;

    @Override //dependency injection
    public void mount(Engine engine){
        this.engine = engine;
    }

}

```java
public interface EngineMountable {
    void mount(Engine engine);
}

```

All of the examples above are use cases of DI in pure Java (no frameworks or added libraries outside of the JDK).

**So now its the dependency injection's responsibility to:**

1.  Create the objects
2.  Know which classes require those objects
3.  And provide them all those objects

If there is any change in objects, then DI looks into it and it should not concern the class using those objects. This way if the objects change in the future, then its DI's responsibility to provide the appropriate objects to the class.


### Using annotations to describe class dependencies

Different approaches exist to describe the dependencies of a class. The most common approach is to use Java annotations to describe the dependencies directly in the class.

The standard Java annotations for describing the dependencies of a class are defined in the Java Specification Request 330 (JSR330). This specification describes the @Inject and @Named annotations.

The following code shows a class which uses annotations to describe its dependencies.

```java
public class Car {

    @Inject 
    private Engine engine;

    //annotations written inline is fine too
    @Inject private Wheels wheeler;

    @Inject
    public void buildCar(CarParts collection) {
        //Notice that we are able to utilize the field variables without initializing either because of the annotation
        engine.info("Engine Running");
        License license = new Label();
        license.setText("120-jk-22");
        engine.revCar(wheeler);
    }

}

```

It is possible to use dependency injection on static and on non-static fields and methods. Avoiding dependency injection on static fields and methods is a good practice, as it has the following restrictions and can be hard to debug.

#### Inversion of control ---the concept behind DI

Inversion of Control or IoC states that a class should not configure its dependencies statically but should be configured by some other class from outside.

It is the fifth principle of **S.O.L.I.D** 

According to the principles, a class should concentrate on fulfilling its responsibilities and not on creating objects that it requires to fulfill those responsibilities. And that's where **dependency injection** comes into play: it provides the class with the required objects.

*Note: If you want to learn about **SOLID **principles by Uncle Bob then you can head to this [link](https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design#toc-single-responsibility-principle).*

#### Benefits of using DI

1.  Helps in Unit testing.
2.  Boiler plate code is reduced, as initializing of dependencies is done by the injector component.
3.  Extending the application becomes easier.
4.  Helps to enable loose coupling, which is important in scaling applications.

#### Disadvantages of DI

1.  It's a bit complex to learn, and if overused can lead to management issues and other problems. Hence all of the frameworks and tools.
2.  Many compile time errors are pushed to run-time.
3.  Dependency injection frameworks are implemented with reflection or dynamic programming. I'll talk more about this later.

You can implement dependency injection on your own (Pure Vanilla) or use third-party libraries or frameworks.

#### **Libraries and Frameworks that implement DI**

-   [Spring ](https://www.tutorialspoint.com/spring/spring_dependency_injection.htm)(Java)
-   [Dagger ](http://square.github.io/dagger/)(Java and Android)
-   [Google Guice](https://github.com/google/guice) (Java)
