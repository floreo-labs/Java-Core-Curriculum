# Linked Lists, Stacks & Queues

# Objectives

- Learn about linked lists and implementations of their common methods. 
- Review stacks and queues and implementations of their common methods. 
- Identify the runtime complexity of each implemented operation.
- Understand the difference between singly vs. doubly linked lists.
- Understand a circular linked list and its implementation.
- Practice traversing linked lists to search for a target value and/or return the list length.

# Resources

- [Visualgo.net - visualizing data structures](http://visualgo.net/)
- [Big O Cheat Sheet](http://bigocheatsheet.com/)
- [CS50 - Linked Lists](https://study.cs50.net/linked_lists)
- [CS50 - Stacks](https://study.cs50.net/stacks)
- [CS50 - Queues](https://study.cs50.net/queues)

# Lecture

## Linked Lists

Let's look at another linear data structure that addresses some of the limitations of arrays. A **linked list** is a data structure consisting of a group of nodes which together represent a sequence.

Simplest form: each element (we will call it a **node**) of a list is comprising of two items - the **data** and a **reference** (aka "link") to the next node. The last node has a reference to `null`. The entry point into a linked list is called the **head** of the list. It should be noted that head is not a separate node, but the reference to the first node. If the list is empty then the head is a `null` reference.

```java
public class Node {
    Node next;
    Object data;
}
```

A linked list is a dynamic data structure, allocating the needed memory while the program is running. The number of nodes in a list is not fixed and it can grow and shrink on demand. 

One disadvantage of a linked list against an array is that it does not allow direct access to the individual elements. If you want to access a particular item then you have to start at the head and follow the references until you get to that item. Also, they have a tendency to use more memory than an array as the references require extra storage space.

## Traversal

A visit to every node of a data structure is called a **traversal**.

To traverse a singly-linked list, we begin at the first node and follow each next link until we come to the end:

```
// Pseudocode

 current node = first node
 while current node is not null
     (do something with current node's data)
     current node = next node
```

### Exercises:

1. Write a method `listLength(Node list)` that receives the head of a singly linked list and returns the number of nodes in the linked list. What is the worst-case runtime complexity of your algorithm?

```java
Node next;
Object data;

public Node(Object data) { 
	this.value = data;
	this.next = null ;
} 

Node list = new Node("Apple");
list.next = new Node("Orange") 
list.next.next = new Node("Banana");
list.next.next.next = new Node("Carrot");
list.next.next.next.next = new Node("Beet");
 
listLength(list); // returns 5 
``` 

2. Write a method called `searchLinkedList(Node head, Object target)` that receives the head of a linked list and target search value, and returns `true` if the target is in the list, or `false` if the target is not in the list. What is the worst-case runtime complexity of your algorithm?

```java
searchLinkedList(list, "Apple"); // returns true
searchLinkedList(list, "Pear"); // returns false
```

## Types of linked lists

### Singly-linked lists

In a singly-linked list every node contains some data and a link to the next node, which allows to keep the structure.

![singly linked](https://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Singly-linked-list.svg/816px-Singly-linked-list.svg.png)

### Doubly-linked lists

In a doubly linked list, each node contains, besides the next-node link, a second link field pointing to the 'previous' node in the sequence.

![doubly linked](https://upload.wikimedia.org/wikipedia/commons/thumb/5/5e/Doubly-linked-list.svg/1220px-Doubly-linked-list.svg.png)

### Circular linked lists

A **circular linked list** is a variation of linked list in which the first element points to the last element and the last element points to the first element. Both singly -linked lists and doubly-linked lists can be circular.

![circular linked](https://upload.wikimedia.org/wikipedia/commons/thumb/d/df/Circularly-linked-list.svg/700px-Circularly-linked-list.svg.png)

- In a singly-linked list, the `next` pointer of the last node points to the head node.
- In a doubly-linked list, the `next` pointer of the last node points to the head node and the previous pointer of the head node points to the last node.

### Exercises

1. Write a method `insertAfter(Node node, Node newNode)` that inserts `newNode` directly after the existing `node` in a singly-linked list.

2. Modify your `listLength(Node list)` and `searchLinkedList(Node list, Object target)` solutions to handle a circular singly-linked list.

3. Can you traverse a singly-linked list backwards? What about a circular singly-linked list?

## Stacks

A **stack** is a last in, first out (LIFO) data structure that can be visualized like a stack of papers or pancakes. What you put on top (push) is the first item you take off (pop).

Stack interface:
- `void push(E item)` - inserts `item` at the top of the stack, O(1).
- `E pop()` - remove item at the top of the stack, O(1).
- `int size()` - return the number of items.
- `boolean isEmpty()` - return `true` if no items, otherwise `false`.
- `E top()` - return item at the top of the stack without removing it, O(1).
 
## Queues

A **queue** is a first in, first out (FIFO) data structure that can be visualized as a waiting line. You add to the back of the line (enqueue), the front of the line is the first item you take off (dequeue).

Queue interface:
- `void enqueue(E item)` - inserts `item` at the rear of the queue, O(1).
- `E dequeue()` - remove item from front of queue, O(1).
- `int size()` - return the number of items.
- `boolean isEmpty()` - return `true` if no items, otherwise `false`.
- `E front()` - return item at the front of the queue without removing it, O(1).

### Exercises

1.  Write a `moveToTop` function that takes in a queue and a value, and moves that value to the top of the queue.


