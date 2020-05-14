# Morning Exercises

***For each of the following exercises, complete the exercise AND calculate the worst-case runtime in Big-O notation of your solution***

1. Write a function called `wordToArray(String word)` that takes in a string as an argument and returns an array that contains all of the letters of that word.

2. Write a function `truncate(String input, int truncateBy)` to truncate a string if it is longer than the specified number of characters. The function should take two arguments, the first argument being the string and the second argument being the number of characters to truncate the string by. Truncated strings should end with an ellipsis sequence ("..."). If the specified number is large than the length of the string, the entire string should be returned.

3. Write a method that takes a string parameter.  For longer strings, the method returns a new string constructed out of the first three letters of the argument, followed by three periods (`"..."`), followed by the last letter of the argument.

However, if the resulting string is not shorter than the argument, the method should return the argument instead.

For example,

```java
elide("Hello!")               // returns "Hello!"
elide("Hello, world!")        // returns "Hel...!"
elide("That's not my name.")  // returns "Tha...."
```

Remember that `String.substring()` can take two arguments: the start index and the end index.


