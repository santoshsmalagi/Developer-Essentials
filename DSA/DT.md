
## Data Type vs Abstract Data Type

The distinction between a data type and an Abstract Data Type (ADT) lies in their level of abstraction and focus:

Data Type:
A data type defines a set of values and the operations that can be performed on those values. It is a concrete classification of data, often directly supported by programming languages. Examples include int (integers), float (floating-point numbers), char (characters), and boolean (true/false values). The definition of a data type often implies details about its storage representation and how operations manipulate that representation.

Abstract Data Type (ADT):
An ADT is a mathematical model or a logical description of a data type, focusing solely on its behavior and the operations that can be performed on it, without specifying any details about its internal implementation or how the data is stored. It defines a contract or an interface for the data, outlining what operations are available and how they behave, but not how they are achieved.

Key Differences:

Implementation Focus:
Data Type: Concerns itself with the concrete representation and implementation details (e.g., how an integer is stored in memory).
ADT: Focuses on the abstract behavior and operations, completely hiding the underlying implementation.
Level of Abstraction:
Data Type: A lower level of abstraction, directly tied to the specifics of a programming language or hardware.
ADT: A higher level of abstraction, providing a conceptual model that can be implemented in various ways.
Examples:
Data Type: int, float, char, String (in Java, a concrete implementation of a sequence of characters).
ADT: Stack (defined by push and pop operations, regardless of whether it's implemented using an array or a linked list), Queue (defined by enqueue and dequeue), List (defined by operations like add, remove, get), Set (defined by operations like add, remove, contains).
