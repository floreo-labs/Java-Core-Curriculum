# Review of Abstract Classes, Lists, and Maps

## Objectives:
- Review abstract classes
- Review ArrayLists
- Review HashMaps

## Vocabulary

|Term|Definition|
|:--:|:---------|
|**Array**|a static data structure that holds a fixed number of values of a single type. The length of an array is established when the array is created. After creation, its length is fixed. This means that an Array's size is immutable.|
|**ArrayList**|a dynamic data structure, meaning items can be added and removed from the list. This means that an ArrayList's size is mutable. A normal array in java is a static data structure, because you are stuck with the initial size of your array at assignment. An ArrayList can change in size by adding or removing elements.|
|**HashMap**|a dynamic data structure, meaning items can be added and removed from the map. Comparable to a dictionary, allowing users to store key/value pairs (like names/phone numbers, or words/definitions).|
|**Interface**|an abstract type that is used to specify a behavior that classes must implement. Interfaces are declared using the interface keyword, and may only contain method signatures, and constant declarations.|
|**Abstract Class**|classes that contain one or more abstract methods. An abstract method is a method that is declared, but contains no implementation. Abstract classes may not be instantiated, and require subclasses to provide implementations for the abstract methods.|

## Resources

### Documents

- [Java docs](http://docs.oracle.com/javase/tutorial/collections/intro/index.html)
- [Abstract Classes](http://docs.oracle.com/javase/tutorial/java/IandI/abstract.html)

### Videos

- [Video: Arrays in Java](http://www.youtube.com/watch?v=Mfacb9T4biQ)
- [Video: ArrayLists](http://www.youtube.com/watch?v=mkCTxtLe7XU)
- [Video: HashMaps](http://www.youtube.com/watch?v=-JOSjIan2g0)
- [Video: Inheritance](http://www.youtube.com/watch?v=wzW-251bGgM)
- [Video: Abstract Classes](http://www.youtube.com/watch?v=CUC522qMGe8)
- [Video: Interfaces](http://www.youtube.com/watch?v=UumX4mQKQlA)

# Lecture

Data structures are an important part of any programming language, especially Java. The Java programming language has a great number of data structures used to store elements/data in a myriad number of ways. Some of the more popular data structures you may encounter are Arrays, ArrayLists, and HashMaps.

### Arrays

Arrays are a data structure where elements of the same data type are assigned in order, by an index. An example of an array of String elements ( String[] ) can be seen in the code snippets below:

```java
// String elements directly assigned to an int array, setting its length to only 5 elements

String[] stringArray = {"John", "Amy", "Theo", "Bharat", "Cristella"};
```

```java
// String array declared, and initialized with an array of exactly 5 int elements

String[] stringArray = new String[5];
```

A programmer who wants to know how many elements have been assigned to an array, or how many indices for elements have been allocated during initialization of this array, may use the ```.length``` property, which may be called on all array objects:

```java
// This will return the length, or the number of elements assigned to the array object

stringArray.length;
```

All elements in an array have an index number. The first element has an index number of 0, and the index of the last element is equal to the length of the array, minus 1.

A programmer may also access or reassign elements within the array, by using the element's index:

```java
// This will allow a user to access the first element of the array, at index 0

stringArray[0];

// This will allow the user to access the last element of the array, regardless of actual index number

stringArray[stringArray.length - 1];

// This will allow the user to access an element in the array, by its index, and assign it a value

stringArray[2] = "Hyun";

```

### Lists, and ArrayLists

An ArrayList behaves very much like an array, in that you may find out the number of elements in it, add elements only of a certain type, where each element has an index, and you can access/change the value of an element by calling it with its index. However, with an array, you cannot increase its size once you have initialized it, without replacing the entire array with a new one, containing both the old and new elements. You can, however, do this with an ArrayList.

```java
// First, you instantiate an ArrayList

ArrayList<String> stringArrayList = new ArrayList<>();
```

You might have seen the keyword ```String``` in angle brackets next to the static type **ArrayList**, as in ```ArrayList<String>``` - this is similar to how you might create an array of type ```String[]```:

```java
String[] stringArray = new String[5];
```

You are essentially stating that the ArrayList you are instatiating will only accept elements of type ```String``` - this is called the ArrayList's **type parameter**. When we do this, under the hood, we are leveraging the power of something called **generics** (we will not cover this today, but soon).

Now that we have this ArrayList object called ```stringArrayList```, let's add elements to it! We can do this with a method called ```.add()```, and we simply pass an argument of type ```String``` into the method's parameter:

```java
stringArrayList.add("Juan-Carlos");
stringArrayList.add("Ashique");
stringArrayList.add("Oksana");
```

The method ```.add()``` will add a new element to the end of the ArrayList.

Now that we've added elements to our ArrayList, let's find out its length. The method we can call for that is ```.size()```:

```java
stringArrayList.size();
```

If we want to update a value at a certain index that already exists, you can use the method ```.set()```:

```java
stringArrayList.set(1, "Pawel");
```

With that method call, we have just updated the value of the element at index 1 from "Ashique", to "Pawel"!

If we want to add an element between two other elements, we can call the method ```.add()```, but with the specific index we want to add our element to:

```java
stringArrayList.add(1, "Selma");
```

This will shift all of the other elements to the next index, while adding the string "Selma" to index 1.

If we want to remove an element, we can call the method ```remove()```, passing in an index to the parameter:

```java
stringArrayList.remove(1);
```

This will shift all of the other elements after the passed-in index, back by one index, effectively removing the element at that index.

ArrayLists also have a very handy method called ```.contains()```, which allows a user to check to see if an element is even in an ArrayList, before having to start a lengthy search with a loop, by returning ```true``` or ```false``` if the element exists somewhere in the ArrayList:

```java
// This will return true

stringArrayList.contains("Ashique");

// This will return false

stringArrayList.contains("Alan");
```

ArrayLists are a very flexible data structure when it comes to adding elements in a numbered order. However, unless you know the index of the element you wish to access, you will most likely have to check every single element to confirm that you are accessing the correct one. Also, the values stored at each index might change with use, and so calling an element by its index might bring unexpected results.

### Maps, and HashMaps

Maps are different from ArrayLists, in that entries in this data structure are stored as key/value pairs. Maps like HashMaps are often called dictionaries in other languages, because they are similar to how one can search for the definition (value) of a word (key) by simply finding the word (key) in the dictionary. Let's create a HashMap:

```java
HashMap<String, String> kindsOfPets = new HashMap<>();
```

This is very similar to how we created an ArrayList object earlier, except that this time, there are two **type parameters** - one for the **key** (the word you will use to get the value you wish to store), and one for the **value** (the actual value you wish to recall when using the key).

Now that we have our HashMap, let's add some values to it, by using the method ```.put()```, and passing in two ```String``` values - one for the key, and one for the value:

```java
kindsOfPets.put("cat", "a domestic feline pet");
kindsOfPets.put("dog", "a domestic canine pet");
kindsOfPets.put("hamster", "a domestic rodent pet");
```

There are several obvious differences between an ArrayList and a HashMap, from what we can already see - there are no indices, meaning you do not add them to a certain location, or a certain order, within the data structure. Also, instead of assigning an element to a particular index, we have made an association between a word, and a definition.

Let's say, after putting a word and its definition into a HashMap, we also want to get a definition (value) out of a HashMap, by using it's word (key). We can simply use the method ```.get()``` on the HashMap object:

```java
// This will retreive the definition "a domestic canine pet"

kindsOfPets.get("dog");
```

There is a catch to using a HashMap - although each value may be different, all of the keys you put into this data structure must be unique. This means that if you use the key "dog" more than once to enter a definition, you won't be adding a second definition, you will instead effectively replace the previous one:

```java
// Original entry
kindsOfPets.put("dog", "a domestic canine pet");

// Replaced entry
kindsOfPets.put("dog", "a person's most hyperbolically bestest friend ever!");
```

If you wish to remove an entry, you may call the ```.remove()``` method on the HashMap:

```java
kindsOfPets.remove("hamster");
```

As with ArrayLists and the method ```.contains()```, HashMaps have a similar method to check if a key already exists within the HashMap, called ```.containsKey()```:

```java
// This will return true

kindsOfPets.containsKey("dog");

// This will return false

kindsOfPets.containsKey("parrot");
```
<!--
We have learned about these data structures before. They are actually just concrete implementations of some abstract classes defined in Java's Collections framework.  

  - ArrayList extends the AbstractList class and implements the following interfaces (List, RandomAccess, Cloneable, and java.io.Serializable).
  - HashMap which extends AbstractMap<K, V> and implements Map<K, V>, Cloneable, and Serializable

We don't really need to dive into such detail about the hierarchy of these structures, but in short A Java class containing data fields and records AND methods that operate on some content may serve as a data structure.

--->

### Abstract classes

Abstract classes are classes which should not be directly instantiated. They must be extended by concrete subclasses which should then be instantiated.

A good analogy to invision is that of Furniture. A chair is a form of furniture, and a couch is a form of furniture, but although it makes sense to make a chair, since a chair is something very concrete, a "furniture" is not concrete, it is abstract, it's a concept.

Let's create an ```abstract``` class in a file called Furniture.java:

```java
public abstract class Furniture {
  private static final int LEG_COUNT = 4;
  
  public static int getLegCount() {
    return LEG_COUNT;
  }
  
  public abstract void textileType();
  
}
```
We can see two methods - a static method, which returns a final constant named LEG\_COUNT, and ```public abstract void textileType();```, a method signature, WITHOUT a method body (no curly brackets). This is intentional - this means that for every subclass that extends furniture, it must OVERRIDE THIS METHOD, AND FILL IT WITH ITS OWN CODE.

Now, let's create a concrete subclass which will extend the Furniture abstract class:

```java
public class Chair extends Furniture {
  
  @Override
    public void textileType() {
        System.out.println("Wood");
    }
}
```

IntelliJ forces you to override this method, and add your own method body. This class has not only inherited the LEG\_COUNT field and method, but it created its own version of the ```textileType()``` method, which was previously only a method signature in the parent class.

Abstract classes can implement interfaces as well by using the ```implement``` keyword. However, because they are abstract, they don't need to implement all methods. The AbstractList interface implements common methods, which allows concrete implementations like ArrayList to be free from the burden of implementing all methods, rather than if they implemented the List interface directly.

Because abstract classes need to have subclasses, they cannot be declared as final. They are also opposites of each other - the ```abstract``` keyword forces a user to extend a class. On the other hand, the ```final``` keyword prevents a class from being extended. In human language terms, abstract signifies incompleteness, while final is used to demonstrate completeness. In short - you cannot make your class ```abstract``` AND ```final``` in Java, as it will result in a compile time error.
