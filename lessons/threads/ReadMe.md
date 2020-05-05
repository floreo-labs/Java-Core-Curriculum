## Multithreading and Concurrency in Java

### Objective

Students will understand:

* what processes and threads are in general
* The difference between threads and processes
* the role of the main (aka UI) thread and what to do and NOT do
* the different ways to work with threading in Java


#### Resources
- [Concurrency](http://docs.oracle.com/javase/tutorial/essential/concurrency/)
- [Processes and Threads](https://docs.oracle.com/javase/tutorial/essential/concurrency/procthread.html)
- [Processes and Threads, an Overview](https://www.youtube.com/watch?v=IcIFJ5V3Ibg)
- [Neso Intro to Threads](https://www.youtube.com/watch?v=LOfGJcVnvAk)


### Definitions

**Process** - A running program; all the threads in a process have access to shared memory, and each process running has its own memory. The CPU of a machine handles running several processes at once. A program itself contains multiple processes inside it. Java runtime environment runs as a single process which contains different classes and programs as processes.


**Asynchronous** - When you execute something asynchronously, you can move on to another task before it finishes.

**Synchronous** - When you execute something synchronously, you wait for it to finish before moving on to another task. (Also referred to as blocking)

**Thread** - A thread of execution in a program. The Java Virtual Machine allows an application to have multiple threads of execution running concurrently.

Threads represents one path of execution in a process. Threads can run concurrently, which makes them tricky to reason about.

In Java, "thread" means two different things:

- An instance of class java.lang.Thread
- A thread of execution

An instance of Thread is just… an object. Like any other object in Java, it has variables and methods, and lives and dies on the heap. But a thread of execution is an individual process (a "lightweight" process) that has its own call stack. In Java, there is one thread per call stack—or, to think of it in reverse, one call stack per thread. Every java application has at least one thread – main thread. The main() method, which starts the whole ball rolling, runs in one thread, called (surprisingly) the main thread. If you looked at the main call stack (and you can, any time you get a stack trace from something that happens after main begins, but not within another thread), you'd see that main() is the first method on the stack—the method at the bottom. But as soon as you create a new thread, a new stack materializes and methods called from that thread run in a call stack that's separate from the main() call stack.

Although there are so many other java threads running in background like memory management, system management, signal processing etc. But from application point of view – main is the first java thread and we can create multiple threads from it.

**Race Condition** - Running more than one thread inside the same application does not by itself cause problems. The problems arise when multiple threads access the same resources. For instance the same memory (variables, arrays, or objects), systems (databases, web services etc.) or files.


**Jank**: Any stuttering, flickering or just plain halting that users see when an app (or site) isn't keeping up with the refresh rate. Jank is the result of frames taking too long for a browser to make, and it negatively impacts your users and how they experience your site or app.


### A Thread Instance

A thread in Java begins as an instance of java.lang.Thread. You'll find methods in the Thread class for managing threads, including creating, starting, and pausing them. For the exam, you'll need to know, at a minimum, the following methods:

-   start()
-   yield()
-   sleep()
-   run()

The action happens in the run() method. Think of the code you want to execute in a separate thread as the job to do. In other words, you have some work that needs to be done---say, downloading stock prices in the background while other things are happening in the program---so what you really want is that job to be executed in its own thread. So if the work you want done is the job, the one doing the work (actually executing the job code) is the thread. And the job always starts from a run() method, as follows:

```java
public void run() {
 // your job code goes here
}

```

You always write the code that needs to be run in a separate thread in a run() method. The run() method will call other methods, of course, but the thread of execution---the new call stack---always begins by invoking run(). So where does the run() method go? In one of the two classes you can use to define your thread job. You can define and instantiate a thread in one of two ways

-   Extend the java.lang.Thread class.
-   Implement the `Runnable` interface.

You need to know about both for the exam, although in the real world, you're much more likely to implement Runnable than extend Thread. Extending the Thread class is the easiest, but it's usually not a good OO practice. In that case, you should design a class that implements the Runnable interface, which also leaves your class free to extend some other class.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#implementing-javalangrunnable)Implementing java.lang.Runnable

Implementing the Runnable interface gives you a way to extend any class you like but still define behavior that will be run by a separate thread. It looks like this:

```java
class MyRunnable implements Runnable {
 public void run() {
 System.out.println("Important job running in MyRunnable");
 }
}

```

Regardless of which mechanism you choose, you've now got yourself some code that can be run by a thread of execution. So now let's take a look at instantiating your thread-capable class, and then we'll figure out how to actually get the thing running.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#instantiating-a-thread)Instantiating a Thread

Remember, every thread of execution begins as an instance of class Thread. Regardless of whether your run() method is in a Thread subclass or a Runnable implementation class, you still need a Thread object to do the work. If you extended the Thread class, instantiation is dead simple (we'll look at some additional overloaded constructors in a moment):

```java
MyThread t = new MyThread();

```

If you implement Runnable, instantiation is only slightly less simple. To have code run by a separate thread, you still need a Thread instance. But rather than combining both the thread and the job (the code in the run()method) into one class, you've split it into two classes---the Thread class for the thread-specific code and your Runnable implementation class for your job-that-should-be-run-by-a-thread code. (Another common way to think about this is that the Thread is the "worker," and the Runnable is the "job" to be done.) First, you instantiate your Runnable class:

```java
MyRunnable r = new MyRunnable();

```

Next, you get yourself an instance of java.lang.Thread (somebody has to run your job...), and you give it your job!

```java
Thread t = new Thread(r); // Pass your Runnable to the Thread

```

You can user anonymous inner class and lamda expressions to initialize you threads too.

```java
Thread t = new Thread(new Runnable(){
    @Override
    public void run() {
    }
});

```

Above is a one liner statement to create new Thread, Here we are creating Runnable as Anonymous Class. With Java 8 lambda expressions, we can create Thread in java like below too because Runnable is a functional interface.

```java
Thread t = new Thread(() -> {
       System.out.println("My Runnable");
});
t.start();

```

If you create a thread using the no-arg constructor, the thread will call its own run() method when it's time to start working. That's exactly what you want when you extend Thread, but when you use `Runnable`, you need to tell the new thread to use your `run()` method rather than its own. The `Runnable` you pass to the Thread constructor is called the target or the target Runnable. You can pass a single Runnable instance to multiple Thread objects so that the same Runnable becomes the target of multiple threads, as follows:

```java
public class TestThreads {
 public static void main (String [] args) {
 MyRunnable r = new MyRunnable();
 Thread foo = new Thread(r);
 Thread bar = new Thread(r);
 Thread bat = new Thread(r);
 }
}

```

Giving the same target to multiple threads means that several threads of execution will be running the very same job (and that the same job will be done multiple times). there are other overloaded constructors in class Thread. The constructors we care about are

-   Thread()
-   Thread(Runnable target)
-   Thread(Runnable target, String name)
-   Thread(String name)

So now you've made yourself a Thread instance, and it knows which `run()` method to call. But nothing is happening yet. At this point, all we've got is a plain old Java object of type Thread. It is not yet a thread of execution. To get an actual thread---a new call stack---we still have to start the thread. When a thread has been instantiated but not started (in other words, the start() method has not been invoked on the Thread instance), the thread is said to be in the new state. At this stage, the thread is not yet considered alive. Once the start() method is called, the thread is considered alive (even though the run() method may not have actually started executing yet). A thread is considered dead (no longer alive) after the run() method completes. The isAlive() method is the best way to determine if a thread has been started but has not yet completed its run() method. (Note: The getState() method is very useful for debugging, but you won't have to know it for the exam.)

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#starting-a-thread)Starting a Thread

Now it's time to get the whole thread thing happening---to launch a new call stack.

```
t.start();

```

Prior to calling start() on a Thread instance, the thread is said to be in the new state, as we said. The new state means you have a Thread object but you don't yet have a true thread:

what happens after you call start()?

-   A new thread of execution starts (with a new call stack).
-   The thread moves from the new state to the runnable state.
-   When the thread gets a chance to execute, its target run() method will run.

Be sure you remember the following: You start a Thread, not a Runnable. You call start() on a Thread instance, not on a Runnable instance. The following example demonstrates what we've covered so far---defining, instantiating, and starting a thread:

```java
class FooRunnable implements Runnable {
 public void run() {
 for(int x = 1; x < 6; x++) {
 System.out.println("Runnable running");
 }
 }
}
public class TestThreads {
 public static void main (String [] args) {
 FooRunnable r = new FooRunnable();
 Thread t = new Thread(r);
 t.start();
 }
}

```

Running the preceding code prints out exactly what you'd expect:

```
% java TestThreads
Runnable running
Runnable running
Runnable running
Runnable running
Runnable running

```

There's nothing special about the run() method as far as Java is concerned. Like main(), it just happens to be the name (and signature) of the method that the new thread knows to invoke. So if you see code that calls the run() method on a Runnable (or even on a Thread instance), that's perfectly legal. But it doesn't mean the run() method will run in a separate thread! Calling a run() method directly just means you're invoking a method from whatever thread is currently executing, and the run() method goes onto the current call stack rather than at the beginning of a new call stack. The following code does not start a new thread of execution:

```java
Thread t = new Thread();
t.run(); // Legal, but does not start a new thread

```

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#what-happens-if-we-start-multiple-threads)What happens if we start multiple threads?

We'll run a simple example in a moment, but first we need to know how to print out which thread is executing. We can use the `getName()` method of class Thread and have each `Runnable` print out the name of the thread executing that `Runnable` object's run() method. The following example instantiates a thread and gives it a name, and then the name is printed out from the run() method:

```java
class NameRunnable implements Runnable {
 public void run() {
 System.out.println("NameRunnable running");
 System.out.println("Run by "
 + Thread.currentThread().getName());
 }
}
public class NameThread {
 public static void main (String [] args) {
 NameRunnable nr = new NameRunnable();
 Thread t = new Thread(nr);
 t.setName("Fred");
 t.start();
 }
}

```

Running this code produces the following extra-special output:

```
% java NameThread
NameRunnable running
Run by Fred

```

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#multithreading)Multithreading

Multithreading refers to two or more threads executing concurrently in a single program. A computer single core processor can execute only one thread at a time and time slicing is the OS feature to share processor time between different processes and threads. There are two types of threads in an application -- user thread and daemon thread. When we start an application, main is the first user thread created and we can create multiple user threads as well as daemon threads. When all the user threads are executed, JVM terminates the program.

We can set different priorities to different Threads but it doesn't guarantee that higher priority thread will execute first than lower priority thread. Thread scheduler is the part of Operating System implementation and when a Thread is started, it's execution is controlled by Thread Scheduler and JVM doesn't have any control on it's execution. We can create Threads by either implementing Runnable interface or by extending Thread Class.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#starting-and-running-multiple-threads)Starting and Running Multiple Threads:

Now we'll do more. The following code creates a single Runnable instance and three Thread instances. All three Thread instances get the same Runnable instance, and each thread is given a unique name. Finally, all three threads are started by invoking start() on the Thread instances.

```java
class NameRunnable implements Runnable {
 public void run() {
 for (int x = 1; x <= 3; x++) {
 System.out.println("Run by "
 + Thread.currentThread().getName()
 + ", x is " + x);
 }
 }
}
public class ManyNames {
 public static void main(String [] args) {
 // Make one Runnable
 NameRunnable nr = new NameRunnable();
 Thread one = new Thread(nr);
 Thread two = new Thread(nr);
 Thread three = new Thread(nr);
 one.setName("Fred");
 two.setName("Lucy");
 three.setName("Ricky");
 one.start();
 two.start();
 three.start();
 }
}

```

Running this code might produce the following:

```
% java ManyNames
Run by Fred, x is 1
Run by Fred, x is 2
Run by Fred, x is 3
Run by Lucy, x is 1
Run by Lucy, x is 2
Run by Lucy, x is 3
Run by Ricky, x is 1
Run by Ricky, x is 2
Run by Ricky, x is 3

```

Note:

-   Each thread will start, and each thread will run to completion. Within each thread, things will happen in a predictable order. But the actions of different threads can mix in unpredictable ways. If you run the program multiple times or on multiple machines, you may see different output. Even if you don't see different output, you need to realize that the behavior you see is not guaranteed.
-   A thread is done being a thread when its target run() method completes. When a thread completes its run() method, the thread ceases to be a thread of execution. The stack for that thread dissolves, and the thread is considered dead.
-   Once a thread has been started, it can never be started again. If you have a reference to a Thread and you call start(), it's started. If you call `start()` a second time, it will cause an exception (an `IllegalThreadStateException`, which is a kind of `RuntimeException`) Only a new thread can be started, and then only once. A runnable thread or a dead thread cannot be restarted.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#java-thread-benefits)Java Thread Benefits

1.  Java Threads are lightweight compared to processes, it takes less time and resource to create a thread.
2.  Threads share their parent process data and code
3.  Context switching between threads is usually less expensive than between processes.
4.  Thread intercommunication is relatively easy than process communication.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#more-thread-example--implementing-runnable-interface)More Thread Example -- implementing Runnable interface

```java
public class HeavyWorkRunnable implements Runnable {

    @Override
    public void run() {
        System.out.println("Doing heavy processing - START "+Thread.currentThread().getName());
        try {
            Thread.sleep(1000);
            //Get database connection, delete unused data from DB
            doDBProcessing();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Doing heavy processing - END "+Thread.currentThread().getName());
    }

    private void doDBProcessing() throws InterruptedException {
        Thread.sleep(5000);
    }

}

```

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#more-thread-example--extending-thread-class)More Thread Example -- extending Thread class

We can extend java.lang.Thread class to create our own java thread class and override run() method. Then we can create it's object and call start() method to execute our custom java thread class run method.

Here is a simple java thread example showing how to extend Thread class.

```java
public class MyThread extends Thread {

    public MyThread(String name) {
        super(name);
    }

    @Override
    public void run() {
        System.out.println("MyThread - START "+Thread.currentThread().getName());
        try {
            Thread.sleep(1000);
            //Get database connection, delete unused data from DB
            doDBProcessing();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("MyThread - END "+Thread.currentThread().getName());
    }

    private void doDBProcessing() throws InterruptedException {
        Thread.sleep(5000);
    }

}

```

Here is a test program showing how to create a java thread and execute it.

```java
public class ThreadRunExample {

    public static void main(String[] args){
        Thread t1 = new Thread(new HeavyWorkRunnable(), "t1");
        Thread t2 = new Thread(new HeavyWorkRunnable(), "t2");
        System.out.println("Starting Runnable threads");
        t1.start();
        t2.start();
        System.out.println("Runnable Threads has been started");
        Thread t3 = new MyThread("t3");
        Thread t4 = new MyThread("t4");
        System.out.println("Starting MyThreads");
        t3.start();
        t4.start();
        System.out.println("MyThreads has been started");

    }
}

```

Output of the above java thread example program is:

```
Starting Runnable threads
Runnable Threads has been started
Doing heavy processing - START t1
Doing heavy processing - START t2
Starting MyThreads
MyThread - START Thread-0
MyThreads has been started
MyThread - START Thread-1
Doing heavy processing - END t2
MyThread - END Thread-1
MyThread - END Thread-0
Doing heavy processing - END t1

```

Once we start any thread, it's execution depends on the OS implementation of time slicing and we can't control their execution. However we can set threads priority but even then it doesn't guarantee that higher priority thread will be executed first.

Run the above program multiple times and you will see that there is no pattern of threads start and end.

### [](https://github.com/shoikot/shoikot.github.io/wiki/Threads#runnable-vs-thread)Runnable vs Thread

If your class provides more functionality rather than just running as Thread, you should implement `Runnable` interface to provide a way to run it as Thread. If your class only goal is to run as Thread, you can extend Thread class.

Implementing `Runnable` is preferred because java supports implementing multiple interfaces. If you extend Thread class, you can't extend any other classes.

Tip: As you have noticed that thread doesn't return any value but what if we want our thread to do some processing and then return the result to our client program, check out Java Callable Future. From Java 8 onward , `Runnable` is a functional interface and we can use lambda expressions to provide it's implementation rather than using anonymous class.

