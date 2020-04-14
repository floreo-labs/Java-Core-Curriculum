# Stacks and Queues

# Objectives
- Review Abstract Data Types
- Understand and implement a Stack Data Structure
- Understand and implement a Queue Data Structure

# Resources
- [Oracle Queue Tutorial](https://docs.oracle.com/javase/tutorial/collections/interfaces/queue.html)
- [Stack Api](http://docs.oracle.com/javase/6/docs/api/java/util/Stack.html)

# Lecture

## Abstract Data Types Revisited

Abstract Data Types (ADT's), as described previously, are not concrete classes for data storage, but rather constructs, or ideas for how data may be stored, retrieved, sorted, or removed. A ```Map``` for example, is an Abstract Data Type used to model the storage of data as Key/Value pairs. Although the classes that implement this model might be different, they should share the same behavior, or methods which should be called to implement Map-like behavior. Although a ```HashMap<K, V>``` may be an unordered collection of Key/Value pairs, and a ```TreeMap<K, V>``` may be an ordered collection of Key/Value pairs, ordered by the keys entered, they both implement the methods that all Maps are expected to implement. The same can be said for the ```List``` Abstract Data Type, with such implementations as an ```ArrayList<E>``` (with elements ordered by an index), and ```LinkedList<E>``` (with elements ordered by their relationships with other elements within the list itself).

Stacks and Queues are also Abstract Data Types - and just as Maps and Lists have varying types of data structures that implement their models, so too do Stacks and Queues. 

## Queue

A ```Queue``` is a First-In-First-Out (FIFO) data structure. It allows you to add elements to one end of a list, and remove elements from the opposite end. Queues can be used to provide a list ordered by the moment elements are added or removed to that data type. For example, imagine you are an HR Manager, and you are in charge of maintaining a list of employees by their start date, to determine who should be allowed to retire first:

Employee data can be stored in a ```class``` called ```Employee.java```:

```java
package com.company;

import java.util.Date;

public class Employee {
    private String name;
    private Date startDate;

    public Employee(String name) {
        this.name = name;
        this.startDate = new Date();
    }

    public String getName() {
        return name;
    }

    public Date getStartDate() {
        return startDate;
    }
}
```

You wouldn't want employees hired in 2010 (assuming they're still under 65 years old), to be asked to retire before employees hired in 2005. One way to keep track of this is to create a program that stores each employee in a data structure, the day they get hired, so that people who are hired earlier, can be asked to retire before those who were hired more recently. A data structure we can use to do this is called a ```LinkedList<E>```:

```java
package com.company;

import java.util.*;

public class Main {

    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {

        retirementQueue.offer(new Employee("Danny"));
        retirementQueue.offer(new Employee("Helen"));
        retirementQueue.offer(new Employee("Yojana"));
    }
}
```

### Adding to the Queue

Adding to the queue is simple, and similar to an ```ArrayList<E>```, in that calling the ```.add()``` or ```.offer()``` methods will allow you to add the element to the end of the queue, which makes sense, since every new employee will also be the most recent employee at that given time.

### Heads and Tails

The **Head** of a queue is the oldest entry in the queue, and the **Tail** is the most recent entry. If you want to confirm who the most senior employee is on your queue, you could run the ```.peek()``` method, which will return the head of the queue, without removing it. If you want to both get (return) **AND** retire (remove) the most senior employee, you could use the method ```.poll()```:

```java
package com.company;

import java.util.*;

public class Main {

    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {

        retirementQueue.offer(new Employee("Danny"));
        retirementQueue.offer(new Employee("Helen"));
        retirementQueue.offer(new Employee("Yojana"));
        retirementQueue.offer(new Employee("Old Man Logan"))
        retirementQueue.offer(new Employee("Ridita"));
        retirementQueue.offer(new Employee("Lily"));
        
        retirementQueue.peek();
        
        retirementQueue.poll();
        retirementQueue.poll();
        
    }
}
```

Perhaps someone passed away before they could retire :coffin::cry:, and you wanted to update your queue accordingly - you could use the method ```.remove()```:

```java
package com.company;

import java.util.*;

public class Main {

    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {

        retirementQueue.offer(new Employee("Danny"));
        retirementQueue.offer(new Employee("Helen"));
        retirementQueue.offer(new Employee("Yojana"));
        retirementQueue.offer(new Employee("Old Man Logan"))
        retirementQueue.offer(new Employee("Ridita"));
        retirementQueue.offer(new Employee("Lily"));
        
        Employee deceased = null;
        
        for (Employee e : retirementQueue) {
            if (e.getName().equals("Old Man Logan")) {
                deceased = e;
            }
        }
        retirementQueue.remove(deceased);
    }
}
```

And, like many other data structures in Java, you can find out the size of the queue by calling the ```.size()``` method on the queue object:

```java
package com.company;

import java.util.*;

public class Main {

    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {

        retirementQueue.offer(new Employee("Danny"));
        retirementQueue.offer(new Employee("Helen"));
        retirementQueue.offer(new Employee("Yojana"));
        retirementQueue.offer(new Employee("Old Man Logan"))
        retirementQueue.offer(new Employee("Ridita"));
        retirementQueue.offer(new Employee("Lily"));
        
        System.out.println(retirementQueue.size());
        
        Employee deceased = null;
        
        for (Employee e : retirementQueue) {
            if (e.getName().equals("Old Man Logan")) {
                deceased = e;
            }
        }
        retirementQueue.remove(deceased);
        
        System.out.println(retirementQueue.size());
    }
}
```

## Stack

A ```Stack``` is a Last-In-First-Out (LIFO) data structure. It allows you to add elements to the tail end of a list, and remove elements from that same end. You could think of Stacks the same way HR managers might determine who to let go during a layoff period - those who have been with the company longer have **Seniority**, and the first person to get laid off, is the most recent person to be hired by that company:

```java
package com.company;

import java.util.*;

public class Main {
    private static Stack<Employee> layOffStack = new Stack<>();
    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {
    
        Employee danny = new Employee("Danny");
        Employee helen = new Employee("Helen");
        Employee yojana = new Employee("Yojana");

        retirementQueue.offer(danny);
        layOffStack.push(danny);
        retirementQueue.offer(helen);
        layOffStack.push(helen);
        retirementQueue.offer(yojana);
        layOffStack.push(yojana);

    }
}
```

Great! We've successfully added three new employees to both lists, by calling ```.offer()``` on the ```retirementQueue```, and ```.push()``` on the ```layOffStack()``` stack, which adds the element to the tail of the Stack! However, our company has recently been hit by hard financial times :chart_with_downwards_trend: - and we'll have to lay off an employee, but how do we know who to lay off first? Like a queue, you could always run the ```.peek()``` method to confirm, or call the ```.pop()``` method, which removes the most recent element added to the stack:

```java
package com.company;

import java.util.*;

public class Main {
    private static Stack<Employee> layOffStack = new Stack<>();
    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {
    
        Employee danny = new Employee("Danny");
        Employee helen = new Employee("Helen");
        Employee yojana = new Employee("Yojana");

        retirementQueue.offer(danny);
        layOffStack.push(danny);
        retirementQueue.offer(helen);
        layOffStack.push(helen);
        retirementQueue.offer(yojana);
        layOffStack.push(yojana);
        
        layOffStack.peek();
        
        layOffStack.pop();

    }
}
```

Alright, you followed company policy, and let that employee go. But what about the ```retirementQueue``` - they can't be laid off, and retire from your company as well, right? So, how can we fix this? Simply remove the employee from the ```retirementQueue``` queue that was recently downsized from the ```layOffStack``` stack:

```java
package com.company;

import java.util.*;

public class Main {
    private static Stack<Employee> layOffStack = new Stack<>();
    private static Queue<Employee> retirementQueue = new LinkedList<>();

    public static void main(String[] args) {
    
        Employee danny = new Employee("Danny");
        Employee helen = new Employee("Helen");
        Employee yojana = new Employee("Yojana");

        retirementQueue.offer(danny);
        layOffStack.push(danny);
        retirementQueue.offer(helen);
        layOffStack.push(helen);
        retirementQueue.offer(yojana);
        layOffStack.push(yojana);
        
        layOffStack.peek();
        
        retirementQueue.remove(layOffStack.pop());
    }
}
```

Good work, you've maintained the integrity of your employment retention process, and learned about stacks and queues in the endeavor!

## Try/Catch Blocks for Handling Exceptions

However, as you run the ```.pop()``` method on Stacks, you'll find that if you call the method on an empty stack, you'll encounter something called an **EmptyStackException**. This is normal, and you can avoid it in several ways - the most basic is with a try/catch code block:

```java
Stack<String> myStack = new Stack<>();

myStack.push("Apple");
myStack.push("Blueberry");
myStack.push("Cherry");

int i = 3;
while(i >= 0) {
    try{
        System.out.println(myStack.pop());
    } catch (EmptyStackException e) {
        System.out.println("All empty!");
    }
    i--;
}
```
