## Anonymous inner classes - "the basics element of understanding lambda expressions"

In Java, anonymous inner classes provide a way to implement classes that may occur only once in an application. For example, in a standard Swing or JavaFX application a number of event handlers are required for keyboard and mouse events. Rather than writing a separate event-handling class for each event, you can write something like this:
```java
JButton testButton = new JButton("Test Button");
testButton.addActionListener(new ActionListener(){
 @Override public void ActionPerformed(ActionEvent ae) {
     System.out.println("Click detected");
 }
}
```
Otherwise, a separate class that implements _**ActionListener**_ is required for each event. By creating the class in place where it is needed, the code is a little easier to read. This is also used with the _**GLFWKeyCallback**_ class - instead of creating a separate _**Input**_ class to provide the functionality of _**GLFWKeyCallback**_, you "create an inner class" with the use of lambda expressions.

The interface of _**ActionListener**_ looks something like:
```java
package java.awt.event;
import java.util.EventListener;

    public interface ActionListener extends EventListener {
        public void actionPerformed(ActionEvent e);
    }
```
The _**ActionListener**_ example is an interface with only one method. With Java SE 8, an interface that follows this pattern is known as a **functional interface**.

**Note**: This type of interface, was previously known as a **Single Abstract Method type (SAM)**.

Using functional interfaces with anonymous inner classes are a common pattern in Java. In addition to the _**EventListener**_ classes, interfaces like _**Runnable**_ and _**Comparator**_ are used in a similar manner.

**Therefore, functional interfaces are leveraged for use with lambda expressions.**

## Lambda expressions
Lambda expressions are an important feature to Java 8. They provide a clear and concise way to represent an interface using an expression. 

Lambda expressions address the bulkiness of anonymous inner classes by converting five lines of code into a single statement. They consist of the following parts:

| argument list | arrow token | body |
| ------------- | ----------- | ---- |
| (int x, int y)|     ->      | x + y|

The argument list refers to the method of the interface which you're trying to use lambda expressions for. For example, if you wanted to acces the `run()` method of the _**Runnable**_ interface, you'd write ` () -> System.out.println("lel");`. Whereas, if you're trying to use the `invoke(long window, int key, int scancode, int mods)` method of the _**GLFWKeyCallbackI**_ interface with lambdas, you'd write: `(long window, int key, int scancode, iny mods) -> {/block of code/}`

The body can be either a single expression or a statement block. In the expression form, the body is simply evaluated and returned. In the block form, the body is evaluated like a method body and a return statement returns control to the caller of the anonymous method.

Now, three different approaches will be shown as how to implement a _**Runnable**_ object and starting it's thread:
Approach one: create and start a thread by creating an anonymous class that implements the _**Runnable**_ interface:
```java
Runnable task1 = new Runnable(){
    @Override
    public void run(){
        System.out.println("Task #1 is running");
    }
};
Thread thread1 = new Thread(task1);
thread1.start();
```

Or pass the anonymous class directly in the thread constructor:
```java
Thread thread1 = new Thread(new Runnable() {
    @Override
    public void run(){
        System.out.println("Task #1 is running");
    }
});
thread1.start();
```

Now, for the part with lambda expressions:
```java
Runnable task2 = () -> { System.out.println("Task #2 is running"); };
new Thread(task2).start();
```

**Lambda**
1.A lambda expression is an anonymous function.
2.A function that doesn't have a name and doesn't belong to any class

Java Method vs Lambda Function
1. Method Name - No Name Because Anonymous class and don't care of the name.
2. Parameter - Parameter
3. Body - Body
4. Return - No Return Type (Java 8 compiler is able to infer the return type)

*Where to use the Lambdas in Java*
1. To create your own *functional Interface*
2. Use the predefined functional interface provided by Java

An interface with only single abstract method is called functional interface(or Single Abstract method interface) for example Runnable, Callable, Action Listener etc...

To Use Function Interface:
Pre Java 8: We create anonymous inner class
Post Java 8: You can use lambda expression instead of anonymous inner classes.