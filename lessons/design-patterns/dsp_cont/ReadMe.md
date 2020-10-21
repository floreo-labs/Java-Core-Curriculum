# Design Patterns in Java

## Objectives
* Students will discover why design patterns are useful in programs
* Students will create a list a constants by using Enums
* Students will explore Abstraction and Instance control with Static Factory Methods
* Students will create single instances with Singletons
* Students will instantiate flexibly with the Builder Pattern

## Resources
* [Sourcemaking: Design Patterns](https://sourcemaking.com/design_patterns)
* [Design Patterns Book](https://en.wikipedia.org/wiki/Design_Patterns)
* [Software Design Patterns](https://en.wikipedia.org/wiki/Software_design_pattern)
* [GrepCode: Java Source Code search](http://grepcode.com)
* [Nested Classes](https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html)

# Lecture

According to the educational website Sourcemaking.com:

*"In software engineering, a design pattern is a general repeatable solution to a commonly occurring problem in software design. A design pattern isn't a finished design that can be transformed directly into code. It is a description or template for how to solve a problem that can be used in many different situations."*

**In short**: Design patterns help developers make their code abstract, concise, and more reusable. 

## Enums

Enums are surprisingly handy, and a great way to manage the state of your program's flow of execution. 

![enums describe state](https://i.pinimg.com/736x/f1/df/5c/f1df5c41f22f2c845180e8da381b1403--the-push-buttons.jpg)

Let's say we want to write a program that tells us what kinds of clothes to wear, depending on the weather. We might first have to come up with a way to store the state of the weather, with boolean values:

```java
private boolean isSunny;
private boolean isRainy;
private boolean isCloudy;
```

Depending on the values of these booleans, we could then run them through an if/else conditional block to see what we could wear:

```java
public void checkWeather(boolean isSunny, boolean isRainy, boolean isCloudy) {
	if(isSunny){
		System.out.println("Wear sunglasses!");
	} else if(isRainy){
		System.out.println("Wear a raincoat!");
	} else if(isCloudy){
		System.out.println("Wear a jacket!");
	} else {
		System.out.println("I don't know what to wear! Ack!!!");
}
```

This will work, but because the order in which these conditions are checked, if isSunny were true, and isCloudy were also true somehow, you might not get the results you'd expect, especially if the order of the code block were to be reversed. There's no singular sorce of truth.

However, if you were only checking one item for the weather, like a String, you could then run it through a switch statement:

```java
public void checkWeather(String weather) {
	switch(weather){
		case "sunny":
			System.out.println("Wear sunglasses!");
			break;
		case "rainy":
			System.out.println("Wear a raincoat!");
			break;
		case "cloudy":
			System.out.println("Wear a jacket!");
			break;
		case default:
			System.out.println("I don't know what to wear! Ack!!!");
			break;
	}
}
```

Great - much more consistent! However, a user could potentially misspell a word, or use the wrong letter case, which might return the wrong result. This is where Enums begin to become useful.

An enum class is essentially a numerically ordered list of static final constants, which make your code more readable and consistent. Let's create one now:

```java
public enum Weather{
	SUNNY,
	RAINY,
	CLOUDY,
	UNKNOWN
}
```

We can reference an enum like this:

```java
Weather currentWeather = Weather.SUNNY;
```

By changing our parameter to be of type `Weather`, we narrow our possible choices for inputs down to four. In fact, we don't even need a `default` for this switch, because the choices are so finite!

But - having to write `Weather` for every choice can be repetitive. A way around this is with a **Static Import**:

```java
import static nyc.floreo.designpatterns.enumpatterns.Weather.*;
```

Static import is a feature in Java which allows members (fields and methods) defined in a class as `public static` to be used in Java code, without specifying the class in which the field is defined. This is particularly useful if a class has a long name....

```java
Weather currentWeather = SUNNY;
```

Now, let's refactor our code to use these new enums:

```java
public void checkWeather(Weather weather) {
	if(weather != null) {
		switch(weather){
			case SUNNY:
				System.out.println("Wear sunglasses!");
				break;
			case RAINY:
				System.out.println("Wear a raincoat!");
				break;
			case CLOUDY:
				System.out.println("Wear a jacket!");
				break;
			case UNKNOWN:
				System.out.println("I don't know what to wear! Ack!!!");
				break;
		}
	}
}
```

Awesome - much cleaner! However, we can make these enums even more robust. Let's add more value to them - by going to the enum class itself, and adding the clothing choices directly inside them:

```java
public enum Weather {
    SUNNY("Wear sunglasses!"),
    RAINY("Wear a raincoat!"),
    CLOUDY("Wear a jacket!"),
    UNKNOWN("I don't know what to wear! Ack!!!");

    private String text;

    private Weather(String text) {
        this.text = text;
    }

    @Override
    public String toString() {
        return text;
    }
}
```

Now, since we've overridden the `toString()` method, we can get our choices directly from the enum:

```java
public void checkWeather(Weather weather) {
	if(weather != null) {
		switch(weather){
			case SUNNY:
				System.out.println(SUNNY.toString());
				break;
			case RAINY:
				System.out.println(RAINY.toString());
				break;
			case CLOUDY:
				System.out.println(CLOUDY.toString());
				break;
			case UNKNOWN:
				System.out.println(UNKNOWN.toString());
				break;
		}
	}
}
```

Although this example appears trivial, these String values can be very useful when logging state to the console, or logcat - or even when sending status updates over the internet.

Enums can be useful when making decisions that can only be made when your program is in the right state - for example, you wouldn't want to push the gas pedal in a car while you we stuck in "park" or "neutral", you'd need to be in "drive" first

```java
	if(Pedal.DOWN && Gear.DRIVE) {
		moveForward();
	} else if(Pedal.DOWN && Gear.PARK) {
		apologizeToRoadTestExaminer();
	}
```

You can also use enums in a variety of ways, like comparing values. Since an enum's value is a constant, you can compare two enums of the same type, and you'd be guaranteed in your comparisons, with no false negatives. This isn't always the case with Strings:

```java
String weather1 = "Sunny";
String weather2 = "Sunny";

/*
This will be true, because the String literal "Sunny" 
is already placed in the "String Pool" by the compiler,
so they both point to the same place in memory:
*/

System.out.println(weather1 == weather2);

String weather3 = "Sunny";
String weather4 = new String("Sunny");

/*
This will be false, because we are explicitly asking 
for a new slot of memory just for the String literal 
referenced by "weather4":
*/

System.out.println(weather3 == weather4);
```

However, with enums, they effectively boil down to static final int primitives, so `SUNNY == SUNNY`, and `SUNNY != RAINY`, since SUNNY is the same as the int 0, and RAINY is the same as int 1, based on their order in the enum list.

```java
Weather weather5 = SUNNY;
Weather weather6 = SUNNY;

/*
This will always be true, since 0 is always equal to 0 in value:
*/
System.out.println(weather5 == weather6);
```

## Static Factory Methods

![static factory](https://i.kinja-img.com/gawker-media/image/upload/s--Jhz7vrKk--/c_scale,f_auto,fl_progressive,q_80,w_800/18to93c6sw0s7jpg.jpg)

Static factory methods are a great way to control how a user can create instances of your classes, or performs tasks. For example, your class might have a number of different constructors you might not want to expose, or perhaps you want to limit the number of instances a user can create:

```java
public class StaticFactoryMethod {

    private static int instanceCount;

    // private constructor:
    private StaticFactoryMethod() {}

    private StaticFactoryMethod(Date date) {
	System.out.println("Logging the date: " + date);
    }

    public static StaticFactoryMethod getNewInstance() {
        if(instanceCount < 5){
            instanceCount++;
            return new StaticFactoryMethod();
        }
        throw new UnsupportedOperationException("Too Many Instances! You can only make up to 5 (five) of them!!!");
    }

    public static StaticFactoryMethod getNewInstanceAndLogDate() {
            return new StaticFactoryMethod(new Date);
    }
}
```

More often than not, static factory methods are used to create objects that follow the *Singleton* pattern - which is to say that you can only make one instance of a particular class:

```java
public class StaticFactoryMethod {

    private static StaticFactoryMethod instanceOfStaticFactoryMethod;

    private StaticFactoryMethod() {};

    public static StaticFactoryMethod getSingleInstance() {
        if(instanceOfStaticFactoryMethod != null) {
            return instanceOfStaticFactoryMethod;
        }
        instanceOfStaticFactoryMethod = new StaticFactoryMethod();
        return instanceOfStaticFactoryMethod;
    }
}
```

By using this pattern, you can limit the number of instances a user can create, and minimize memory usage. You also add a level of abstraction to your code, preventing users from seeing what's actually happening under-the-hood. This is very useful when creating database instances, or when creating things like Retrofit objects, when subclassed.

## Builder Pattern

This is a pattern you may have seen before if you've used the Retrofit Library for Java HTTP requests:

```java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com/")
    .build();
```

You might have written that boilerplate code and wondered what magic was happening when that `.build()` method was finally called. Welp, nothing magical here - Santa Claus isn't real, and builders are just static nested classes that call an enclosing class's private constructor.

![wat](http://i0.kym-cdn.com/photos/images/newsfeed/000/173/580/Wat.jpg?1315930588)

So that's a lot of jargon. Let's break it down.

### Nested Classes

You can place classes inside other classes. There are two forms of this:

* Inner Classes, which can only be instantiated when an instance of the outer class already exists:

```java
public class Outer {
    	public class Inner{
	}
}
```

Instantiation of inner class:

```java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
```

* Nested Classes, which belong to the class (static), and so can be called on the actual class, and not instance objects:

```java
public class Outer {
    	public static class StaticInner{
	}
}
```

Instantiation of static nested class:

```java
Outer.StaticInner staticInner = new Outer.StaticInner();
```

So, that's great. But the question remains: Why, though?

### Constructors can become unweildy

![hard to weild](https://thabto.files.wordpress.com/2013/07/baby-tire.jpg)

Remember when we were bakers, who used Object-Oriented Programming techniques to bake pies? This time, imagine we've moved on to a Pizza shop - and we want to make pizza pies with a variety of toppings:

```java
public class PizzaClassWithConstructors {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchovies;
}
```

If we used a default constructor, we'd make pizzas without any toppings (Bleeeeeccchhhh!!!), so we'll have to make unique constructors:

```java
public class PizzaClassWithConstructors {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchovies;

    public PizzaClassWithConstructors(boolean tomatoSauce) {
        this.tomatoSauce = tomatoSauce;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchovies) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchovies = anchovies;
    }
}
```

That's... a lot. And we haven't even added getters/setters yet:

```java
public class PizzaClassWithConstructors {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchovies;

    public PizzaClassWithConstructors(boolean tomatoSauce) {
        this.tomatoSauce = tomatoSauce;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
    }

    public PizzaClassWithConstructors(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchovies) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchovies = anchovies;
    }

    public boolean isTomatoSauce() {
        return tomatoSauce;
    }

    public void setTomatoSauce(boolean tomatoSauce) {
        this.tomatoSauce = tomatoSauce;
    }

    public boolean isCheese() {
        return cheese;
    }

    public void setCheese(boolean cheese) {
        this.cheese = cheese;
    }

    public int getDiameter() {
        return diameter;
    }

    public void setDiameter(int diameter) {
        this.diameter = diameter;
    }

    public boolean isPepperoni() {
        return pepperoni;
    }

    public void setPepperoni(boolean pepperoni) {
        this.pepperoni = pepperoni;
    }

    public boolean isOnions() {
        return onions;
    }

    public void setOnions(boolean onions) {
        this.onions = onions;
    }

    public boolean isOlives() {
        return olives;
    }

    public void setOlives(boolean olives) {
        this.olives = olives;
    }

    public boolean isArugula() {
        return arugula;
    }

    public void setArugula(boolean arugula) {
        this.arugula = arugula;
    }

    public boolean isArtichokes() {
        return artichokes;
    }

    public void setArtichokes(boolean artichokes) {
        this.artichokes = artichokes;
    }

    public boolean isAnchovies() {
        return anchovies;
    }

    public void setAnchovies(boolean anchovies) {
        this.anchovies = anchovies;
    }
}
```



Plus, these pizzas aren't really made-to-order - yeah, you can get a pepperoni pizza with sauce and cheese, but if you want one with anchovies (🙄🙄🙄), you'd either have to order a pizza with a bunch of unwanted toppings, and pass in "false" for each one you didn't want - or use a setter to effectively throw cold fish on a hot pizza (😨🤢🤮)....

So, what can we do if we want a pizza made just the way we like it, and not have to add to it after it's been made?

![we can build it](http://i0.kym-cdn.com/photos/images/original/000/908/557/8d6.jpg)

### Creating the Builder

First, create the enclosing class, and add a single constructor setting all the fields, and make sure the constructor is *PRIVATE*:

```java
public class PizzaClassWithABuilder {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchoves;

    private PizzaClassWithABuilder(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchoves) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchoves = anchoves;
    }
}
```

Next, create the inner static builder class, adding a parameter for every parameter in the enclosing class, and a constructor setting the `tomatoSauce`, `cheese`, and `diameter` builder fields:

```java
public class PizzaClassWithABuilder {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchoves;

    private PizzaClassWithABuilder(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchoves) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchoves = anchoves;
    }

    // PizzaBuilder inner static class:
    public static class PizzaBuilder {

        private boolean tomatoSauce;
        private boolean cheese;
        private int diameter;
        private boolean pepperoni;
        private boolean onions;
        private boolean olives;
        private boolean arugula;
        private boolean artichokes;
        private boolean anchoves;

        public PizzaBuilder(boolean tomatoSauce, boolean cheese, int diameter) {
            this.tomatoSauce = tomatoSauce;
            this.cheese = cheese;
            this.diameter = diameter;
        }
    }
}
````

Now add setters for each of these inner fields:

```java
public class PizzaClassWithABuilder {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchoves;

    private PizzaClassWithABuilder(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchoves) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchoves = anchoves;
    }

    public static class PizzaBuilder {

        private boolean tomatoSauce;
        private boolean cheese;
        private int diameter;
        private boolean pepperoni;
        private boolean onions;
        private boolean olives;
        private boolean arugula;
        private boolean artichokes;
        private boolean anchoves;

        public PizzaBuilder(boolean tomatoSauce, boolean cheese, int diameter) {
            this.tomatoSauce = tomatoSauce;
            this.cheese = cheese;
            this.diameter = diameter;
        }

        public PizzaBuilder setTomatoSauce(boolean tomatoSauce) {
            this.tomatoSauce = tomatoSauce;
            return this;
        }

        public PizzaBuilder setCheese(boolean cheese) {
            this.cheese = cheese;
            return this;
        }

        public PizzaBuilder setDiameter(int diameter) {
            this.diameter = diameter;
            return this;
        }

        public PizzaBuilder setPepperoni(boolean pepperoni) {
            this.pepperoni = pepperoni;
            return this;
        }

        public PizzaBuilder setOnions(boolean onions) {
            this.onions = onions;
            return this;
        }

        public PizzaBuilder setOlives(boolean olives) {
            this.olives = olives;
            return this;
        }

        public PizzaBuilder setArugula(boolean arugula) {
            this.arugula = arugula;
            return this;
        }

        public PizzaBuilder setArtichokes(boolean artichokes) {
            this.artichokes = artichokes;
            return this;
        }

        public PizzaBuilder setAnchoves(boolean anchoves) {
            this.anchoves = anchoves;
            return this;
        }
    }
}
```

Finally, in order to actually get a Pizza back, we need to create a method that will actually return a pizza, by creating one using all of the arguments passed into the builder, and passing them all into the private constructor:

```java
public class PizzaClassWithABuilder {

    private boolean tomatoSauce;
    private boolean cheese;
    private int diameter;
    private boolean pepperoni;
    private boolean onions;
    private boolean olives;
    private boolean arugula;
    private boolean artichokes;
    private boolean anchoves;

    private PizzaClassWithABuilder(boolean tomatoSauce, boolean cheese, int diameter, boolean pepperoni, boolean onions, boolean olives, boolean arugula, boolean artichokes, boolean anchoves) {
        this.tomatoSauce = tomatoSauce;
        this.cheese = cheese;
        this.diameter = diameter;
        this.pepperoni = pepperoni;
        this.onions = onions;
        this.olives = olives;
        this.arugula = arugula;
        this.artichokes = artichokes;
        this.anchoves = anchoves;
    }

    public static class PizzaBuilder {

        private boolean tomatoSauce;
        private boolean cheese;
        private int diameter;
        private boolean pepperoni;
        private boolean onions;
        private boolean olives;
        private boolean arugula;
        private boolean artichokes;
        private boolean anchoves;

        public PizzaBuilder(boolean tomatoSauce, boolean cheese, int diameter) {
            this.tomatoSauce = tomatoSauce;
            this.cheese = cheese;
            this.diameter = diameter;
        }

        public PizzaBuilder setTomatoSauce(boolean tomatoSauce) {
            this.tomatoSauce = tomatoSauce;
            return this;
        }

        public PizzaBuilder setCheese(boolean cheese) {
            this.cheese = cheese;
            return this;
        }

        public PizzaBuilder setDiameter(int diameter) {
            this.diameter = diameter;
            return this;
        }

        public PizzaBuilder setPepperoni(boolean pepperoni) {
            this.pepperoni = pepperoni;
            return this;
        }

        public PizzaBuilder setOnions(boolean onions) {
            this.onions = onions;
            return this;
        }

        public PizzaBuilder setOlives(boolean olives) {
            this.olives = olives;
            return this;
        }

        public PizzaBuilder setArugula(boolean arugula) {
            this.arugula = arugula;
            return this;
        }

        public PizzaBuilder setArtichokes(boolean artichokes) {
            this.artichokes = artichokes;
            return this;
        }

        public PizzaBuilder setAnchoves(boolean anchoves) {
            this.anchoves = anchoves;
            return this;
        }

	// Method that returns the actual Pizza object:
        public PizzaClassWithABuilder build() {
            return new PizzaClassWithABuilder(
                    tomatoSauce,
                    cheese,
                    diameter,
                    pepperoni,
                    onions,
                    olives,
                    arugula,
                    artichokes,
                    anchoves);
        }
    }
}
```

Now, we can create a very unique, made-to-order pizza:

```java
PizzaClassWithABuilder pizza = new PizzaClassWithABuilder
                .PizzaBuilder(true, true, 12)
                .setPepperoni(true)
                .setArugula(true)
                .build();
```

![pizza](https://s3-media1.fl.yelpcdn.com/bphoto/oacYHuSvtp_Z2gKIH0etUg/o.jpg)
