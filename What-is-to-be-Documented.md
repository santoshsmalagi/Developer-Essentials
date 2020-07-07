# What to document

### Documenting C++ Code
By writing documentation blocks for all public or protected C++ components (namespaces, types, methods, functions, and constants).

### File Comments
Start each file with license boilerplate.File comments describe the contents of a file. If a file declares, implements, or tests exactly one abstraction that is documented by a comment at the point of declaration, file comments are not required. All other files must have file comments.

#### .cc, .C or .c file comments
#### .h, .hpp file comments
If a .h declares multiple abstractions, the file-level comment should broadly describe the contents of the file, and how the abstractions are related. A 1 or 2 sentence file-level comment may be sufficient. The detailed documentation about individual abstractions belongs with those abstractions, not at the file level. Do not duplicate comments in both the .h and the .cc. Duplicated comments diverge.

### Class Comments
Every non-obvious class or struct declaration should have an accompanying comment that describes what it is for and how it should be used.

```C++
/ Iterates over the contents of a GargantuanTable.
// Example:
//    GargantuanTableIterator* iter = table->NewIterator();
//    for (iter->Seek("foo"); !iter->done(); iter->Next()) {
//      process(iter->key(), iter->value());
//    }
//    delete iter;
class GargantuanTableIterator {
  ...
};
```

The class comment should provide the reader with enough information to know how and when to use the class, as well as any additional considerations necessary to correctly use the class. Document the synchronization assumptions the class makes, if any. If an instance of the class can be accessed by multiple threads, take extra care to document the rules and invariants surrounding multithreaded use.  

The class comment is often a good place for a small example code snippet demonstrating a simple and focused usage of the class. When sufficiently separated (e.g., .h and .cc files), comments describing the use of the class should go together with its interface definition; comments about the class operation and implementation should accompany the implementation of the class's methods.

### Functions
Declaration comments describe use of the function (when it is non-obvious); comments at the definition of a function describe operation.

#### Function Declarations
Almost every function declaration should have comments immediately preceding it that describe what the function does and how to use it. These comments may be omitted only if the function is simple and obvious (e.g., simple accessors for obvious properties of the class). These comments should open with descriptive verbs in the indicative mood ("Opens the file") rather than verbs in the imperative ("Open the file").  

The comment describes the function; it does not tell the function what to do. In general, these comments do not describe how the function performs its task. Instead, that should be left to comments in the function definition.

Types of things to mention in comments at the function declaration:
* What the inputs and outputs are.
* For class member functions: whether the object remembers reference arguments beyond the duration of the method call, and whether it will free them or not.
* If the function allocates memory that the caller must free.
* Whether any of the arguments can be a null pointer.
* If there are any performance implications of how a function is used.
* If the function is re-entrant. What are its synchronization assumptions?

Here is an example:
```C++
// Returns an iterator for this table.  It is the client's
// responsibility to delete the iterator when it is done with it,
// and it must not use the iterator once the GargantuanTable object
// on which the iterator was created has been deleted.
//
// The iterator is initially positioned at the beginning of the table.
//
// This method is equivalent to:
//    Iterator* iter = table->NewIterator();
//    iter->Seek("");
//    return iter;
// If you are going to immediately seek to another place in the
// returned iterator, it will be faster to use NewIterator()
// and avoid the extra seek.
Iterator* GetIterator() const;
```

However, do not be unnecessarily verbose or state the completely obvious. When documenting function overrides, focus on the specifics of the override itself, rather than repeating the comment from the overridden function. In many of these cases, the override needs no additional documentation and thus no comment is required.  

When commenting constructors and destructors, remember that the person reading your code knows what constructors and destructors are for, so comments that just say something like "destroys this object" are not useful. Document what constructors do with their arguments (for example, if they take ownership of pointers), and what cleanup the destructor does. If this is trivial, just skip the comment. It is quite common for destructors not to have a header comment.  
Function Definitions

If there is anything tricky about how a function does its job, the function definition should have an explanatory comment. For example, in the definition comment you might describe any coding tricks you use, give an overview of the steps you go through, or explain why you chose to implement the function in the way you did rather than using a viable alternative. For instance, you might mention why it must acquire a lock for the first half of the function but why it is not needed for the second half.  

Note you should not just repeat the comments given with the function declaration, in the .h file or wherever. It's okay to recapitulate briefly what the function does, but the focus of the comments should be on how it does it.  

### Variable Comments
In general the actual name of the variable should be descriptive enough to give a good idea of what the variable is used for. In certain cases, more comments are required.

#### Class Data Members
The purpose of each class data member (also called an instance variable or member variable) must be clear. If there are any invariants (special values, relationships between members, lifetime requirements) not clearly expressed by the type and name, they must be commented. However, if the type and name suffice (int num_events_;), no comment is needed.  

In particular, add comments to describe the existence and meaning of sentinel values, such as nullptr or -1, when they are not obvious. For example:

```C
private:
 // Used to bounds-check table accesses. -1 means
 // that we don't yet know how many entries the table has.
 int num_total_entries_;
```

#### Global Variables
All global variables should have a comment describing what they are, what they are used for, and (if unclear) why it needs to be global. For example:

```C
// The total number of tests cases that we run through in this regression test.
const int kNumTestCases = 6;
```

### Useful Links
[Documenting C++ Code](https://developer.lsst.io/cpp/api-docs.html)  
[Where do I start with C++ documentation?](https://writing.stackexchange.com/questions/36232/where-do-i-start-with-c-documentation)  
[C++ Program Documentation Guidelines](https://www.bgsu.edu/arts-and-sciences/computer-science/cs-documentation/c-plus-plus-program-documentation-guidelines.html)  
[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Comments)  
[C++ Coding Practices, Style, Standards and document generation](http://www.yolinux.com/TUTORIALS/LinuxTutorialC++CodingStyle.html)  
[LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html)  
[Programming in C++, Rules and Recommendations](http://www.doc.ic.ac.uk/lab/cplus/c%2b%2b.rules/)  
[C++ Coding Standard](http://www.possibility.com/Cpp/CppCodingStandard.html)  
[C++ Programming Style Guidelines](http://geosoft.no/development/cppstyle.html)  
