# Design Patterns (Gamma et al, 1994\)

### Introduction

* Each Design Pattern has a name, describes both the problem and the solution, and discusses the consequences of applying that particular approach. They describe the actors (objects and classes), who collaborates with whom, and who is responsible for what.  
* DPs can be classified according to their purpose: Creational, Structural and Behavioural  
* They can also be classified according to their scope: Class patterns revolve around inheritance and subclasses, and are therefore static (set at compile time); Object patterns defer action to other objects and are therefore dynamic (set at run time).  
* The principle of encapsulation states that an object takes responsibility for its data and that data should not be changed by other objects (unless express permission is given).  
* The *interface* to an object describes the requests that can be made of it and the return values. This is the only access that the outside world has to the encapsulated data inside, objects may implement several interfaces, and an interface may have several implementations. The principle of polymorphism states that the decision over which specific implementation to use can be deferred until run time by referring only to the interface of the class.  
* It is important to make the distinction between *type* and *class*. The type defines what operations the object can perform (closely related to its interface); the class defines how the object will perform these operations (closely related to its implementation).  
* **Interface inheritance** means that one object (a *subtype*) can be used in the place of another; **class inheritance** means that the implementation of one object depends on that of another (usually a *subclass* or *superclass*). In C++, for example, you can inherit publicly (allowing the client to call *DerivedClass::BaseClassMethod()*, making *DerivedClass* a *subtype*)  or privately (such that *DerivedClass* can call *BaseClassMethod()* within its own implementation but the client cannot call *DerivedClass::BaseClassMethod(),* making it a subclass).  
* **Program to an interface, not to an implementation**. This principle minimizes the assumptions made about a class at run time and therefore maximizes the flexibility of the code (because new implementations can be added later just as long as they conform to the assumed interface).  
* **Favour object composition over inheritance.** *Inheritance* (white-box reuse, “is-a”) often allows the derived class to see the internals of the base class via protected or public members; in *composition* (black-box reuse, “has-a”), the internals of the composite parts are hidden (encapsulated).Inheritance therefore binds the implementation of the parent class and all of its subclasses, which effectively “breaks encapsulation.”  
* **Delegation** is an extreme form of object composition in which an operation of class X is delegated to a component Y that is a member variable (“X has-a Y”). In the example given, a Window class has an Area() member function. Under inheritance, Window could inherit from Rectangle and use the parent Rectangle.Area() method. Under delegation, however, Window would have a member of type Rectangle to which it would delegate the Area() call, i.e. Window.Area() would simply return the value of Window.Rectange.Area(). More generally, Window would have a member of type Shape whose interface would be implemented by Rectangle but that could also be implemented by Circle. This would effect a round Window by delegation rather than having to implement RectangleWindow and RoundWindow subclasses as you would with inheritance.  
* A similar effect can be achieved with *parameterized types* (e.g. templates in C++) but these are not much considered here.  
* Aggregation implies that one object (or method) is responsible for another object (e.g. local variables in a class method); acquaintance implies that one object merely knows about another that exists independently (e.g. arguments passed by reference to a class method). Acquaintance (run time) is more loosely coupled than aggregation (compile time) and therefore more flexible though often more complex.  
* Design patterns encourage weaker assumptions in the code (e.g. the assumption of a concrete class, algorithm or platform, or access to source code) and therefore greater flexibility and the ability to adapt to changes in the requirements.  
* Code reuse takes different priorities depending on what you’re building  
  * **Applications** put greater priority on *internal reuse* to minimize the costs of redesigns.  
  * **Toolkits** (a collection of related classes that come in handy) emphasize *code reuse* in many different applications.  
  * **Frameworks** specify an architecture for applications within a given domain, and therefore emphasize design reuse; the developer effectively fills in the gaps. Because of this, changes to the underlying framework can have substantial consequences, which makes them the hardest of all to design.  
* The recurrent theme in this book is that you should ***encapsulate the concept that varies***.

### A Case Study: Designing a Document Editor

* (Detailed discussion about how a WYSIWYG document editor might be implemented and which design patterns might be relevant.)  
* **Structuring** the document (using the *Composite* pattern)  
  * Use a recursive composition of glyphs (including glyphs within glyphs, and so on). A Composition object contains a collection of child Glyph objects and, crucially, the Composition object is a subtype/subclass of Glyph itself. This ensures that a Composition can be a child of another Composition while also being treated as if it were just another Glyph.  
* **Formatting** the document (using the *Strategy* pattern)  
  * Use a Compositor to handle the formatting algorithm so that the algorithm is decoupled from the objects that it formats. When a Composition needs composing, it delegates the job to the Compositor rather than trying to format itself.  
* **Embellishing** the user interface (using the *Decorator* pattern)  
  * Could be done with inheritance: to add a border to a window, use BorderedWindow(Window); to add a scrollbar to a window, use ScrollableWindow(Window). This would, however, lead to an explosion of classes: ten embellishments for five base objects would yield 50 derived classes.   
  * Instead, create a Border(Glyph) object that contains the Glyph to be bordered. Making the Border a Glyph ensures that it is treated in exactly the same way as any other Glyph while adding the embellishment.  
  * Making Glyph a child of Border (rather than the other way around) ensures that you don’t need to mess with the Glyph code when extending the list of embellishments (e.g. to add a scrollbar).  
* **Supporting multiple look-and-feel standards** (using the *Abstract Factory* pattern)  
  * Designing an application that can follow different standards for appearance (and be easily extended to include new standards as they are invented) requires you to avoid constructing concrete classes directly in the code.  
  * Instead, construction is delegated to an instance of the *Abstract Factory* class that is responsible for constructing members of a family of objects (rather than a single object). There will be one concrete factory for each look-and-feel family, which can be selected at compile-time or run-time and easily switched.  
* **Supporting multiple windows systems** (using the *Bridge* pattern)  
  * Any Windows class hierarchy will implement the different types of Window that serve different purposes. The Window implementation, however, will be different on different platforms (e.g. MS Windows vs Linux vs …)  
  * These two orthogonal variations (purpose and platform) suggest two class hierarchies: one hierarchy for the interface (purpose) and one for the implementation (platform).  
  * The base Window class would contain a base WindowImpl class (that provides an interface for the implementation) which is made concrete later on. The base Window class then implements its functions in terms of the WindowImpl methods (that can be more extensive than the API).  
* **User operations** (using the *Command* pattern)  
  * Often, what you want to do is independent of the means of doing it (e.g. to Save a document, you might use the menu or click the button or press Ctrl+S \- it really shouldn’t matter). Coupling the request with the handler is bad and will result in unwieldy class hierarchies.  
  * The solution is, of course, to abstract away the request within a generic command class (rather than a function) that each of these shortcuts (menu, button, keystroke) stores as a member and calls in order to perform the common function.  
  * As an added bonus, implementing an operation with a Command.Execute() function suggests a reverse of the operation with a Command.Unexecute() function that restores the previous state, thereby providing a simple implementation of undo/redo functionality.  
* **Spell-checking and hyphenation** (using the *Iterator* and *Visitor* patterns)  
  * Spell-checking and hyphenation require us to scan the text one word (or perhaps character) at a time \- in other words, to iterate over a sequence.  
  * Since we do not want to couple the implementation of the sequence (it could be a list, a vector, an array, a linked-list, etc) with the specific iteration (forwards, backwards, or some other quirky form \- one that skips vowels, for example) we create a hierarchy of iterator algorithm classes and access them through the base class (abstract) interface.  
  * The operation you apply to every item in the sequence will depend on both the request (spellcheck, hyphenate, etc) and the object to which it applies (a character, an image, etc).   
  * Rather than add every possible operation to the object base class or resort to cumbersome switch statements, create a concrete class for the operation \- a *Visitor* \- that contains a method for every subclass in the object hierarchy and pass that operation instance to a method (e.g. Accept()) in the object base class. Each object subclass then overrides Accept() in order to call the corresponding method in the Visitor instance.  
  * The Visitor pattern therefore is suitable for stable class hierarchies where new subclasses are unlikely to be added (which requires an update to all Visitors)

## Creational Patterns

These will be illustrated using a Maze model consisting of rooms with walls and doors. Different creational patterns will be used to demonstrate how code can be reused and extended most effectively.

### Abstract Factory (class)

* **Concept that varies: family of products**  
* Where an application uses a collection of objects (products), all from the same “family” (e.g. when themed), you can create a concrete Factory class with methods that instantiate each product.  
* When different families are to be implemented, each family has its own concrete factory, and all concrete factories implement the interface of the *Abstract Factory* class.  
* All products from the same family are allowed to refer to each other as concrete instances because the factory approach does not allow you to mix and match.  
* The Client need not know what family of objects are being created \- all variants of the same product share the same abstract interface, as do all variants of the same factory.  
* Because this requires a separate method for every product, it can be unwieldy to maintain (e.g. when new products are required). Use a Factory Method or Prototype allows you to implement a single creational method (e.g. “make”) that takes the product type as a parameter. This does, however, make life difficult in statically-typed languages when product interfaces differ greatly (because the return type must be known in advance).  
* A concrete factory is often also a *Singleton*.

### Builder (object)

* **Concept that varies: the product, given a fixed set of inputs**  
* When you need to vary the product that is assembled *from the same raw materials*, you have a *Builder* class for each product.  
* The *Builder* (such as a TeXBuilder) becomes a parameter for the “director” (the client class, such as a TextFileReader, that calls the builder and controls the building) such that changing the *Builder* parameter changes the finished *product.*  
* The same *Builder* class can then be used by multiple director classes.  
* In the Maze example, the Builder class contains methods for building every part of a Maze (e.g. Room, Wall, Door) that are called by the director in order to construct whatever Maze it wants. Importantly, the *Builder* takes care of updating the relationships between components in the Maze’s internal representation.  
* The key feature of the builder class is that it maintains the object *that is being created step-by-step* until the director requests it. In the Maze example, the Builder provides the methods to add rooms, walls and doors to the Maze. \[This differs from the way in which projects I’ve worked on have used the Builder class.\]

### Factory Method (object)

* When your Creator class doesn’t know what kind of Product (i.e. the ConcreteProduct) it needs to create, delegate creation to a Factory Method that is defined in a subclass of Creator (e.g. ConcreteCreator).  
* An Abstract Factory is effectively a collection of Factory Methods, though Factory Methods can be included in concrete classes that do other things rather than having a separate Abstract Factory.  
* As with the Abstract Factory, creator methods can have a default implementation so that subclasses need only override specific methods, and creator methods can be parameterized or templated.  
* Do not call a Factory Method from within the base class constructor because the subclass will not exist yet; use **lazy initialization** (checking for the existence of the product and creating one on request if it does not already exist) instead.

### Prototype (object)

* Factory patterns have a habit of spawning large class hierarchies which can be unwieldy.  
* The Prototype pattern uses virtual construction via a clone() method of all Prototype instances. These may well be instances of the *same* class with *different* internal parameters, which provides a rich family of objects that behave differently without the need for a huge class hierarchy.  
* The Prototype pattern is especially useful for flexibility at *run-time*. In some languages, for example, Prototypes can be included at run-time in order to extend a framework.  
* A prototype manager (recommended) looks a lot like an Abstract Factory, only it uses clone() to instantiate new objects rather than creating them from scratch.

### Singleton (object)

* When you want only one instance of a class, use the Singleton pattern that hides the constructor (by making it private) and constructs itself, only if necessary, when the client requests access (i.e. lazy initialization).  
* A similar effect could be achieved using global variables, but there is too much freedom while simultaneously imposing constraints (some classes cannot be constructed at compile time).  
* The Singleton pattern can also be subclassed to make more specific instances that exist only in one place with a single point of entry, though this is trickier than it sounds. In cases where the type of Singleton to be created isn’t known in advance, you may need a register of the available types where a requested type is looked up and instantiated on request.

(Summary of creational patterns that is worth rereading to understand better the subtle differences and relationships between these patterns.)

## Structural Patterns

Structural patterns demonstrate ways of combining different classes to extend functionality. *Class* structural patterns do this via (multiple) inheritance; *object* structural patterns do it via composition.

### Adapter (class/object)

* The Adapter (or Wrapper) pattern modifies the interface of an implementation (the Adaptee) so that it conforms to another desired interface (the Target) for use by the Client. This allows interchangeability between implementations with different interfaces because they now share a common interface.  
* A class Adapter inherits publicly from the Target and privately from the Adaptee, giving the Adapter access to the Adaptee’s methods, while hiding the Adaptee’s methods from the Client who can access the Target’s methods.  
* An object Adapter inherits publicly from the Target and contains an Adaptee instance whose methods the Adapter calls in its implementation.  
* The class Adapter makes it easy to override a concrete Adaptee’s methods and requires less indirection, but makes it harder to adapt subclasses of Adaptee (because the Adaptee is concrete); the object Adapter does the opposite (because it usually stores a pointer to Adaptee and will therefore treat subclasses of Adaptee equally with no modification).

### Bridge (object)

* The Bridge pattern separates the interface (e.g. related to the purpose) from the implementation (e.g. related to the platform) by maintaining a class hierarchy for each.  
* In their example, the authors consider a window class with purposes (such as IconWindow and TextWindow) and platforms (such as XWindow and PM). In this example the purpose-specific interfaces contain an instance of the platform-specific implementations and use the abstract methods in the implementation class.  
* Among the advantages it reduces the number of classes needed for M purposes and N platforms from NxM to N+M.  
* Another pro is that it allows easy switching of the implementation, even at run-time. For example, there are many ways to implement a collection and their efficiency is dependent on the size of the collection. When a collection reaches a certain size, therefore, you can switch from one storage method to another to ensure maximum efficiency.

### Composite (object)

* In a tree structure, there are Primitives and Composites (collections of Primitives) that represent the leaves and nodes of the structure, respectively. The Composite pattern defines a Composite as a subclass of Primitive (recursive composition) so that the client need not be aware of which class it is working with.  
* Methods specific to Composites (Add, Remove, etc.) are declared and defined in the Primitive class (with null behaviour) and overridden in the Composite class for transparency; to do otherwise would mean the client would need to know whether it was dealing with a Primitive or a Composite.  
* The alternative is to declare those methods only for the Composite class for safety, but this breaks the beauty of the Composite pattern.

### Decorator (object)

* Inheritance extends the functionality of a class; a Decorator extends the functionality of *one instance* of a class.  
* Using the Decorator pattern keeps class hierarchies small and provides a lightweight way to add functionality.  
* Both the Decorator and the Component being decorated are derived from the same base class. A concrete Decorator must have the same interface as the concrete Component it is designed to decorate so that it is transparent to the Client (i.e. the Client needn’t know whether they are dealing with a decorated or undecorated Component).  
* The Decorator forwards requests to the Component it decorates, with added operations before or after that do the decoration.  
* Using composition means that any combination of decorations can be applied without a combinatorial explosion in the number of classes.  
* The authors refer to decoration as changing the “skin” of an object without altering the “guts.” In contrast, the Strategy pattern changes the “guts” rather than the “skin.”

### Facade (object)

* 

### Flyweight (object)

* 

### Proxy (object)

* 

## Behavioural Patterns

### Chain of Responsibility (object)

* Like the baggage reclaim at an airport. Put a suitcase on the belt (send a request) and let it pass by various travellers who will either claim (responsibility for) it or reject it because it is someone else’s responsibility.

### Command (object)

* 

### Interpreter (class)

* 

### Iterator (object)

* 

### Mediator (object)

* 

### Memento (object)

* 

### Observer (object)

* 

### State (object)

* 

### Strategy (object)

* 

### Template Method (class)

* 

### Visitor (object)

* 