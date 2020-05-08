# Clean Code and Software Architecture in Java

## Resources
- [Georgia Tech Intro to Software Architecture](https://www.youtube.com/watch?v=6bX2EPI-BqE)
- [Georgia Tech: Types of Architectural Styles](https://www.youtube.com/watch?v=JLbo9Lvvy5M)
- [What is MVC](https://www.youtube.com/watch?v=pCvZtjoRq1I)

## 3 Rs of Software Architecture


![3 R's of Software Engineering](https://github.com/ryanmcdermott/3rs-of-software-architecture/raw/master/public/software-architecture-pyramid.png)


After 50+ years of software engineering's existence, we haven't settled on an exact definition of what software architecture is. After all, it is the art in computer science -- persistently evading our most determined of efforts to define it. Even still, it's so vital to the fabric of our industry and applications, that it's impossible to ignore.

Despite our lack of agreement, there are a lot of definitions that can help bring us closer to a formalization of software architecture. Perhaps the most notable of such comes from the IEEE:

>"Architecture is the fundamental organization of a system embodied in its components, their relationships to each other, and to the environment, and the principles guiding its design and evolution." [IEEE 1471]

While this definition and others can bring clarity to the elements that make up architecture, it doesn't give us a mental model to use when developing our applications. By looking at 3 particular "ilities" (readability, reusability, and refactorability), we can form a hierarchy of architectural attributes that can give us a framework for thinking about our system's code and architecture. It won't give you an architecture per se, but it will guide you in thinking about what architecture works for your application.

## 1. Readability
Readability is the simplest way of assessing code quality and it's the most straightforward to fix. It is the most obvious thing you see right when you open up a piece of code, and it generally consists of:

* Formatting
* Variable names
* Method names
* Amount of method arguments
* method length (number of lines)
* Nesting levels

* Use meaningful and pronounceable variable/method names. Code is for people, and only incidentally for computers. Naming is the biggest thing that communicates the meaning behind your code.
* Limit your Method arguments to between 1-3. 0 arguments implies you're mutating state or relying on state coming from somewhere else other than your caller. More than 3 arguments is just plain hard to read and refactoring it is difficult because there are so many paths your Method can take the more arguments it has.
* There is no set limit of lines for a Method, as this depends on what particular language you are coding in. The main point is that your Method should do ONE thing, and ONE thing only. If your Method, which calculates the price of an item after taxes, first has to connect to the database, look up the item, get the tax data, and then do the calculation, then it's clearly doing more than one thing. Long Methods typically indicate too much is happening.
* More than two levels of nesting can imply poor performance (in a loop), and it can be especially hard to read in long conditionals. Consider extracting nested logic into separate Methods.

## 2. Reusability
Reusability is the sole reason you are able to read this code, communicate with strangers online, and even program at all. Reusability allows us to express new ideas with little pieces of the past.

That is why reusability is such an essential concept that should guide your software architecture. We commonly think of reusability in terms of DRY (Don't Repeat Yourself). That is one aspect of it -- don't have duplicate code if you can abstract it properly. Reusability goes beyond that though. It's about making clean, simple APIs that make your fellow progammer say, "Yep, I know exactly what that does!" Reusability makes your code a delight to work with, and it means you can ship features faster.

## 3. Refactorability
Code that is refactorable is code that you can change without fear. It's code that you can deploy on a Friday night, and come back to on Monday morning without any concern that your users encountered runtime errors.

Refactorability is about the system as a whole. It's about how your reusable modules connect together like LEGO pieces. If you change your Employee module and somehow it breaks your Reporting module, then you know you have some refactorability issues. Refactorability is the highest piece of the 3 R hierarchy, and it's the hardest to achieve and maintain. There will always be issues with any human system, and code is no different. 

# Writing Clean Code

Our craft of software engineering is just a bit over 50 years old, and we are still learning a lot. When software architecture is as old as architecture itself, maybe then we will have harder rules to follow. For now, let these guidelines serve as a touchstone by which to assess the quality of the Java code that you and your team produce.

One more thing: knowing these won't immediately make you a better software developer, and working with them for many years doesn't mean you won't make mistakes. Every piece of code starts as a first draft, like wet clay getting shaped into its final form. Finally, we chisel away the imperfections when we review it with our peers. Don't beat yourself up for first drafts that need improvement. Beat up the code instead!

## **Variables**
### Use meaningful and pronounceable variable names

**Bad:**
```java
String yyyymmdstr = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

**Good:**
```java
String currentDate = new SimpleDateFormat("YYYY/MM/DD").format(new Date());
```

### Use the same vocabulary for the same type of variable

**Bad:**
```java
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**
```java
getUser();
```

### Use searchable names
We will read more code than we will ever write. It's important that the code we
do write is readable and searchable. By *not* naming variables that end up
being meaningful for understanding our program, we hurt our readers.
Make your names searchable.

**Bad:**
```java
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);

```

**Good:**
```java
// Declare them as capitalized `const` globals.
public static final int MILLISECONDS_IN_A_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);

```

### Use explanatory variables
**Bad:**
```java
String address = "One Infinite Loop, Cupertino 95014";
String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

saveCityZipCode(address.split(cityZipCodeRegex)[0],
                address.split(cityZipCodeRegex)[1]);
```

**Good:**
```java
  String address = "One Infinite Loop, Cupertino 95014";
  String cityZipCodeRegex = "/^[^,\\\\]+[,\\\\\\s]+(.+?)\\s*(\\d{5})?$/";

  String city = address.split(cityZipCodeRegex)[0];
  String zipCode = address.split(cityZipCodeRegex)[1];

  saveCityZipCode(city, zipCode);

```

### Avoid Mental Mapping
Don’t force the reader of your code to translate what the variable means.
Explicit is better than implicit.
**Bad:**
```java
String [] l = {"Austin", "New York", "San Francisco"};

for (int i = 0; i < l.length; i++) {
    String li = l[i];
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    // Wait, what is `$li` for again?
    dispatch(li);
 }
```

**Good:**

```java
String[] locations = {"Austin", "New York", "San Francisco"};

for (String location : locations) {
    doStuff();
    doSomeOtherStuff();
    // ...
    // ...
    // ...
    dispatch(location);
 }
```

### Don't add unneeded context
If your class/object name tells you something, don't repeat that in your
variable name.

**Bad:**
```java
class Car {
  public String carMake = "Honda";
  public String carModel = "Accord";
  public String carColor = "Blue";
}

void paintCar(Car car) {
  car.carColor = "Red";
}
```

**Good:**
```java
class Car {
  public String make = "Honda";
  public String model = "Accord";
  public String color = "Blue";
}

void paintCar(Car car) {
  car.color = "Red";
}
```

## **Methods**
### Methods arguments (2 or fewer ideally)
Limiting the amount of Method parameters is incredibly important because it
makes testing your method easier. Having more than three leads to a
combinatorial explosion where you have to test tons of different cases with
each separate argument.

One or two arguments is the ideal case, and three should be avoided if possible.
Anything more than that should be consolidated. Usually, if you have
more than two arguments then your method is trying to do too much. In cases
where it's not, most of the time a higher-level object will suffice as an
argument.



### Methods should do one thing
This is by far the most important rule in software engineering. When Methods do more than one thing, they are harder to compose, test, and reason about. When you can isolate a Method to just one action, they can be refactored easily and your code will read much cleaner. If you take nothing else away from
this guide other than this, you'll be ahead of many developers.

**Bad:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        Client clientRecord = repository.findOne(client.getId());
        if (clientRecord.isActive()){
            email(client);
        }
    }
}
```

**Good:**
```java
public void emailClients(List<Client> clients) {
    for (Client client : clients) {
        if (isActiveClient(client)) {
            email(client);
        }
    }
}

private boolean isActiveClient(Client client) {
    Client clientRecord = repository.findOne(client.getId());
    return clientRecord.isActive();
}
```

### Method names should say what they do

**Bad:**
```java
private void addToDate(Date date, int month){
    //..
}

Date date = new Date();

// It's hard to to tell from the method name what is added
addToDate(date, 1);
```
**Good:**
```java
private void addMonthToDate(Date date, int month){
    //..
}

Date date = new Date();
addMonthToDate(1, date);
```


### Methods should only be one level of abstraction
When you have more than one level of abstraction your method is usually
doing too much. Splitting up methods leads to reusability and easier
testing.

**Bad:**


**Good:**


### Remove duplicate code
Do your absolute best to avoid duplicate code. Duplicate code is bad because it
means that there's more than one place to alter something if you need to change
some logic.

Imagine if you run a restaurant and you keep track of your inventory: all your
tomatoes, onions, garlic, spices, etc. If you have multiple lists that
you keep this on, then all have to be updated when you serve a dish with
tomatoes in them. If you only have one list, there's only one place to update!

Oftentimes you have duplicate code because you have two or more slightly
different things, that share a lot in common, but their differences force you
to have two or more separate Methods that do much of the same things. Removing
duplicate code means creating an abstraction that can handle this set of
different things with just one Method/module/class.

Getting the abstraction right is critical, that's why you should follow the
SOLID principles laid out in the *Classes* section. Bad abstractions can be
worse than duplicate code, so be careful! Having said this, if you can make
a good abstraction, do it! Don't repeat yourself, otherwise you'll find yourself
updating multiple places anytime you want to change one thing.
