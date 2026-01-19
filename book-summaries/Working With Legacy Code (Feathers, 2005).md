# Working With Legacy Code (Feathers, 2005\)

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

Slightly less care is required if you have an automated refactoring tool that you can trust. (Most IDEs have one nowadays; if yours doesn't then switch to one that does.) When using automated refactoring tools to Extract Method, however, it is important that you use *only* the tool and make no manual changes yourself. This ensures that you can have confidence that the changes you make have already been checked (which won't be the case if you're changing code by hand yourself).

