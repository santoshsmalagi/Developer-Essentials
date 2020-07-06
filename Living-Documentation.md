### Documentation is All About Knowledge

Software development is all about knowledge and decision-making based on that knowledge, which in turn creates additional knowledge. Software design can last a long time. It can last long enough to forget about previous decisions made, as well as their contexts. It can last long enough for people to leave, taking with them their knowledge, and for new people to join, lacking knowledge. Documentation is about knowledge transfer, the process of transferring valuable knowledge to other people now and also to people in the future. The greater the ambition, the more documentation becomes necessary to enable a cumulative process of knowledge management that scales beyond what fits in our heads. When our brains and memories are not enough, we need assistance from technologies such as writing, printing, and software to help remember and organize larger sets of knowledge.  

For a new programmer to come to possess an existing theory of a program it is insufficient that he or she has the opportunity to become familiar with the program text and other documentation. We’ll never completely solve that knowledge transfer problem, but we can accept it as a fact and learn to live with it. The theory as a mental model in programmers’ heads can never be fully shared with those who weren’t part of the thought process that led to building it. The conclusion seems inescapable: At least in certain kinds of large programs, the continued adaption, modification, and correction of errors is dependent on a certain kind of knowledge possessed by a group of programmers who are closely and continuously connected to each other. So effectie documentation is about transferring knowledge in space between people and also about transferring it over time.

#### What Knowledge Must be Documented?
* Knowledge that is of interest for a long period of time deserves to be documented.
* Knowledge that is of interest to a large number of people deserves to be documented.
* Knowledge that is valuable or critical may also need to be documented.

> ***What is documentation?***
  The process of transferring valuable knowledge to other people now and also to people in the future.

### 1. Why knowledge is necessary?

Knowledge is necessary because:

* **It represents the mapping between code and the real world:**
     * The programmer who has the theory of the program can explain how the solution relates to the affairs of the world that it helps to handle.
* **The rationale of the program:**
     * The programmer who has the theory of the program can explain why each part of the program is what it is; in other words, the programmer is able to support the actual program
       text with a justification of some sort.
* **The potential of extension or evolution of the program:**
     * The programmer who has the theory of the program is able to respond constructively to any demand for modification of the program in order to support the affairs of the world
       in a new manner.
* **Without knowledge the following problems cannot be solved:**
     * Where can I fix that issue safely?
     * Where should I add this enhancement?
     * Where would the original authors add this enhancement?
     * Is it safe to delete this line of code that looks useless?
     * I’d like to change a method signature, but what impacts will result if I do?
     * Do I really have to reverse engineer the code just to understand how it works?
     * Do I really have to spend time reading the source code each time the business analysts need to know about the current business rules?
     * When a customer asks for a feature, how do we know if it’s already supported if it needs to be developed?
     * We have the feeling that the way we evolve the code is the best possible, but what if we lack a complete understanding of how it works?
     * How do we easily find the part of the code that deals with a particular feature?
* **Lack of knowledge manifests as two costs:**
     * **Wasted time:** That time could have been better invested in improving something else.
     * **Suboptimal decisions:** Other decisions could have been more relevant, or cheaper in the long term.

With existing software, when we miss the knowledge developed before, we end up redoing what’s already done because we don’t know it’s there already. We also end up putting a feature in an unrelated component because we don’t know where it should be, and this makes the software bloated. Or the code about the feature becomes fragmented across various components.

### 2. Specific vs Generic Knowledge

There is knowledge that is specific to your company, your particular system, or your business domain, and there is knowledge that is generic and shared with many other people in many other companies in the industry.

* **Generic Knowledge**
     * Knowledge about programming languages, developer tools, software patterns, and practices belongs to the generic knowledge category. Examples include DDD, patterns, continuous 
       integration and Git tutorials. 
     * Knowledge about mature business industry sectors is also generic knowledge. Even in very competitive areas like EDA or VLSI design, most of the knowledge is public and 
       available in industry-standard books, and only a small part of the business knowledge is specific and confidential.
     * The good news is that generic knowledge is already documented in the industry literature. There are books, blog posts, and conference talks that describe it quite well. There 
       are standard vocabularies to talk about it. There are trainings available to learn it faster from knowledgeable people.
       
* **Specific Knowledge**
     * Specific knowledge is the knowledge your company or team has that is not (yet) shared with other peers in the same industry. This knowledge is more expensive to learn than 
       generic knowledge; it takes time practicing and learning from mistakes. This is the kind of knowledge that deserves most attention.
     * Specific knowledge is valuable and cannot be found ready-made, so it’s the kind of knowledge you have to put maximal efforts to learn.
     
> ***As a professional, you should know enough of the generic, industry standard knowledge to be able to focus on growing the knowledge that’s specific to your particular ambitions.***

### 2.1 Learning Generic Knowledge
You learn generic knowledge by doing your job, as well as by reading books and attending trainings and conferences. This only takes a few hours, and you know beforehand what you’re going to learn, how long it will take, and how much it will cost. It’s as easy to learn generic knowledge as it is to go to the store to buy food. Generic knowledge is a solved problem. This knowledge is ready-made, ready to be reused by everyone. When you use it, you just have to link to an authoritative source.

### 2.2 Learning Specific Knowledge
Knowledge specific to your software or business product, is gained by learning from tool documentation, interacting with experienced collegues, internal training material, and finally using or experimenting with the software product or tool.

### 3. Characteristics of Effective Documentation

* **Reliable: Living documentation is accurate and in sync with the software being delivered, at any point in time.**
     * To achieve reliable documentation, we rely on the following ideas:
          * Exploiting available knowledge: Most of the knowledge is already present in the artifacts of the project, it just needs to be exploited, augmented, and curated for
              documentation purposes.
          * Accuracy mechanism: An accuracy mechanism is needed to ensure that the knowledge is always kept in sync.

* **Low effort:**
    * Living documentation minimizes the amount of work to be done on documentation**
    * Even in case of changes, deletions, or additions
    * It requires only minimal additional effort—and only once

* **Collaborative:**
    * Living documentation promotes conversations and knowledge sharing between everyone involved

* **Insightful:**
    * By drawing attention to each aspect of the work, living documentation offers opportunities for feedback and encourages deeper thinking
    * It helps reflect on the ongoing work and helps in making better decisions

### 4. Most Knowledge Is Already There

Acknowledge that most of the knowledge is already in the system itself. When needed, identify where it is located and exploit it from there. Even if the knowledge is there somewhere, this does not mean that there is nothing to do about it. There are a number of problems with the knowledge that’s already there:

* **Inaccessible:** 
     * The knowledge stored in the source code and other artifacts is not accessible to nontechnical people. For example, source code is not readable by nondevelopers.
* **Too abundant:** 
     * Huge amounts of knowledge are stored in the project artifacts, which makes it impossible to use the knowledge efficiently. 
     * For example, each logical line of code encodes knowledge, but for a given question, only one or two lines may be relevant to the answer.
* **Fragmented:** 
     * There is knowledge that we think of as one single piece but that is in fact spread over multiple places in the project’s artifacts. 
     * For example, a class hierarchy is usually spread over multiple files, one for each subclass, even though we tend to think about the class hierarchy as a whole.
* **Implicit:** 
     * A lot of knowledge is present implicitly in the existing artifacts. It may be, for example, 99% there but missing the 1% to make it explicit. 
     * For example, when you use a design pattern like a composite, the pattern is visible in the code only if you’re familiar with the pattern.
* **Unrecoverable:** 
     * It may be that the knowledge is there but that there is no way to recover it because it’s excessively obfuscated. 
     * For example, business logic is expressed in code, but the code is so bad that nobody can understand it.
* **Unwritten:** 
     * In the worst case, the knowledge is only in people’s brains, and only its consequences are there in the system. 
     * For example, there may a general business rule, but it may have been programmed as a series of special cases, so the general rule is not expressed anywhere.

### 5. Types of Documentation 

* **Internal Documentation**
     * *The best place to store documentation is on the documented thing itself.* 
     * The advantage of internal documentation is that it’s always up-to-date with any version of the product, as it’s part of its source code. 
     * Internal documentation cannot be lost because it’s embedded within the source code itself. 
     * It’s also readily available and comes to the attention of any developers working on the code just because it’s under their eyes. 
     * Internal documentation also enables you to benefit from all the tools and all the goodness of your fantastic IDE, such as autocomplete, instant search, and seamless navigation 
       within and between elements. 
     * Because internal documentation is expressed using implementation technologies, it can usually be parsed by tools. This opens new opportunities for tools to assist developers 
       in their daily tasks. In particular, it enables automated processing of knowledge for curation, consolidation, format conversion, automated publishing, or reconciliation

* **External Documentation**
     * An advantage of external documentation is that it can take whatever format and tool is most convenient for the audience and for the writers. 
   
 ### 6. Creating Documentation

The best place to put documentation about a thing is on the thing itself. Choosing internal documentation by default, at least for all knowledge that’s at risk of changing regularly.
Choose to do external documentation only when there is clear value add. The point of using external documentation would be to be able to add a human feel to the final document, so I’d use Apple Keynote or Microsoft PowerPoint, select or create beautiful quality pictures, and beta test the effectiveness of the documentation on a panel of colleagues to make sure it’s well received.

Examples of internal documentation:
* self-documenting code and use of clean code practices
* class and method naming
* using composed methods and types
* annotations that add knowledge to elements of the programming language
* comments on public interfaces, classes, and main methods
* folder organization and decomposition and naming of modules and submodules

Examples of effective external documentation
* hand-crafted slides
* diagrams with careful manual layout
* appealing illustrations
* tool user manuals, flow documentation
* example scripts etc.

### Useful Links
[Documenting C++ Code](https://developer.lsst.io/cpp/api-docs.html)  
[Where do I start with C++ documentation?](https://writing.stackexchange.com/questions/36232/where-do-i-start-with-c-documentation)  
[C++ Program Documentation Guidelines](https://www.bgsu.edu/arts-and-sciences/computer-science/cs-documentation/c-plus-plus-program-documentation-guidelines.html)  
[Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html#Comments)  
[C++ Coding Practices, Style, Standards and document generation](http://www.yolinux.com/TUTORIALS/LinuxTutorialC++CodingStyle.html)  
[LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html)
