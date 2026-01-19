# Object-Oriented Analysis and Design (Booch, 1994\)

## \*\*\*SECTION 1: CONCEPTS\*\*\*

## Complexity

* We are not interested in widgets here but the large, industrial-scale software that is inherently complex.  
* Complexity arises in four ways:  
  * Complexity of the problem domain: Users find it hard to express their requirements in a form that developers understand, and the demands of the problem domain can evolve over time.  
  * Difficulty in managing the development process: Modern software consists of millions of lines of code across thousands of modules, and requires a team of developers to write and maintain it. Managing such a team while maintaining focus is hard.  
  * Flexibility possible through software: It is totally feasible to create every building block from scratch rather than reuse others’ code, and some developers try to do so in their quest for abstraction.  
  * Behaviour of discrete systems: Because we are dealing with discrete systems (represented in binary), small changes can have large effects and the behaviour can become chaotic. It is impossible to anticipate every possible state and outcome, but we must do our best.  
* Changing requirements of a complex system (a common request from users) invites disaster. We should therefore work to understand and reduce the complexity of systems to minimize the risks involved in future changes.  
* Complex systems are often hierarchical \- split into layers of abstraction \- and involve lots of reuse.   
  * A desktop computer consists of a monitor, keyboard, motherboard, hard drive and so on. The motherboard consists of an ALU, primary memory, etc. Both the ALU and primary memory (and the hard drive and others) reuse integrated circuits that, in turn, reuse logic gates and transistors.  
  * Plants consist of roots, stems and leaves which all, ultimately, are made of cells. Roots and leaves don’t interact directly because they have an ‘abstraction layer’ \- stems \- in between that each interact with independently.  
  * Cells are also the building blocks for animals \- good reuse and abstraction.  
  * In cosmology: galaxies \> solar systems \> planets \> objects \> elements \> molecules \> atoms \> fundamental particles  
  * In business: economies \> industries \> companies \> departments \> teams \> people  
* Good implementations of complex systems have five attributes:  
  * They are hierarchical, and the value comes from the interactions between parts rather than the parts themselves  
  * Whether a part is ‘primitive’ is in the eye of the beholder; cosmologists and particle physicists look at the world in very different ways  
  * Links within components are stronger than links between components, separating the high-frequency dynamics (within) from the low-frequency dynamics (between)  
  * Hierarchical systems reuse only a few kinds of subsystems but in various combinations and arrangements  
  * A working complex system almost certainly evolved from a working simple system; you cannot build a complex system without a working simple system first.  
* Good software encapsulates these five attributes; software that doesn’t is often not good.  
* Software has two distinct hierarchies, collectively known as its **architecture**:  
  * The **class structure**, which says what each level of the hierarchy *has* (e.g. an aeroplane *has* engines, wings, a rudder, a fuselage, etc.)    
  * The **object structure**, which says what each component is within the component’s hierarchy (e.g. a turbofan engine *is* a type of jet engine, as *is* a ramjet engine; a jet engine *is* a type of engine, as *is* a propellor engine).  
* A limiting factor in our handling of complexity is the limit of the human brain: we can only mentally deal with so much complexity.  
* Because of this limitation, it is important that we consider only one layer of abstraction at a time \- divide and conquer, so to speak.  
* Algorithmic decompositions are the traditional top-down decompositions that turn one large operation into smaller operations which, in turn, are decomposed into smaller operations until you reach single commands at the leaves of the tree.  
* Object-oriented decompositions instead consider the actors or agents involved in the process, and the messages they pass to each other to effect the lower level operations.  
* Booch advocates the OO approach because it often leads to a greater separation of concerns and better software with more scope for reuse and evolution.  
* Creating a piece of software requires both design (an art), and analysis and implementation (a science)  
* Good design leads to useful models that enable us to predict outputs from given inputs, and therefore check whether the system meets the requirements (constraints, performance, form, schedule, cost, etc.)  
* Models often reuse well-tested concepts (e.g. Design Patterns) to reduce the scope for error  
* Software design methods bring discipline to the process, and come in three flavours:  
  * Notation: the language we use to express the model  
  * Process: activities leading to the orderly construction of the system’s models  
  * Tools: artifacts that eliminate the tedium of model building, and that enforce rules about the models to catch errors early

## The Object Model

* Object-oriented languages are a relatively late step in the evolution of software platforms.  
  * Early platforms employed subroutines that manipulated global data  
  * Later subroutines \- now recognized as a level of abstraction \- were grouped into modules, first for convenience before it was recognized that the module was a higher level of abstraction  
  * Object-oriented platforms \- those that focus on ‘nouns’ (objects) rather than ‘verbs’ (procedures) \- later formalised the OO design methods by encapsulating an object’s data within the object, making it inaccessible to outside objects as it had previously been  
  * Every new generation of software introduced a new level of abstraction and, though many platforms did not survive the concepts they introduced live on as features in later releases of more established platforms.  
* **Object-oriented programming** (OOP) languages must implement programs via a) *composition* (the ‘has-a’ hierarchy), b) *inheritance* (the ‘is-a’ hierarchy), and c) *instances* of *classes*. Anything else is not considered object-oriented (though it could be *object-based*).  
* **Object-oriented design** (OOD) leads to an OO decomposition of the problem domain rather than an algorithmic decomposition, using various notations to express the logical and physical models of the solution  
* **Object-oriented analysis** (OOA) is a depiction of the system *requirements* in terms of object models  
* OOA leads to models for OOD which provides blueprints for OOP.

### Elements of the Object Model

* The Object Model has seven characteristics:  
  * Four major (essential): abstraction, encapsulation, modularity, hierarchy  
  * Three minor (useful): typing, concurrency, persistence

##### Abstraction

* This is the means by which we describe an object in terms of its essential behaviour and nothing more, focusing on the attributes that are important to the viewer while hiding irrelevant details, and offering no surprises or side effects.  
* There are four levels of abstraction:  
  * Entity (best): An object that is a useful model of an entity in the problem or solution domain  
  * Action: An object that provides a set of operations that all fulfill the same kind of function  
  * Virtual machine: An object that groups operations used by some higher level of control, or that all depend on some lower level set of operations  
  * Coincidental (worst): An object that packages operations that are unrelated  
* Entities should reflect the vocabulary of the problem domain.  
* Under the *contract model*, the outside view of the object defines a contract upon which other objects can depend and that encompasses the responsibilities of an object.  
* The legal ordering of the permitted operations is called a *protocol*.  
* An invariant specifies something that is always True or False, and whose value must be checked. *Preconditions* check the validity of the *inputs*; *postconditions* check the validity of the *outputs*. \[I think this is the purpose of *assert* statements in Scott Meyers’s books.\]  
* \[There follows an example set up of an automated gardener\]  
  * The first class created is a temperature sensor that can be calibrated (a *modifier* \- changes something internally) and probed (a *selector* \- returns something)  
  * This is extended by adding the option of a *callback* function \- a function provided to the server (the sensor) by a client (the thing using the sensor) that is to be called at a time of the server’s choosing. In this case, it is designed to execute the callback when the temperature crosses some threshold.  
  * Finally, a GrowingPlan object is defined with a virtual modifier and a nonvirtual selector that returns the desired conditions (a structure \- not a class) at a given time.

##### Encapsulation

* While Abstraction defines the outwardly observable behaviour and therefore “helps people to think about what they are doing”, *Encapsulation* defines how that behaviour is achieved and “allows program changes to be reliably made with limited effort.”  
* **“\[the\] ability to change the representation \[implementation\] of an abstraction without disturbing any of its clients is the essential benefit of encapsulation.”**  
* \[This is part of fleshing out the example started earlier\]  
  * Adds a Heater class abstraction, with an implementation that uses a SerialPort class (important for the representation but not the abstraction)  
  * Suggests using an open hash table with interpolation (encapsulated implementation details) for the GrowingPlan (the abstraction).

##### Modularity

* Objects (abstractions) reflect the *logical* structure of the problem domain, whereas modules should reflect the *physical* structure.  
* Modules should expose only those parts that are absolutely necessary for their clients  
* It is possible to over-modularize, making maintenance and documentation harder.  
* “System details that are likely to change independently should be secrets of separate modules”  
* A server module’s data should not be accessible to client modules, except through the methods of the server module itself.  
* Cohesion is the property of grouping logically related objects  
* Coupling is dependency between modules.  
* **“Modularity is the property of a system that has been decomposed into a set of cohesive and loosely coupled modules.”**  
* Logical design and physical design proceed together, often iteratively  
* Large modules with lots of interfaces are likely to change more often which, in C++ at least, will involve lots of recompilation.

##### Hierarchy

* Juggling different abstractions can be mentally challenging, and aided by the use of hierarchies that partition the many abstractions into separate layers, each of which can be considered largely independently of the other layers.  
* The first hierarchy of note is the “is-a” hierarchy defining the *object structure* via *inheritance*. \[Note that Booch here flips the definitions that he used before.\]  
  * “Is-a” hierarchies define effectively a Venn Diagram \- circles within circles (single inheritance) with the odd overlapping circles (multiple inheritance)  
  * \[Note: in Booch’s terms, derived classes can add new public methods which do not exist in the parent class.\]  
  * “Is-a” hierarchies indicate specialization: lower levels of abstraction are specialized versions of the objects a higher levels of abstraction.  
  * Multiple inheritance is often implemented using MixIn classes (that simply add functionality to an interface) and is often overused.  
* The second hierarchy is the “has-a” hierarchy defining the *class structure* via *composition*.  
  * The higher levels of abstraction contain (are composed of) objects at a lower level of abstraction, but one is not a specialization of the other. (E.g. a garden contains flowers, but flowers are not a specific type of garden.)  
  * It is important to consider ownership and the lifespan of the component objects.   
    * In some cases, the child objects are intrinsically tied to the parent and should be destroyed along with it. This is implemented with value instances.   
    * In other cases, child objects can be shared between parent objects or transplanted from one parent to another. This is usually implemented with references or pointers.

##### Typing

* **“Typing is the enforcement of the class of an object, such that objects of different types may not be interchanged or, at the most, they may be interchanged only in very restricted ways.”**  
* Typing effectively limits operations to specific class hierarchies  
* Run-time identification is when you can check the type at run-time before attempting an operation, but this goes against encapsulation and should be avoided  
* Run-time coercion involves casting an object to a specific class in order to perform some operation (e.g. call a method that is only defined in the derived class) but this goes against abstraction and should also be avoided  
* We emphasize the difference between *typing* (enforcing restrictions on operations between certain objects) and *binding* (deciding which member function to call in an inheritance hierarchy).  
  * *Static binding* means that we know from the outset which lines of code will be executed for a given function call (e.g. a method defined only in the base class)  
  * *Dynamic binding* means we won’t know until run-time which lines of code will be called (e.g. a method defined in the base class and also overridden in the derived class).

##### Concurrency

* Where multiple processes need to occur simultaneously, we call this concurrency.  
* Each process is considered to be *active* or *inactive*, and :  
  * **“Concurrency is the property that distinguishes an active object from one that is not active”**  
* Many languages support concurrency in one way or another  
* Concurrent programs are typically more difficult to implement because we need to ensure that objects acting at the same time do not “tread on each others’ toes.”

##### Persistence

* **“Persistence is the property of an object through which its existence transcends time (i.e., the object continues to exist after its creator ceases to exist) and/or space (i.e., the object’s location moves from the address space in which it was created).”**  
* Usually, persistence is concerned with the former (time). Objects can be transient (e.g. loop variables) right through to outliving all processes that might use them (e.g. entries in a database file).  
* Incorrect consideration of persistence can lead to unhelpful implementations, e.g. wacky ways to implement persistence in memory when entry into a database would be more appropriate, or entering transient objects into a database. \[These are usually examples of the adage “when the only tool you have is a hammer, everything can start to look like a nail.”\]

### Applying the Object Model

* Using the Object Model over other forms (e.g. procedural models) has a number of benefits including ease of design, reuse of code, ease of understanding, and others.

## Classes And Objects

### The Nature of an Object

* Objects have specific behaviours and crisp boundaries that separate them from other objects and nonobjects (e.g. love, time, anger).  
* “An object has state, behaviour and identity; the structure and behaviour of similar objects are defined in their common class; the terms instance and object are interchangeable.”  
  * *State* is the set of (usually dynamic) values of the object’s (usually static) properties.  
  * *Behaviour* is how an object reacts to messages from other objects (or from the main program). It is typically encoded via operations (such as modifiers, selectors and iterators), can change the state (via modifiers) and often depends on the current state. The behaviour will also depend, to a degree, on the object’s role and responsibility at any given time (both of which can change over time). Some objects behave passively (waiting for something to send them a message) whereas others can be more active (doing lots of the message sending, as in an event loop).  
  * *Identity* distinguishes one object from all others (including those of the same class). How we refer to an object is intimately related to its identity, as is how we allocate memory to store it. This has implications for construction, assignment, equality, argument passing and destruction if we are to avoid memory leaks or other undesirable behaviour.

### Relationships Among Objects

* Relationships between objects come in two main flavours: Links and Aggregation  
  * *Links* are connections between objects, indicating that one object can send a message to another. Objects can have one of three roles in this context: an Actor operates on another object; a Server is operated upon by another object; and an Agent works as an intermediary, operating upon a Server in order to carry out the instructions of an Actor. The visibility of one object to another is of concern (a “tactical design issue”). The synchronisation of operations is also a concern when using concurrent programming or threads.  
  * *Aggregation* is where one object (the attribute) is a part of \- or belongs to or is contained by \- another (the aggregate). This may be a physical containment (wings, wheels and engines are all part of an aeroplane) or a conceptual one (shares belong to a shareholder without being a part of the shareholder). There is, by definition, a *Link* between an aggregate and its attributes.  
* There is often a choice to make between Links and Aggregation: aggregation (make an object an attribute of the aggregate so that the aggregate can access it) is a form of encapsulation and structure, whereas Links (passing the attribute object as an argument to the aggregate’s method) reduce coupling.

### The Nature of a Class

* “*A class is a set of objects that share a common structure and a common behaviour*”  
  * Corollary: “*A single object is simply an instance of a class*”  
* Some classes are large and complex, and only describable in terms of other smaller component classes; Booch calls such a class a *class category*.  
* Classes come with an interface and an implementation  
  * The interface describes the behaviour of the class from the outside looking in \- the client’s view  
  * The implementation describes the method by which the behaviour is implemented from the inside.  
* Methods can be private (available only to the instance itself), protected (available to the class, its subclasses and identified “friend” classes) or public (available to any other class).

### Relationships Among Classes

* Classes are related by one of several relationships, of which three are well known (from strong to weak):  
  * Generalization/Specialization (“is-a”; inheritance)  
  * Whole/Part (“has-a”; aggregation or composition)  
  * Client/Server (“uses”; association)

##### Association

* Some classes are associated in some weak way, for example a Product that is sold and the Sale that includes the product and perhaps other products.  
* The direction of the relationship is not always specified but can often be inferred by the names we give to the classes, indicating their role.  
* Cardinality indicates the relationship between numbers of objects in the relationship: one-to-one; one-to-many; or many-to-one.

##### Inheritance

* In a class structure, properties of a superclass are inherited by its subclasses, and the topmost superclass in the hierarchy tree is the base class.  
* Classes in an inheritance tree have two clients: instances and subclasses.  
  * Instances are from outside and can access only public members  
  * Subclasses are more within and can access both public and protected members  
* Inheritance of protected members breaks encapsulation to a degree  
* Subclasses inherit both the structure of their superclass and, in some cases, the behaviour too.  
* Member functions that can be overridden are declared virtual; those that must be overridden are pure virtual; everything else is bound to the class in which the function is declared.  
* This paves the way for polymorphism whereby a method can have different implementations \- one for every class in the hierarchy \- and the specific implementation used is chosen according to the class of the instance in question at run-time (using dynamic, or late, binding). The alternative is to use switch/case statements to select the specific implementation manually, which is necessary when everything uses static, or early, binding.  
* In practice, the methods are called via a reference to the base class interface, since all subclasses are required to conform to (or implement) this interface.  
* It is not unusual for a subclass to extend a superclass method by first calling the superclass implementation, then adding more commands after it.  
* It is also not unusual for a superclass to specialize its own methods by calling subclass member functions within a general algorithm.  
* Notes on assignment:  
  * Subclass instances can be assigned to superclass variables, though data may be lost  
  * Superclass instances cannot be assigned to subclass variables because missing data cannot be inferred  
  * Functions to cast between types can be written explicitly for this purpose  
* In most cases, a subclass exposes the same interface as its superclass and is therefore also a subtype.  
  * In cases where a subclass inherits privately from a superclass, what was public in the superclass becomes private in the subclass, thus changing the exposed interface such that the subclass is no longer a subtype.  
  * (I believe the subtype status of an object affects what it can be assigned to or casted as.)  
* Inheritance is used both as a means to reuse code (e.g. methods implemented in a superclass) and to specialize functions (e.g. superclass methods that call subclass methods polymorphically). There is a tug-of-war between these two ideas, which Booch comes back to later in the book.  
* There are times when single inheritance is insufficient to capture the heritage of an instance and the designer would be forced to choose between two or more equally attractive superclasses and then reimplement the bits they would have liked from the superclasses they didn’t choose. At these times, multiple inheritance is called for.  
* Multiple inheritance usually gives people the willies for a number of reasons:  
  * What if both superclasses have a variable or method of the same name?  
  * Some languages consider this a violation and won’t compile.  
  * Some require you to be specific in which of the superclass members you’re referring to (e.g. through “superclass::member” notation)  
  * Others take the order in which you specify the superclasses as an order of preference, effectively merging all of the superclasses into one and treating it as single inheritance.  
  * Virtual base classes are required (if supported) when the diamond formation occurs: D derives from B and C, both of which derive from A (such that D derives from A twice).  
* Superclasses that exist only to extend the functionality of an existing class in specific ways are called mix-in classes, and typically cannot be instantiated by themselves (they need a fellow class to extend).  
* “A class that is constructed primarily by inheriting from mixins and does not add its own structure or behaviour is called an *aggregate class*.”  
* Where a polymorphic function takes a reference to a base class and calls its methods (which involves a second level of polymorphism) this is known as *double dispatch*.

##### Aggregation

* Aggregation typically refers to a has-a or uses-a relationship rather than is-a (i.e. inheritance).  
* Physical containment implies that the container owns an instance of the containee such that they have the same lifespan and do not exist independently of each other \[e.g. a person “has a” skeleton\].  
* In containment by reference, the container owns a pointer (or reference) to an instance of the container such that the two can exist independently (and the container can be contained by more than one container). Responsibility for memory allocation is delegated to a single object.  
* Unless one class genuinely “is a” superclass of another, you should prefer aggregation over inheritance \[i.e. don’t use inheritance for the purpose of code reuse\].  
* A client that uses a “supplier” instance to do work needn’t contain the supplier. It is only strictly a “using a” relationship if the supplier is part of the client’s *interface*.  
* Parameterized classes (using templates in C++, for example) allow you to instantiate the same class for any number of object types (e.g. a *Queue* container of *Item* containees, where different concrete classes are instantiated for whatever *Item*s the user chooses). This is preferable to inheritance in Booch’s view.  
* A class whose instances are, themselves, classes is called a *metaclass*. C++ doesn’t support them as such, though it has features that are metaclass-like. \[I didn’t really get the point of metaclasses, tbh.\]

### The Interplay of Classes and Objects

* A software designer will, in the early stages, break down the problem into its *key abstractions* (classes and objects) and propose a solution in terms of *mechanisms* (processes). This is primarily an outside-in view of the scene.  
* Later on, they will be more concerned with the physical implementation of the key abstractions and mechanisms; this is the inside-out view.  
* More often, these two processes iterate.

##### Abstraction Quality

* Coupling denotes the degree to which modules that could and should be separate are intertwined, making it difficult to change one without taking the other apart. (Analogy: a speaker cabinet with the power supply fixed inside.) It is generally bad, though inheritance (which is a good feature) increases coupling. \[i.e the strength of Connections between modules/classes.\]  
* Cohesion denotes the degree that elements of the same module contribute to the same specific behaviour and nothing else. Coincidental cohesion is achieved by lumping completely different elements into the same module (and is therefore bad); functional cohesion occurs when all elements work toward the same unique purpose. \[i.e. the strength of connections within modules/classes.\]  
* An abstraction is sufficient if it provides the minimum standard of use to the client; it i complete if it provides everything the client might need; and it is primitive if it has no methods that are implemented solely in terms of other public methods (i.e. that the user couldn’t implement themselves via the client interface). All of these are desirable (though there is obviously a trade-off).  
* Functional semantics refers to which functions are packaged together and which are kept apart. Free subprograms (e.g. functions outside the class body, and therefore that are not inherited) can be useful for keeping classes primitive.  
* Time and space semantics refer to when messages are passed from sender to receiver, and how much space those messages (and the associated functions) take.  
* This is especially salient where concurrency may be used:  
  * Timeout: the sender sends and waits for the receiver to receive and act, aborting the process after a set period of time, t.  
  * Synchronous: as Timeout with t=infinity  
  * Balking: as Timeout with t=0  
  * Asynchronous: sender sends whether receiver is ready or not, and then continues with its business.  
* Collaboration is linked to coupling in that objects can only send messages to objects of other classes that they can see; keep the visibility of classes to the minimum required.  
* Making decisions on representation and packaging can be hard; they often involve a trade-off; and they require knowledge of what the client will expect or assume, and what might change in the future.

## Classification

* Classification is hard, and often in the eye of the beholder (or, at least, very application dependent).  
* Classification comes in three flavours (at the time of writing):  
  * **Classical classification** looks for orthogonal (noninteracting) properties that an object either has or hasn’t, and groups objects according to the binary code that results. But it has problems in that, for example, not all birds can fly and not all chairs have legs. Nor need the classification be useful in any given application: traffic light systems do not care for the colour of the cars they direct.  
  * When concrete properties aren’t readily available **conceptual clustering** may provide some guidance, grouping objects more probabilistically on the basis of fuzzy characteristics (e.g. “love song-ness”).  
  * If all else fails, **prototype theory** groups object according to their similarity to some prototype(s), asserting that everything that is similar to a chair should be classified as a chair.  
* **Analysis** seeks to discover the classes and objects that make up the vocabulary of the problem domain, and comes in different flavours:  
  * **Classical approaches** identify the nouns, verbs, roles, etc. with which the problem domain can be described.  
  * **Behaviour analysis** connects objects with similar behaviours or similar responsibilities and that are often thought of as members of the same class.  
  * **Domain analysis** requires developers to pick the brains of domain experts (usually end users) to identify the key components of the problem domain.  
  * **Use-case analysis** sits developers and end-users down together to sketch out typical (and corner) scenarios of the system in use, identifying actors and responsibilities. It can take place very early on (as early as requirements analysis).  
  * **“CRC” Cards** are 5x7 inch cards that have \- in pencil \- the name of the **C**lass at the top, its **R**esponsibilities on the left, and its **C**ollaborators on the right. They were originally devised as a teaching tool but eventually found their way into real-world applications because they’re incredibly useful. They are created by a group, working together to identify the structure of the problem and solution.  
  * **Informal English Description** requires you to express clearly the problem in prose and then underline the nouns and verbs. The hard part (making it unsuitable for many applications) is the expressing clearly the problem bit \- that isn’t trivial.  
  * **Structured Analysis** is a precursor to Structured Design rather than Object Oriented Design \[and is therefore shunned somewhat by Booch here, so I’ll not say any more about it \- his explanation is confusing to anyone not versed in SA/SD\].  
* **Design** seeks to invent the abstractions and mechanisms that provide the behaviour that the model requires  
  * **Key abstractions** are those that form part of the vocabulary of the problem domain. Other abstractions are considerations outside of the domain.  
  * Refining the key abstractions is a process of evolution, and the most common operations are splitting a class into two or more components or merging a number of initially disparate classes into a class hierarchy with a common superclass.  
  * Naming conventions are important if the code is to be as easy to read by a human as it is by a computer (*which is crucial*).  
  * **Mechanisms** are the means by which objects interact with each other \- how they communicate and collaborate. They range from primitive *idioms* (quirks that are usually specific to a language) to complex *frameworks* (a complete suite of models and classes specific to a problem that you customise to solve your particular instance of the problem).  
  * The *Model-View-Controller* paradigm is an example of a mechanism.

## \*\*\*SECTION 2: THE METHOD\*\*\*

## The Notation

* *“A diagram captures a statement of a system’s behaviour (for analysis) or the vision and details of an architecture (for design).”*  
* Many views are necessary for a complete understanding of a system, just as many camera views are used in sports broadcasts and movies to give a clearer picture of what is going on.  
* A diagram provides one view of a given model, and there will be other possible diagrams that provide other possible views.  
* The Logical Model describes the key abstractions in terms of their class and object structure (and behaviour, roles and responsibilities) while the Physical Model considers how they will be implemented in a specific piece of hardware and software (their relationships, collaborations, creation/destruction and allocation to processes).  
* A diagram gives a *static* view of the model whereas we often require a *dynamic* image of the flow or order of events.  
* Tools amplify what is already there: good designers will come up with greater designs; bad designers will come up with awful ones.

### Class Diagrams

##### Classes

* A Class is represented as a dotted blob with a name, attributes and operations.\#  
* A relationship with another class is indicated by a solid line.  
  * An arrowhead points from a subclass (derived) to a superclass (base)  
  * A container has a filled circle at their end, and the number of containers/containees is stated at the relevant end of the line. (Think about creating k multiples of the object at each end.)  
  * A client has an open circle at their end, with the supplier at the other.  
* Other properties of the class can be added with a letter in a rectangle, such as “A” to indicate an abstract class.

##### Class Categories

* Class categories are clusters of related classes that serve a given function. In Python, a *package* is a class category.  
* Some of the classes in a category (package) are intended to be used (imported) by classes in other categories and can be considered “public” (the default), whereas others are used only internally to provide the implementation and are therefore considered “private”.  
* Class categories are denoted by a solid rectangle with a name and perhaps a short list of the most important classes.  
* They are linked to other categories via “using” relationships.  
* Setting them out in a tree-like hierarchy indicates any hiding of categories from ones several levels up in the hierarchy.  
* Categories closest to the problem domain are usually found near the bottom, whereas those closest to the user domain are found nearer the top.

##### Advanced Concepts

* Parameterized classes (Templates in C++)  
* Metaclasses (the class of classes)  
* Class utilities are a collection of methods or functions that operate on a particular class. In implementation, these could be functions in a module or static methods in a class that has no state of its own and cannot be used to instantiate objects.  
* Nesting is where one class (or class category) is defined within the scope of another. Nesting classes tends to be tactical whereas nesting categories is more strategic/structural. (In Python, nesting packages is common; nesting classes is not.) (Booch recommends a depth of no more than two, though this may not apply in today’s world.)  
* Export control: we can define (and denote on the diagram) the visibility of attributes, inheritance and methods via marks either on the relationship line (at the superclass, client or container end) or before the name of an attribute. Zero lines means public; one line is protected; two lines private; and three lines indicate an implementation detail (inaccessible even to friends of the class).  
* Properties such as static methods, virtual inheritance and friendship are indicated with S, V or F in a triangle (as an Abstract class was denoted before).  
* Physical containment suggests that one class can access another from within its members, though this does not suggest that one physically contains another. If, however, one is to contain the other it may do so by value (in which case the contained is created and destroyed at the same time \- usually by \- the container) or by reference (e.g. a pointer to the contained is manipulated by the container). These latter relationships are denoted by a filled and open square at the containee’s end of the “has-a” relationship.  
* Roles and keys indicate the protocols used between a client and a supplier, for instance whether the client receives information from the supplier or whether the client can send information to the supplier as a “contributor.” They are written next to the line and closest to the class they describe. Keys are a unique identifier of an object and are written in \[brackets\].  
* Constraints within or between classes are denoted similarly to keys, only in {braces} rather than \[brackets\].  
* Attributed associations are properties of associations that are more complex than those already mentioned. (I get the feeling I’ll never come across this ever again.) Notes can be added using a graphic of a note for added information anywhere, really.

##### Specifications

* Class diagrams are a useful bird’s-eye view of the system, but there are parts that must necessarily be left out. These additional details are better suited to text rather than graphics and so are included as **specifications**.  
* Every specification includes a name and definition. This should be self-explanatory.  
* Classes also list the responsibilities, attributes, operations and constraints applicable. There may also be specifications if the class follows a state machine, for export control and cardinality (e.g. a Singleton has cardinality 0 or 1), parameters (in the case of a parameterised/templated class), persistence, concurrency, space complexity (how it is stored).  
* Operations (class member methods) also have their own specification that includes the return type and arguments as the minimum. They may also include qualifications (static, virtual, etc), export control (public, private, etc), protocol, preconditions, postconditions, semantics, exceptions, concurrency, space complexity and time complexity (e.g, “O(n)”).

### State Transition Diagrams

* State transition diagrams illustrate the dynamic behaviour of a class or of the system as a whole.  
* States are drawn as rounded rectangles with a name and events/actions, much as a class has a name and attributes (though an event triggers an action).  
* State transitions are drawn as directed arrows and they indicate a change in the state of the class.   
* An action takes zero time (e.g. turning on a heater) whereas an activity takes finite time (e.g. heating a room to a desired temperature).  
* Actions and activities can be represented in the program by symbolic names or operations or classes created specifically (like Exception in C++/Python).  
* A filled circle with an arrow points to the default state that is the state upon the first entry.  
* An end condition points with an arrow to a filled circle within an unfilled circle, though this is uncommon to show (most state machines don’t end, only destruct).  
* Entering or exiting a state may be the event that triggers an action.  
* Events that are conditional have their conditions added in \[brackets\]  
* Keywords such as “in” may indicate the current state, while “do” can indicate the action to take in response to an event.  
* Timers can trigger an event and transition, and are denoted by timeout(X, t) for condition X after time t.  
* If a state does nothing for a particular event, this should be documented explicitly: *“Silently ignoring events is usually an indication of an incomplete analysis of the problem.”*  
* States can be nested, such that an outer superstate contains inner substates with their own state transition diagram.  
* A filled circle within a superstate diagram show the transition upon first entry to the superstate.  
* An H in a circle denotes a “history” setting whereby upon exiting from a superstate, the substate of the inner system is returned to upon the next entry to the superstate.  
* When leaving a superstate, all substates within are set to the same state by default.  
* Specifications for states are usually unnecessary because everything has been specified already.

### Object Diagrams

* Object diagrams are similar to class diagrams, only objects (usually) have names and are instantiations of the classes. If only the class is specified, it is denoted “:\<classname\>” whereas a fully named object specifies the name and class as “\<objectname\>:\<classname\>”  
* Objects communicate via *links*: concrete instances of abstract class associations.  
* When object A calls method M of object B, A is the *client* and B is the *supplier*.  
* Messages are shown by an arrow alongside the association, directed from the client to the supplier, and labelled with the message (member function) name with arguments. Messages can be numbered to indicate an ordered sequence.

##### Advanced Concepts

* Links between objects can also be adorned with:  
  * Roles, keys and constraints  
  * Directions of data flow (e.g. returning a flag to indicate success)  
  * visibility of the supplier from the perspective of the client (supplier is Global, supplier is Local, supplier was passed as a Parameter to a client method, or the supplier is a member \- or Field \- of the client) in a square box  
  * Synchronization attributes (do they communicate synchronously or asynchronously)  
  * Time budgets (i.e. when there is a delay between one event and the next)

##### Specifications

* The only specification needed for an object diagram is whether the scope is globe, class category, class or operation.

### Interaction Diagrams

* \[I’ve seen these much more frequently than object diagrams \- presumably they’re more popular.\]  
* These contain the same information as object diagrams, only in a different form that is easier to scan over the sequential nature.  
* Objects are indicated by name as “columns” across the top with a vertical line running top to bottom beneath each object.  
* Messages are indicated by arrows running left to right (or vice versa) and ordered sequentially from top to bottom.

##### Advanced Concepts

* A “script” (i.e. a narrative) can be added to the left of the messages to give more depth to the description.  
* Focus of control is indicated with boxes along the vertical lines to show for how long an object is in control of the flow of information.

### Module Diagrams

* Module diagrams indicate dependencies between physical files in the software system.  
* There is usually a main program, along with one or more specifications (e.g., .h files) and bodies (e.g., .cpp files)  
* Dependencies are shown by directed arrows pointing from the includer to the included.

##### Essentials: Subsystems

* Subsystems are a collection of modules that are closely related and provide a high-level function. (In Python, they are packages.)  
* The subsystem will contain some entities that are public and others that are internal and for implementing the function of the subsystem only.

##### Advanced Concepts

* Some languages have more specific module types that might be indicated on the diagram.  
* Memory constraints may impose segmentation of the code which may also be indicated.  
* \[Neither of these are likely to be applicable today.\]

##### Specifications

* Again, written specifications add little to module diagrams \- the diagrams themselves are sufficient.

### Process Diagrams

* Process Diagrams indicate the associations between processors, devices and connections.  
* A *processor* is shown as a 3D cube with filled sides and is capable of executing a program  
* A *device* is a 3D cube with empty sides and is not capable of (or at least responsible for) executing code (e.g. a modem or terminal)  
* Connections can be unidirectional or bidirectional (usually the latter) and link devices to devices, processors to processors, or devices to processors.

##### Advanced Concepts

* Tailoring the diagram uses more specific icons to represent devices and processors of different kinds to better illustrate the system.  
* Nesting entities into subsystems is also helpful at the hardware level.  
* Scheduling of processes can take place by five main protocols:  
  * Preemptive gives greater priority to more important tasks, even when less important ones are already running  
  * Nonpreemptive allows one process to complete before scheduling the next, regardless of perceived importance.  
  * Cyclic passes control from process to process after a given frame (slice) of time.  
  * Executive has a single process that controls the schedule of all others  
  * Manual systems have the scheduling set by an outside user

##### Specifications

* Again, there is little need for additional specifications

### Applying the Notation

* These diagrams all support each other, and a full system specification will include one or more of each at several levels of the hierarchy.  
* Keeping track of them and ensuring consistency and completeness between them is a job for a software tool in itself.

##### Scaling Up and Scaling Down

* These diagrams can be used for all projects, big and small, and evolve as new design decisions are made.

## The Process

### First Principles

* Every successful software system has two properties  
  * A strong architectural vision  
  * The application of a well-managed, iterative and incremental development life cycle  
* The architecture…  
  * …is constructed in well-defined layers of abstraction, each tapping into the lower layers through a well-managed interface  
  * …show a clear separation of concerns between interface and implementation  
  * …is simple, and achieves common behaviour through common abstractions and common mechanisms.  
* The best development cycles are neither anarchic (no rules) nor draconian (too many rules) but iterative and incremental.  
* These cycles are the antithesis of the waterfall approach where everything flows in one direction only, and therefore require developers to have a broad set of skills so that they can think about everything from architecture to the real nitty-gritty.  
* A well-managed development cycle is essential to know how long things will take and what changes should be made with the resources at hand.  
* Development processes range in maturity from Initial (chaotic) through Repeatable (a documented steps forward), Defined (well-established with some automation) and Managed (quantitative measures provide a feedback loop) to Optimizing (well-tuned processes that consistently deliver good results).  
* The development process can be further split into an iterative Micro process and waterfall-like Macro processes.

### The Micro Development Process

* By and large, the Micro process covers the daily decisions and activities of the development team.  
* It runs in a cycle of four stages.

##### Identifying Classes and Objects

* The goal of this process is to refine the data dictionary \- a vocabulary of actors in the system along with the roles that each plays.  
* A sign that this process is working well is that the data dictionary becomes stable from one iteration of the Micro cycle to the next.

##### Identifying Semantics

* Next, identify the roles of the classes within the responsibilities they have.  
* Every responsibility should be expressible in a single sentence.  
* Think through various scenarios and assign responsibilities to abstractions, reusing existing responsibilities where possible (“storyboarding”).  
* Later, create a list of operations that satisfy the responsibility for every abstraction. Think about creation, destruction and copying soon after (don’t let every class do this in its own idiomatic way).  
* Leave thoughts of inheritance until as late as possible; “introducing inheritance prematurely often leads to loss of type integrity.”  
* When you do get to inheritance, put operations common to siblings in their parent superclass, and operations common to disjoint classes into a mixin class.  
* The output of this process is a set of clearly defined abstractions with roles and responsibilities, plus the operations that are needed to carry out those responsibilities. At this point it should be possible to write the interface/header for every class.

##### Identifying Relationships

* Here we identify the ways in which classes work with, and depend on, each other.  
* The output from this stage is typically (class, object, module) diagrams that show how classes interact.  
* Later on in the process, these will show how class categories cluster abstractions into layers and partitions.  
* This part of the process involves thoughts on inheritance (“is a”) and aggregation (“has a”) and how classes and modules would be organized to give a cohesive and loosely coupled system.  
* At this point, the header files should be largely complete, including definitions of inheritance and aggregation among classes.

##### Implementing classes and objects 

* This is where we put the meat on the bones, and identify new classes that will help us to implement the responsibilities of the classes we are currently implementing.  
* These new classes will be the subject of the next iteration of the Micro development process.  
* By the end of this stage, we should have pseudocode at least and executable code at the very end.  
* We are then ready to release the code into the wild where we can look for scenarios where it does not perform as well as expected and work out how to improve it.

### The Macro Development Process

* The Macro Development process is concerned primarily with risk, schedules and overall project management.  
* The five main stages (start to finish, because there is a definite finish) are conceptualization, analysis, design, evolution and maintenance.

##### Conceptualization

* At this stage, an idea pops into someone’s head and needs to be tested to see if it has legs.  
* Prototype software serves that purpose \- a proof of concept to test the water to see if any big assumptions turn out to be wrong.  
* Prototype software is intended to be archived rather than developed into a full system; it is better to start the actual system afresh.  
* When developing the prototype, it is best to remove all rules and barriers and leave the team (often the one person who had the idea) to get on with it. Otherwise, the team might leave and start up their own company with constraints of their choosing.  
* A business with too few ideas is moribund; one with too many is chaotic.  
* The prototype should come with a strict deadline to avoid “mission creep”.

##### Analysis

* In Analysis, we aim to create a model of the system’s behaviour. That is, what the system does under the most important conditions rather than how it does it.  
* The “most important” conditions are the ones occurring most often and the ones occurring less often but that have a dramatic effect on the system behaviours.  
* Analysis can also quantify the requirements of the system in terms of efficiency, security, reliability, portability and so on.  
* A risk assessment document produced here also spells out the main risks that might require expensive changes to architecture further down the line.  
* Domain analysis looks at the properties and characteristics of the domain in which the problem lies. Also look for other similar problems within the same domain in order to learn from those who came before you.  
* Scenario planning involves running through typical use cases in order to document any assumptions, constraints or performance issues.   
* Involve people with different roles in this process in order to get diversity of perspectives.  
* This process is complete when a complete and simple analysis document has been signed off by the analysis team (a minimum of domain expert, architect, analyst and end user).

##### Design

* Although the analysis will never be “finished” (in the sense that you can’t plan ahead for every scenario), starting the design before the analysis is “complete” (with all of the major scenarios considered) leads to trouble.  
* An “architectural release” is a “vertical slice through the entire architecture” and should be executable, though incomplete in its capture of the system’s semantics.  
* The Design stage also includes defining tactical policies to ensure consistency among different developers in the same system.  
* There are three activities here:  
  * Architectural planning: defining layers and partitions whereby higher layers build on lower ones, and classes in the same partition operate at the same level of abstraction as peers.  
  * Tactical design: nail down the many policies that developers are expected to adhere to, with examples and incentives to adhere to them.  
  * Release planning: starting with an architectural release that encompasses only a single important scenario, determine what successive releases will include until you have the final version that encompasses all scenarios considered. Time the releases such that you have margin for error and time to develop in between, and make them coincide with documentation and testing duties.  
* The output should be the first draft of a fully working system.

##### Evolution

* This is the essence of the design/production cycle: design is a creative process not especially concerns with constraints while production implements the design within the constraints present.  
* By producing a working system, then profiling to find where the most value can be added at the next iteration (Faster? Better interface? Better Graphics?) a plan can be made for the next micro cycle.  
* Every micro cycle ends with a release. For moderate projects (12-18 months\[\!\]), a new release may be completed every few weeks whereas for bigger projects a new release might only come every six months \[\!\].  
* Releases first go to the quality assurance (QA) team and then to alpha and beta testers with controlled expectations and guidance on what aspects of the system are to be tested.  
* Internal releases to the development team would typically happen every week or so.  
* Milestones should take days \- maybe a week or two at most \- to reach.  
* Planning for regular releases ensures closure of the micro cycle.  
* *“It is tempting to allow undisciplined change to class hierarchies, class protocols, or mechanisms, but unrestrained change tends to rot the strategic architecture and leads to thrashing of the development team.”*  
* Many changes are to be expected in this stage. Adding new classes and changing the implementation of a class come at a low cost; changing the representation of a class at a larger cost; changing the interface to classes or their structure is larger still (though rarely avoidable); *“the greatest risk is major architectural change, which can sink a project. Often, this change results from bright engineers with too many good ideas.”*  
* Measuring the rate of change of defect discovery and architectural changes should be a priority. Many changes to the architecture is a warning flag that signals greater risks down at the next release.

##### Maintenance

* This is the stage of collecting and ironing out bugs, and deciding which of the shelved developments might make it into the next release.  
* There is a big difference between *maintenance* and *preservation* whereby an existing system is being patched up all the time and the value added by doing so rarely exceeds the effort required. Tough decisions should be made about the future of systems in preservation.

## Pragmatics

* Two developers will give you two very different solutions to the same brief. In other words, there is a lot of flexibility required when designing and implementing a complex system such that management and practicalities cannot and should not be ignored.

### Management and Planning

* Risk management includes nontechnical risks as well as technical ones. Nontechnical risks are things like ensuring that third party vendors deliver their good on-time.  
* Task planning allocates resources (mostly time or man-hours) to tasks with a view to estimating the delivery date for the milestone. It is important that developers are asked to estimate the time a task will take, and that this be recorded, so that the accuracy of every developer’s estimates can be quantified. This is not an issue of trust but the acknowledgement of reality and a tool for making reasonable corrections to developers’ estimates. \[Joel “On Software” Spolsky makes the same point.\]  
* Walkthroughs are a tried-and-tested tool but can be overused. Weekly informal reviews focus on small clusters of code (such as a class category) whereas less frequent reviews look at the architecture of the system to identify places where it could be improved. (Informal reviews also give an opportunity for senior members to instruct junior colleagues.)

### Staffing

* Resource allocation in OO systems is usually more efficient than when using traditional methods. Integration in OO is (or should be) continuous and the quality of the product ought to be higher.  
* Development teams often contain three central roles:  
  * The software architect (usually singular) should have prior experience of “architecting” software in that domain but also some experience of programming, too. They should be involved for almost the whole life cycle of the software so that they can feel the implications of their own architecture decisions.  
  * The subsystem lead is responsible for a subsystem or class category (e.g. a package or library) and is usually an experienced programmer, though not necessarily an expert in a given language or domain. One third to one half of the development team will be a subsystem lead.  
  * The application engineer is closest to the coal face. They will have more experience and expertise in a given language and will therefore understand its idioms and idiosyncrasies. They are responsible for creating, testing and perhaps integrating individual classes in the subsystem.  
* There are other roles and there may be a many-to-one or one-to-many relationship between roles and people.  
* Fewer and better developers tends to lead to better teams; don’t assume a bigger team is better. \[See also the Mythical Man Month.\]

### Release Management

* Software systems should be integrated gradually and continuously; there is no “Big Bang” event.  
  * Interfaces need to be defined at the outset and carefully controlled while the implementation can be filled in later when every subsystem lead knows what they can expect from other subsystems, and what other subsystems expect from them.  
  * \[This tallies with Fred Brooks’s assertion that software is not built but *grown*. I like to think of the analogy with a baby that enters the world doing nothing useful, just eating, sleeping, crying, pooing and generally staying alive. It is the potential that keeps us invested in the baby’s development, and the human changes certainly but almost imperceptibly from one day to the next.\]  
* There are often different kinds of releases, such as a stable release to the public and also internal releases that are not seen outside the organization.  
* Version control is essential and the unit of versioning should be the class categories (groups of classes) rather then classes themselves.  
* Testing should also be under version control and proceed at three levels:  
  * Unit testing, class-by-class and the responsibility of the implementing developer.  
  * Subsystem testing, group-by-group and the responsibility of the subsystem lead  
  * Integration testing, the whole system and the responsibility of the quality-assurance team. 

### Reuse

* Code reuse must be institutionalised if it is to be successful. Take a long-term view of any project and aim to produce code that can easily be reused in other projects. Reward team members who consistently produce code that is reused by others.

### Quality Assurance and Metrics

* Look at the rate of defect discovery with respect to time; it should resemble a bell curve for any given release. Some projects have a very heavy tail, perhaps never approaching zero.  
* Lines of code written is a terrible metric with which to judge or compare between developers or releases.  
* Other metrics are better such as the depth of an inheritance tree, coupling between objects and cohesion in methods.

### Documentation

* Documentation for users should focus on how to operate the system, obviously.  
* For developers, the most important documents are:  
  * A high-level architectural document, explaining why it is structured the way it is and the strategic decisions that went into the design  
  * Key abstractions and mechanisms  
  * Scenarios that illustrate the behaviour of the system and its key aspects  
* Don’t waste loads of time documenting every method of every class \- most of it is guff and will barely be looked at.  
  * \[I wonder if this still applies in the age of documentation tools. I often need to look at a class’s interface documentation when using a package.\]

### Tools

* Tools that produce architectural or design documentation from source code are indispensable for when developers change the design without updating the documents.  
* Something that allows you to navigate the class hierarchy is useful. \[Most modern IDEs do this now.\]  
* Incremental compilation that allows you to quickly update the binary in response to minor changes is essential. \[I don’t know anything about this now.\]  
* A good debugger will let you see inside classes, and a really good one will support debugging in multiple threads.  
* Tools that stress test code are also handy (trying to write to unavailable memory, etc.)  
* Version control tools are essential \[and now widespread\].  
* A class library that helps you discover existing packages for a specific purpose are very valuable, since the effort required to find an existing package is often overestimated while the time taken to implement the functionality from scratch is underestimated.  
* A GUI builder is the final tool you need; writing GUI components from scratch in code is not fun.  
* Managing these tools requires a class librarian and a toolsmith. You can either invest in these roles explicitly and under some control, or the work will be done in a more ad hoc, and less efficient and effective, fashion anyway.

### Special Topics

* Prototyping when designing a GUI is essential; it’s too hard to get it right the first time.  
* Other considerations are necessary when dealing with databases, real-time systems (whatever the application defines as “real-time”) and legacy systems that are too risky to replace in one go (they should be wrapped and functionality replaced a bit at a time).  
* OOP can be promoted by introducing it to junior programmers who then propagate it sideways and upwards. \[OOP is no longer a new concept, so this barely applies now.\]

### Benefits and Risks of OO Development

* \[Many of the risks here are unlikely to crop up in modern applications, in my opinion, though the benefits remain valid\]

## \*\*\*SECTION 3 Applications

## Data Acquisition: Weather Monitoring Station

### Analysis

* This spells out the requirements (explicitly in the form “The system must…”) and then decomposes the system into its various actors, taking care not to introduce much coupling.  
* Scenarios are then spelled out, step by step, describing the sequence of events that take place for each of the use cases derived from the requirements. These event sequences can be turned into state machine diagrams.

### Design