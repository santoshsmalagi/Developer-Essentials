An Abstract Data Type (ADT) defines a logical view of data and the operations that can be performed on it, without specifying the underlying implementation details. An Abstract Data Type (ADT) is a mathematical model for data types, defining the behavior and operations associated with a data structure, without specifying the underlying implementation details. It focuses on what a data structure does rather than how it does it.

Key characteristics of ADTs include:

* Abstraction:
    * ADTs hide the internal representation and implementation of the data, allowing users to interact with the data through a defined set of operations without needing to know the specifics of how those operations are performed or how the data is stored.
* Behavioral Definition:
    * They specify the logical properties of the data and the operations that can be performed on it (e.g., adding an element, removing an element, checking if empty).
* Implementation Independence:
    * An ADT can be implemented using various underlying concrete data structures. For example, a "Stack" ADT can be implemented using either an array or a linked list. The user of the Stack ADT does not need to know which implementation is being used.

While there isn't a single "complete" and universally agreed-upon list, as new ADTs can be conceptualized, here are some of the most common and fundamental ADTs:  

#### Linear ADTs:
* List: An ordered collection of elements, allowing insertion, deletion, and access at specific positions.
* Stack: A LIFO (Last-In, First-Out) structure, supporting push (add) and pop (remove) operations.
* Queue: A FIFO (First-In, First-Out) structure, supporting enqueue (add) and dequeue (remove) operations.
* Deque (Double-ended Queue): A queue that allows elements to be added or removed from both ends.
* Priority Queue: A queue where elements are retrieved based on their priority.

#### Non-Linear ADTs:
* Set: A collection of unique, unordered elements.
* Multiset (Bag): A collection of elements where duplicates are allowed and order is not significant.
* Map (Associative Array, Dictionary): A collection of key-value pairs, where each key is unique and maps to a specific value.
* Multimap: Similar to a map, but allows multiple values to be associated with a single key.
* Tree: A hierarchical data structure with a root node and child nodes.
* Binary Tree: A tree where each node has at most two children.
* Binary Search Tree: A binary tree where the left child's value is less than the parent's, and the right child's value is greater.
* Heap: A specialized tree-based data structure that satisfies the heap property (e.g., min-heap or max-heap).
* Graph: A collection of nodes (vertices) and edges connecting them, representing relationships.

#### Other Notable ADTs:
* String: A sequence of characters.
* Container/Collection: A general ADT representing a group of elements, often serving as a supertype for other ADTs.
* Tuple: A fixed-size, ordered collection of elements, typically of different types.

## ADT vs Data Structure

The distinction between an Abstract Data Type (ADT) and a Data Structure lies in their level of abstraction and focus:  

Abstract Data Type (ADT): An ADT is a logical description or mathematical model of a data type. It defines the behavior and operations that can be performed on a set of data, without specifying the implementation details of how that data is stored or those operations are carried out. It focuses on "what" can be done with the data. Examples include Stack, Queue, List, and Map, which define operations like push, pop, enqueue, dequeue, add, remove, get, etc.  

Data Structure: A data structure is a concrete implementation or physical organization of data in memory. It defines how data is stored and arranged to facilitate efficient access and manipulation. It focuses on "how" the data is represented and managed. Examples include arrays, linked lists, trees, and hash tables, which are specific ways to organize data.  

In essence, an ADT is an interface or contract, while a data structure is a concrete realization of that contract. An ADT can be implemented using various data structures. For example, a Stack ADT can be implemented using either an array or a linked list data structure. The ADT specifies the LIFO behavior (Last-In, First-Out), while the underlying data structure determines the specific memory layout and implementation of push and pop operations.

## Data Type vs Data Structure

A data type defines the kind of values a variable can hold and the operations that can be performed on those values. It dictates how data is interpreted by the computer, such as whether it's a whole number, a decimal, a character, or a sequence of characters. Examples of primitive data types include integers (int), floating-point numbers (float), characters (char), and booleans (boolean).  

A data structure is a way of organizing and storing data in a computer so that it can be accessed and modified efficiently. It is a collection of data types arranged in a specific logical or physical layout to facilitate particular operations. Data structures are designed to manage large amounts of data and optimize performance for tasks like searching, sorting, and insertion. Examples of data structures include arrays, linked lists, stacks, queues, trees, and graphs.  

Key Differences:

* Scope: Data types are fundamental building blocks that define the nature of individual pieces of data, while data structures are higher-level constructs that organize and relate multiple pieces of data.
* Purpose: Data types specify what kind of data is being stored, while data structures specify how that data is organized and related to other data.
* Abstraction Level: Data types are more primitive and directly tied to how data is represented in memory. Data structures are more abstract, focusing on the logical relationships between data elements and the operations that can be performed on them.
* Composition: Data structures are often composed of multiple data types. For example, a linked list (a data structure) is made up of nodes, where each node is a composite data type containing data and a reference to the next node.
