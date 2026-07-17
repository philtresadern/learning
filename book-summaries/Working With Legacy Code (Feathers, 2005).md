# Working With Legacy Code (Feathers, 2005)

```
This book presents a number of scenarios encountered when dealing with legacy code (code without adequate tests), followed by a catalogue of refactorings that are specifically useful for untested code.
```

Feathers defines "legacy code" as anything that is not sufficiently tested, and therefore difficult to change. This might be the case for old code, but it can also be the case for fresh-off-the-press code.

Whether adding a feature, fixing a bug, refactoring or optimizing, the chances are that 99%+ of the behaviour should be unchanged. (In refactoring, the behaviour should not change at all.)

Tests \- and unit tests in particular \- act as a vice that clamp the code in place while you work on it. They alert you to when behaviour has changed when it should not have so that you can make the corrections to get it back in working order quickly.

Unit tests should each run in \<0.1s (at the time of writing, 2005). Anything slower than that is not a unit test.

As per other writers, unit tests should do everything in memory \- no touching file systems, databases, networks or peripherals.

The algorithm for making changes works in five steps:

1. Identify change points  
2. Find test points  
3. Break dependencies  
4. Write tests  
5. Make changes and refactor

**Note that making the changes is the final step with four steps before it.** You don't start changing the code until it is clamped in the vice of unit tests (some newly-written for the purpose).

A lot of the work goes into breaking dependencies, and this will come up later.

## Sensing and Separation

* Break dependencies to *sense* when we can't access values our code computes.  
* Break dependencies to *separate* when we can't even get a piece of code into a test harness to run.

Fake objects allow you to substitute a test class for a production class. The test class might cut out operations that are undesirable (e.g., accessing a database), replace parts that are undesirable so that we can check the result (e.g., writing to a file system) or simply instrument the test class so that we can analyse behaviour (e.g., count the number of times a method is called).

We do this by making the Object Under Test depend on an interface rather than an implementation, and then supply a test class that implements the interface in its own way.

## The Seam Model

* A *seam* is a place where you can alter behaviour in your program without editing in that place.

Look for \- and create \- seams in your code to make it easier to alter the behaviour from within the test harnesses themselves without changing anything in production.

Seams come in several flavours:

* Preprocessing seams work by substituting calls to one method with something more benign using macros.  
* Link seams apply at the point of linking and are typically implemented by linking to another library of functions (with the same names) altogether, used only for testing. They effectively swap the production environment for a test environment via the Makefile (or similar build configuration) and are useful for replacing methods that are called throughout the code base and therefore difficult to unit test.  
* Object seams are the most common and typically revolve around changing the arguments that are passed to a function or method (e.g., passing in a Fake Object) or around putting the behaviour you want to change into methods that you can override in a test-only subclass of the class-under-test.

(It is encouraging that I've used the latter technique myself without realising that it is a bona fide way to test classes.)

## Tools

There are many refactoring tools available, though you should use them with care \- many won't check whether the refactored code is truly identical to what you had before (e.g., checking that a newly extracted method does not have the same name as an existing method).

The xUnit framework is the most common for testing and is available in most languages (e.g., unittest in Python).

FIT (and fitnesse) are both web-based testing frameworks where tests cases are defined in HTML which is parsed by the framework and results written to HTML.

## "I don't have much time and I have to change it"

This chapter effectively introduces four ways to change the functionality of a class's method with minimal changes to the class itself (sometimes no changes to the class at all). These four methods can be expressed in a 2x2 matrix along these axes:

* You want to add new code inside/outside the method you're changing. By "outside" I mean before or after the method's code.  
* The class can/cannot be instantiated in a test harness (possibly not because it takes too long and too many resources to create within the time given to a unit test).

The first axis determines whether you sprout or wrap the method to change:

* "sprouting" a method involves creating a new method in the class, adding the new code to that method, and calling the method from within the method to change. This new method can be tested independently of the existing code using TDD, which is how you insert functionality into the centre of an existing method safely.  
* "wrapping" a method involves extracting the entire code of the method-to-change into a new private method and calling this private method from the existing method to change. You can then add new code before or after the call to the existing method as calls to new methods.

public existing\_method():  
    blah

becomes:

private \_private\_existing\_method():  
    blah

private \_new\_code\_before():  
    blah

private \_new\_code\_after():  
    blah

public existing\_method():  
    \_new\_code\_before()  
    \_private\_existing\_method()  
    \_new\_code\_after()

The second axis determines whether to create a new method or a new class. If the class under test can be instantiated quickly enough for a unit test, you can work with methods. If, however, the class is too big to test easily, you can create a new class, implement the new functionality safely with TDD, and then make a call to the new class's method from within the old class knowing that the new code is tested.

Again, this can be a "Sprout Class" move (inserting new code into existing code) whereby the new class is separate to the old class, with method calls providing the functionality or it can be a "Wrap Class" (adding new code before or after an existing method) whereby new the new class is a subclass of the old class that overrides existing methods.

When you wrap a class independently along several axes (i.e., overriding one method in each class wrapper) this is the *decorator pattern* and classes can be nested to wrap multiple methods without a combinatorial explosion in classes. As the nesting depth grows, however, the code gets harder to read so use this sparingly.

## "It takes forever to make a change"

This chapter is mostly focussed on compiled languages where changes to the implementation of a component requires recompilation of its clients. (This will not be the case in languages such as Python.)

Here the Dependency Inversion Principle is used extensively, replacing dependencies on concrete implementations with dependencies on abstract interfaces such that a change in the client does not require recompilation of the client.

The overall effect is that it takes slightly longer to rebuild the entire system from scratch (because there are more files and modules) but less time to recompile part of an already built system (because fewer modules will need recompilation).

## "How do I add a feature?"

The first part of this chapter is mostly about Test Driven Development and the Red-Green-Refactor cycle.

The second part covers Programming By Difference whereby you add a feature to an existing class by first subclassing that class and adding the feature to the derived class (thus preserving the original code and behaviour). The author then notes that this fails when you want to combine two features that have been added in separate subclasses (you can't), but also notes that the subclass-and-extend approach should be used as a temporary measure to get new code to Green before undertaking the Refactor step to find better ways to implement the new feature without breaking the tests.

This Refactor step might involve moving progressively more functionality to a new class, possibly to the point that the new class needs renaming to be more representative of its responsibility.

Wherever possible when subclassing, try to avoid overriding concrete implementations so that each class method has only one concrete implementation in its hierarchy (called a *normalized hierarchy*).

## "I can't get this class into a test harness"

This is concerned with classes that are difficult even to construct for test purposes (often because of code in the constructor).

Feathers notes that often the best way to see how hard it is to construct an object for testing is simply to write a test that does nothing but construct. It will fail if construction is problematic, but if all goes well then you can write your tests to test the object before moving construction to the setUp() method and deleting the construction test.

Now to the popular antipatterns...

First: "The Case of the Irritating Parameter" where one of the constructor arguments is a concrete class that is itself difficult to construct (or unappealing for unit testing, such as a database connection). This can be handled by replacing the concrete instance with an abstract interface so that you can pass in a Fake Object during testing.

(During the discussion, Feathers asserts that test code needn't live up to the exacting standards of production code such that exposing internal members of Fake Objects, for example, is acceptable. He also isn't afraid to Pass Null into a method during testing *in certain languages* because in most cases any attempt to use the Null's members will simply fail the test; other languages such as C/C++, however, will plough on and do nasty things with Null references so he cautions against Pass Null in these languages. He definitely advises against Pass Null in production code, preferring instead the Null Object pattern unless it would have side effects.)

Second: "The Case of the Hidden Dependency" where a constructor itself constructs an instance of another object, creating a dependency that can't be controlled in the test. The answer here is to replace the construction with passing in an argument (preferably an interface) that can be a Fake Object for testing purposes. This *Parameterize Constructor* operation is Feathers's preferred approach to breaking dependencies in many cases \- simple but effective.

Third: "The Case of the Construction Blob" where a constructor uses its arguments to construct other objects such that using Parameterize Constructor would place a burden on the client of the class. In this case, you can apply the *Supersede Instance Variable* pattern in a test-only subclass of the class under test. This gives tests (and only tests) the ability to replace one instance variable's value with a new value that can be instrumented in a way that the production object could not.

Fourth: "The Case of the Irritating Global Dependency" where classes throughout the code base access a global variable (often via a Singleton pattern). These are particularly tricky to Fake but one approach is to add a method to the Singleton that allows you to replace, for test purposes only, the object it manages. Another approach uses inheritance to replace the original Singleton with another whose methods are overridden, and where the Singleton takes an interface to a class rather than an instance of it.

Fifth: "The Case of the Horrible Include Dependencies" applies mainly to C++ where you need to get your includes in order before you can even instantiate a class for testing, and that can be difficult in its own right.

Sixth: "The Case of the Onion Parameter" feels very familiar \- layer upon layer upon layer of dependencies such that you have to do a lot of work just to create an instance of the class under test. The answer, of course, is to Extract Interface so that you can pass in a Fake Object that does just enough to exercise the class under test.

Seventh: "The Case of the Aliased Parameter" occurs when a parameter to a constructor is itself at the bottom of an inheritance hierarchy, making it impractical to use Extract Interface (because you'd have to Extract Interface for every level of the hierarchy). In this case, Subclass and Override Method comes to the rescue: you can create a Fake Object by subclassing the parameter in question, and (you hope) override the problematic methods (e.g., that access a database). If it isn't that simple, you'll need to refactor the parameter class so that it is that simple.

## "I can't run this method in a test harness"

Once you've instantiated your class (congratulations\!) you might find that you can't run a method for one of several reasons.

1. The Case of the Hidden Method: The method is private or protected, and you can't call it from the test harness. First, consider whether it needs testing independently. If it is a utility method then exercise it through the public methods that use it, as a customer or API user would. Second, consider why it is private in the first place. If it's just a helper method then there are ways to make it public just for testing; if it is private because it changes state in a way that ordinary users shouldn't be allowed to do, you can put it in its own public class (that can be unit-tested) and access it via a private instance of the class. If you really do want to test a hidden method, you can first make it protected (not private) then Subclass-And-Override to make it effectively public. This will enable you to test it, albeit at a cost of indirectly enabling users to call it (by doing the same thing) and by glossing over the fact that the class probably has too many responsibilities anyway. In short, S\&O but make time to address the underlying design flaw later.  
2. The Case of the "Helpful" Language Feature: some languages allow you to designate a class as non-subclassable ("final" in Java, "sealed" in C\#) so S\&O is no longer an option. The lesson here is to avoid depending on classes and features such as these. Instead, write your own classes (sometimes wrappers) that imitate the same functionality (implement the same interface) and use these instead. (I admit this section didn't make loads of sense, but so far it isn't something I've been troubled by.)  
3. The Case of the Undetectable Side Effect: Sometimes the method-under-test calls other methods that change state in ways that are not obvious, and because they're buried in the class it is unlikely that they will be instrumented in a way that allows us to see what's going on under the hood. The answer is to break up the method so that the classes and methods that it interacts with can be instrumented and checked, and also tested independently to check what they're doing.

Feathers also notes the distinction by Bertrand Meyer that methods should be either a command (changes state, returns nothing) or a query (does not change state, returns a value) but never both. I agree with this.

## "I need to make a change. What methods should I test?"

Here, Feathers suggests undertaking an Effects Analysis: drawing a diagram of the state (data) that is modified, the Commands that modify it, and the Queries that return the modified value. The resulting effect sketch should be simple; if it isn't, that's a sign of muddled code.

Next, think about the knock-on effects (primarily via the Queries that pass the modified value to other methods). Don't forget subclasses as clients, and clients of subclasses.

Effects propagate in one of three main ways:

1. Return values that are used by a caller  
2. Modification of mutable objects passed by reference into a function or method  
3. Modification of global variables

Encapsulation reduces the number of pathways you have to consider when looking at effects. Break encapsulation if it makes classes easier to test \- it's usually worth it.

## "I need to make many changes in one area. Do I have to break dependencies for all the classes involved?"

When instrumenting code or classes, look for *interception points*: places where it is easy to see the effects of modified code.

Sometimes these points can be at a relatively high level, meaning that you don't have to instrument lower level (often private) classes or class members. Sometimes adding high level tests can be the first step in working back toward unit tests.

*Pinch points* are the ideal interception points: places where there is a narrowing in the effects sketch such that you can test for the effect to changes of many methods in the same place.

Sometimes, looking at an effect sketch and seeing where the pinch points are can suggest improvements to the design by breaking off coupled code into a class of its own.

## "I need to make a change, but I don't know what tests to write"

Very often, legacy code has little or no testing whatsoever. In this case, the most important property of the code is that it keeps doing what it already done, and for this you need *characterization tests*.

Characterization tests (often known as regression tests) simply record what the code does at the time of writing the tests. They are used to catch unexpected changes in behaviour, regardless of whether that behaviour was correct or desirable in the first place. The aim is simply to *preserve* existing behaviour.

Bugs will be spotted along the way, but the purpose in the early stages is to wrap the code in scaffolding that fixes the behaviour while the code is refactored. Once the code is in better shape, you can start to make changes and fix bugs that you recorded along the way.

Often the easiest way to write a characterization test is to assert that a given value is equal to a value you know will be wrong: the assertion will tell you what the actual value was, and you can replace the target with the actual value in order to monitor changes in behaviour.

## "Dependencies on libraries are killing me"

Wrap them\!

## "My application is all API calls"

Extract the parts that aren't calls to an API and at least test those parts. This can come in two forms:

* Skin and Wrap the API: create your own class that calls the API with the same signatures, sometimes enabling you to instrument and test some of those calls with Fake objects. Often, however, you find that this has limited usefulness because the API is so closed/coupled that there are relatively few seams to exploit.  
* Responsibility-based Extraction: split the code containing API calls into classes with more specific and limited responsibilities so that you can at least test parts of the code, even if you can't test all of it.

## "I don't understand the code well enough to change it"

Notes and sketches can serves as useful conversation aids when discussing code with other developers. These needn't be formal or structured, and probably only make sense to those involved in the conversation when they were made, but they will serve the purpose for the time in which they are used.

Listing Markup involves printing off code and marking it up using coloured marker pens to better illustrate the responsibilities it has, what data is modified, etc.

Scratch Refactoring is where you refactor code purely for the purpose of understanding it better; the refactored code is thrown away at the end of the exercise.

Delete Unused Code does what it says on the tin. We have Git for long-term memory in code.

## "My application has no structure"

Try telling the story of the code in a few sentences, distilling it to the very essence of what it does. This can often give you some pointers as to what the structure should look like. Don't worry about leaving out some details, or not being completely accurate \- simplicity is the objective here.

Naked CRC (Class, Responsibility, Collaborators) uses cards (or some other tokens) with no annotation (in contrast to CRC) as a means to explain how a small group of classes interact. Simply place cards down on the table, point at them, and explain the workflow as they interact.

Conversation Scrutiny questions how well the structure of the code reflects conversations about the code; ideally, they should follow each other closely. For example, if you refer to a piece of code with a noun, is the code contained in a class with the same name? If not, why not? (The code would explain itself better if it did.)

## "My test code is in the way"

Use agreed conventions when writing and maintaining tests:

* test cases should end (or start \- ending is better) with "Test"  
* Fake objects should begin with "Fake"  
* Testing versions of classes under test should start with "Testing"

Think about where you want to put test code. Feathers advocates for putting test code alongside the classes it tests; Bob Martin argues otherwise (because it couples tests to production code). I prefer to keep them separate \- I believe you should be able to write tests for code that you don't control, which requires the two to be separate.

## "My project is not Object Oriented. How do I make safe changes?"

This mostly involves looking for the seams that are available (link or preprocessor) when object seams don't exist.

Use TDD to develop new functions before you insert calls to them from the existing code you want to adapt.

When the language you're using has an OO extension or equivalent (e.g., C++ for C) then try to move toward OO-based code wherever possible to exploit object seams. (OO code has other advantages such as when you come to extend it.)

## "This class is too big and I don't want it getting any bigger"

Classes should have one primary responsibility (or reason to change) and do little else, though identifying responsibilities is a skill that must be acquired.

Heuristics for identifying responsibilities include:

* Group methods by name (often looking at prefix or suffix values)  
* Look at private methods: more often indicates more than one responsibility.  
* Look for decisions that can change: is there anything hard-coded that might be better off separated into a class where an alternative might be needed in the future?  
* Look for internal relationships: not all methods use all variables; look for clusters using the diagram approach below.  
* Look for the primary responsibility: describe what the class does in a single sentence. If you have to say "and" then identify the most important responsibility.  
* Scratch refactor: mess about in order to gain knowledge while intending to throw away your refactored code.  
* Focus on the current work: if you are writing a different way of doing something that already exists (or that you are confident might need to exist in the future) then that responsibility should probably go in another class.

One approach to spotting multiple responsibilities is to group variable names and method names into clusters that appear related (often by a common suffix or prefix). Furthermore, you can draw a diagram \- variables on one side and methods on the other \- and draw lines between each variable and the methods that depend on it. This often has the effect of highlighting clusters and natural "seams" across which you can split the class into two or more classes, with delegation from the original class to the newly extracted classes.

When working on an Extract Class refactor, group the variables you intend to extract together and extract all the code you intend to move in new methods starting with a common prefix (e.g., "MOVING"). This will make it easier to see what needs to move when the time comes. Take care where inheritance is concerned: moving variables/methods that shadow variables/methods with the same name in a parent class will introduce bugs.

## "I'm changing the same code all over the place"

Duplication is a common source of bugs (changing code in one place but not the corresponding places) and removing it is one of the main reasons for refactoring.

Merging two classes with duplicated code into a common base/template class with only the minimally discriminating code in the derived classes will do away with much duplication while also drawing attention to the underlying design (and how it might be improved).

Do this carefully, though \- one small step at a time.

## "I need to change a Monster Method and I can't write tests for it"

Extremely long (hundreds or thousands of lines) "Monster" Methods present a particular challenge and require special care.

* Bulleted methods look like a bullet list - lots of sections of code that do different things and that should be easy to Extract Method but look out for temporary variables defined in unhelpful places
* Snarled methods have one single, heavily indented section and lots of branching, also ripe for an Extract Method but with care.

Slightly less care is required if you have an automated refactoring tool that you can trust. (Most IDEs have one nowadays; if yours doesn't then switch to one that does.) When using automated refactoring tools to Extract Method, however, it is important that you use *only* the tool and make no manual changes yourself. This ensures that you can have confidence that the changes you make have already been checked (which won't be the case if you're changing code by hand yourself).

If you do insist on doing the Extract Method by hand, make sure you (a) pass in all the variables as arguments (b) give it a unique name that doesn't override an existing method and (c) check the return values.

Insert sensing variables in such a way as to add (but not change) the code you are refactoring and check the state of the new variables in your tests. This will give you confidence that branches that were being visited before the refactor are still being visited after the refactor. Once the refactoring is to your satisfaction, you can remove them (and any assertions that depend on them in your tests).

## "How Do I Know That I'm Not Breaking Anything"?

Most manufactured artifacts wear out over time. Not code. Code will run and run and run until it is changed. But even the tiniest change can break the code with ease. Ensuring that the changes you make do not break anything is an essential skill (especially without tests, which is what Legacy Code is).

This can be achieved with a few key discplines:

* Hyperaware Editing: get feedback that is almost continuous so that you become incredibly aware of the effects that youre changes make. Some changes in the code effect no changes whatsoever in the behaviour (e.g., adding comments); others change the behaviour dramatically. Get feedback as quickly and as often as possible to ensure you get a "feel" for what you're doing to the code's behaviour.
* Single Goal Editing: change one thing - and only one thing - at a time in order to avoid going off on a tangent. If you spot another thing that "needs" changing, make a note to come back later and go back to the one goal you were already working on. \[In my recent experience, Conventional Commits are a good incentive to maintain discipline. A commit message must be either "refactor" or "fix" or "feat", and commits that span more than one of these goals feel wrong. Splitting the commit into multiple commits resolves the problem; not working on more than one thing at a time avoids the problem in the first place.\]
* Preserve Signatures: when extracting methods, keep the signature of the new method the same as the old to reduce the risk of introducing errors here (that are very easy to introduce and more difficult to detect). Besides, extracting the method and changing the signature break the Single Goal Editing discipline.
* Lean On The Compiler: refactor code in a way that is deliberately broken, then get the compiler to alert you (via compile-time errors) to all the other places in the code that you need to change in order to complete the refactor. Beware, however, of instances where the lack of compile-time errors simply indicates substituted behaviour, such as when meddling with the methods of a class hierarchy where deleting a subclass method simply falls back to the superclass's implementation. \[This discipline also obviously only works with compiled languages.\]
* Pair Programming: sit with another developer in order to share knowledge, pick each others' brains, and act as a regulator for each other (e.g., when pursuing Single Goal Editing).

## "We Feel Overwhelmed. It Isn't Going To Get Any Better"

Programming should be fun, whether you do it for money or do it for love. Find the way that gets the most joy out of working the code, whether it's a pristine greenfield project or legacy code; whether it's great code or terrible code. Any combination of these scenarios can leave you either elated or deflated. One way or another will leave you elated, and it usually involves getting at least a small portion of the code under test so that you can play around with it - having fun in the process - without an overwhelming fear of breaking something. Take away the fear, and only the fun will remain.

# Dependency-Breaking Techniques

These are refactorings with the specific intention of making it easy to introduce tests where no tests exist.

* Adapt Parameter: Where an argument/parameter to a function is a complex class but only a small portion of the interface is used, wrap the class in a narrower interface that can be substituted with a simple fake.
* Break-Out Method Object: Extract a complex class method as a class in its own right, passing the original class in as a parameter to access any instance variables or private methods (which will need to be exposed - shock! horror! - but this is a temporary measure to get tests in place before redesigning). The reference to the original class can then be faked to test the method object.
* Definition Completion: where declaration and definition are separated (e.g., in C++ .h and .cpp files, respectively) then reimplement the methods in the test file to simplify the test.
* Encapsulate Global References: where functions/methods access global variables, wrap the global variables in a class that can be faked in a test.
* Expose Static Method: where a class method doesn't reference any of its instance variables or methods, it can be extracted to a method that is static and public so that it can be brought under test. This also flags the method as being a good candidate for moving to another class, though this is a job for another time.
* Extract and Override Call: when you want to instrument or modify a specific function call in a larger method, extract the call to a method of its own and redefine that method in a testing subclass. (This is one of the author's favourite refactorings that he uses often.)
* Extract and Override Factory Method: when an object instantiates complex instance variables in its constructor, it can make testing the object cumbersome. Move the instantiations to a factory method that you can override in a testing subclass to return variables of your choice and test different scenarios.
* Extract and Override Getter: where a virtual factory method can't be used in a constructor (e.g., C++ forbids it), consider adding a lazy getter that does the construction only on first use and returns the result. This getter then needs to be substituted throughout the class wherever the objects being "got" are used, but can then be overridden in a testing subclass to substitute the return variables with test doubles.
* Extract Implementer: separate the interface and implementation of an existing concrete class by taking a copy, giving the copy a new name, removing all implementation from the original to make it an interface, and make the newly renamed copy implement the new interface. All existing references to what was a concrete class now reference an interface to a class, so the places that construct the concrete class will now need to rename the class they construct. Thinking of a good name for the new concrete class is often the hard part.
* Extract Interface: separate the interface and implementation of an existing concrete class by creating a new interface with a new name, adding empty public methods for the implementations used during testing, and making the existing concrete class implement the interface. Now change the code that references the concrete implementer to reference the abstract interface instead. Unlike Extract Implementer, here the interface (rather than the implementer) gets the new name that didn't exist before.
* Introduce Instance Delegator: clients that use a (utility) class's static methods can be hard to test because the utility class (effectively a namespace because all methods are static) looks a lot like a global variable. Adding instance methods to the utility class that delegate to the static methods and replacing calls to the utility class with calls to *an instance of* the utility class creates an *object seam* that facilitates testing (because the instance of the utility class can be mocked during testing).
* Introduce Static Setter: globals that return an object are hard to mock when testing, especially if they maintain system state that influences the order in which tests must be run. Add a static setter method so that - at test time, and only at test time (which you should document in the code) - you can change the object that the global returns and substitute a test double.
* Link Substitution: (as in "substitute at link time") are useful in procedural languages that don't have classes and objects. The idea is that you create a library of function doubles and link them into the tests rather than the production functions.
* Parameterize Constructor: where you have a constructor that constructs an instance of another class, make the instance-of-another-class a constructor argument that you pass in so that it can be substituted easily during testing. To avoid changing the interface, either make the old constructor call the new constructor with the original construction passed in, or keep the one constructor and use default argument values to specify the instance. The one downside is that clients using the new constructor must construct the dependent instance which introduces new dependencies, but that's a small price to pay for easy testing.
* Parameterize Method: same as above, only now you have the option of giving the newly parameterized method a different name.
* Primitivize Parameter: where you want to add a function to a class that is hard to bring under test, put the implementation in a separate function in a way that it (a) can be tested and (b) can be called by the hard-to-test class later on. (In the example given, a hard-to-test Sequence class extracts its own data into a primitive vector<> that is passed to a function that uses only vector<>s.) This refactoring makes a mess of the code and should be used sparingly. (In an ideal world, the class would be brought under test before adding any new features, but this is the real world.)
* Pull Up Feature: when you want to test a subset of a class's methods that have fewer dependencies than the class as a whole (e.g., methods operating on a list of items, where the class connects to a database purely to satisfy other methods), you can pull the simpler methods up into an abstract superclass, splitting the class in two: more "primitive" methods (amenable to unit tests) in the superclass and the more involved methods (requiring integration tests) in the subclass. You can then use a testing subclass to bring the simpler methods of interest under test, before considering further changes to the code (e.g., delegate external dependencies to a new class). When doing this refactor copy-and-paste to create two parallel classes before removing the derived class's implementations.
* Push Down Dependency: has the same effect as Pull Up Feature in that the more dependent methods go in a subclass and the less dependent methods in the superclass. This time, however, the new class is the subclass and you move methods down; Pull Up Feature creates a new superclass and pulls methods up. Either way, you instantiate a test subclass of the new superclass for testing.
* Replace Function with Function Pointer: in non-object-oriented languages, you can emulate polymorphism with function pointers. Replacing a call to a function with a call to a function pointer means you can change the function pointee at test time to introduce function test doubles.
* Replace Global Reference with Getter: where a class accesses a global reference (or something equivalent such as a Singleton), add a getter method that returns the global and use that in place of the direct reference throughout the class. You can then use a test subclass to override the getter and return an object of your choice.
* Subclass and Override Method: this is the Daddy of refactors, and many others are variations on this theme (often getting a class into a state where it can be usefully subclassed). Make heavily dependent methods protected so that during testing you can create a testing subclass and rewrite the dependent method to return a useful value without the dependencies. (I suspect that in this day and age, you could simply Mock the method to return the useful value.)
* Supercede Instance Variable: there are a few ways to replace a variable that is created in the constructor, though many are rather complex. (Some languages don't allow you to call virtual functions in a constructor.) If the instance variable constructed isn't used in the remainder of the constructor, a simple way to modify it during testing is to add a supercedeX(newValue) method to the class so that you can replace the instance after construction but before test. (The author points out that "supercede" is a sufficiently unusual word that you can search for it to find uses of it by naughty clients. What I haven't yet figured out is why you can't subclass the class under test and add the supersede method to the test subclass.)
* Template Redefinition: where templating (aliasing) is supported (e.g., in C++) you can change the type of an instance variable at the point of use, making it easy to insert a test double. The use of templates, however, does introduce other complications so it should be low on the list of preferences.
* Text Redefinition: some interpreted languages allow you to replace a class method with a new implementation on-the-fly. This is effectively Subclass and Override Method without the Subclass step.
