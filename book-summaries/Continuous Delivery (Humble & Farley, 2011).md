# Continuous Delivery (Humble & Farley, 2011\)

## Foundations

### The Problem of Delivering Software

Basically, this chapter drives home why the following principles and practices are “best” and persuades the reader to keep on reading.

Luckily, it is persuasive.

### Configuration Management

Setting up a new machine or environment \- whether development or staging or production \- should be as easy as downloading a script and running it. *There should be no manual intervention*. (If a third party package requires manual intervention, ditch it and look for an alternative that can be fully automated. It will save time in the long run.)

Keep all of these scripts (and the associated files) in version control so that there is a history of what worked and when, and so that you can easily roll back if need be. Use only the trunk branch (no side branches), check in regularly, and write meaningful commit messages.

Keep local copies of external libraries so that you always have access to them.

Keeping components together in one build is the simplest way to process for small to medium sized projects. Only when the project grows large should you split them into separate pipelines.

Configuration is as important to get right as source code. (The authors point out that they could stop your program working as easily by tampering with the configuration than with the code.) It can be integrated at build-, packaging-, deployment-, startup- or run-time (or all of those), though build/packaging time is problematic in that different binaries are deployed to different environments.

Don’t put passwords into version controlled files or source code \- it isn’t secure.

Configurations should be accessible from a central repository or perhaps a web server.

### Continuous Integration

Before CI became a “thing,” software would be build every night and there would be a worker on-call to deal with problems. Nowadays, CI ensures that software is rebuilt and tested every time someone commits to the repository.

Essential practices:

* Don’t commit to a broken build: if the build is broken, someone will (or should) be trying to fix it. Adding a new commit makes their job a lot more difficult. Wait until it’s working again.  
* Run all commit tests locally before committing: avoid breaking the build by testing before you commit.  
* Wait for the commit tests to pass before moving on: don’t get to work on the next feature or story if you don’t know you have a working build. Have a cup of tea and a think while the tests run. (This is why it is important to have tests that are fast.)  
* Never go home on a broken build: leaving a build broken for hours at a time is especially bad, particularly if you work with colleagues in another part of the world whose working hours are very different to yours. Check in regularly, run fast tests to give confidence that the build will work, and don’t commit late in the day (and especially on a Friday).  
* Be prepared to revert to the previous revision: if the fault isn’t obvious and easily fixed, roll back to the previous revision while you investigate the problem. This will enable the team to continue using a working build.  
* Time-box Fixing before Reverting: have a time limit on how long you will spend trying to find and fix a broken build before rolling back.  
* Don’t comment out failing tests: this should be obvious, but there are also steps you can take such as marking a test as “skipped” so that there is at least a record of how many tests aren’t being run (it should be very low, preferably zero).  
* Take responsibility for your breakages: this is also obvious, but requires that all team members have access to all the code so that you can see how you introduced a regression error. (This is necessary, for example, when your tests pass but someone else’s fail because of a change you made.)  
* Test-driven development: write the tests before you write the code. This ensures that all code is tested thoroughly and leads to better quality code (and tests).

Suggests practices:

* Extreme Programming techniques (see Beck).  
* Fail tests for architectural breaches or slow tests: although tests may pass, they may accept behaviour that should not be accepted so don’t be afraid to make the test fail for not meeting certain structure or performance criteria.

Think carefully if working in a distributed team because the communication aspects will be much harder to overcome.

Distributed version control (e.g., Git) is now becoming the norm but can come with headaches (especially where distributed teams and forking is the norm).

### Implementing a Testing Strategy

Tests come in two types of two flavours: business-facing vs. technology-facing; and those that support the programming vs. those that critique the project.

Developers are more concerned with tests that support the programming while customers are more likely to be interested in those that critique the project.

Acceptance tests are the bedrock \- everything below these (unit tests, component/integration tests) are in service to acceptance tests. All tests should ensure that no backward progress has been made. Acceptance tests tell developers when they are done, and customers when they have what they want.

Acceptance tests should be written collaboratively between customers, developers and testers, and written in code before any development takes place. They are the ultimate documentation.

Every test has a “happy” path \- the ideal sequence of events that the user would complete to undertake the task; tests should exercise this path first. There are also numerous “sad” paths where the user might make a mistake; these paths should be exercised in tests later on if time is short, but they must be exercised at some point.

Aim for \>80% coverage at every level of testing.

Any user interface should not contain business logic, but should access the business logic layer via a public API. \[I presume the ideal API would accept commands in the form of strings, for example, such that an action in the UI would be mapped to a string. This would also facilitate UI testing in that API commands would be sent to the software at exact times to simulate user interactions.\]

Unit tests should not interact with external services such as file systems, databases or web services \[e.g., licensing managers\]. Component tests are where interactions between components should be tested. (The authors dislike the term “integration test” because they feel it is overused and has become somewhat ambiguous.)

Deployment tests ensure that the system is correctly installed and configured, and can access all the services it needs after deployment.

Manual tests (e.g., business-facing tests that critique the project) include showcases, usability testing and exploratory testing and, by nature, are difficult to automate. They can involve sitting testers down at a machine and recording the way they use the software to see where it can be improved.

Some organizations release different versions of their software in a form of A-B testing (known as “canary releasing”) where different versions can be compared.

Nonfunctional tests (technology-facing, critique the project) don’t test what the software does as much as how it performs in different environments. Typically, they stress test capacity, scaling, and so on.

Test Doubles are an effective way to undertake unit testing that keeps components isolated from each other. Doubles come in many forms \- Dummies, Fakes, Stubs, Spies and Mocks \- and the simplest version should be chosen for the task. (See “Mock Roles, Not Objects” and “Mocks Aren’t Stubs” for more info.)

\[This could be where using factories comes into its own: the test framework would create a Test Double, register it with the factory under a new name, and pass the name to the object under test so that the test double is automatically created using the existing system.\]

New projects are the best time to create tests because the testing can be instituted from the very beginning. In this ideal case: customers \+ analysts \+ testers define acceptance criteria; testers \+ developers write acceptance tests based on acceptance criteria; developers write implementations that fulfill the acceptance criteria; any tests that fail are fixed as a priority.

Midproject is a time for filling out any existing automated tests, and for automating many manual tests. Expect some resistance from customers and managers who don’t see the business value in adding tests.

Legacy Systems are defined by some as “any code that doesn’t have tests.” Start with “smoke tests” that cover core functionality, then add more tests to carefully check the code you intend to change. You’ll probably find that the structure does not lend itself to being tested; one of the advantages of test-driven development is that code written to be easy-to-test tends to have a better design and architecture.

Integration/Component testing involves checking that your components can access the external services they depend upon. You’ll need Doubles of these services, or at least a configurable version of the production service that knows when it’s being tested. A test harness (see “Release It\!”) should cover both “happy” paths and conditions where the service doesn’t respond in the expected way. \[We should probably have a licence manager class in PoPy, for example, that can be replaced with something that responds with an error message.\] Service Doubles should also replicate erroneous situations that *aren’t* meant to happen.

Creating acceptance/customer tests is a collaborative process, sometimes using tools (Cucumber, JBehave, Concordion, Twist, etc) to turn text into runnable code. Domain-specific language (DSL) can also be helpful here. A quick feedback cycle can avoid the problem where the next story is well underway when a bug is found in the previous story, creating tension in the teams where one (testers) wants to stop development and the other (developers) doesn’t.

Managing a defect backlog is crucial, though ideally you won’t have one. Fix bugs as soon as you know about them, especially if they are of high risk or high impact (“critical”) rather than “blockers”, “medium” or “low” priority (those that are rare, have low impact or have an easy mitigation). Sometimes, a high impact feature may be prioritised over a low risk/impact bug; this is okay.

## The Deployment Pipeline

### Anatomy of the Deployment Pipeline

The deployment pipeline consists of a number of stages, each of which takes longer than the last (slower feedback) but also tests the system more thoroughly (greater confidence that it works).

At the Commit Stage, you compile the code, run unit tests, analyse the code base (e.g., test coverage) and build any installers. \[This is supposed to take a few \- single digits \- minutes but can take longer in my experience.\]

The Acceptance Test Stage runs harder and slower tests after configuring the environment and deploying binaries to a test environment.

Manual Test Stages require user intervention (i.e., human testers); Capacity Test Stages check how the system performs under different scaling and loading conditions.

Finally, the Release Stage deploys binaries to a production environment where “smoke tests” are run. (These are like unit tests in that they run quickly, but they check that end-to-end operations run to completion.)

At each stage, the development team receives feedback in the form of reports and metadata (how many tests failed, how long they took, etc.) and the pipeline is triggered automatically when a developer commits to the source control repository. Each stage in the pipeline should be triggered automatically when the previous stage completes successfully; if a stage fails then nobody commits anything except to fix the problem and get the stage working again.

##### Practices

* Build Binaries Once: don’t have multiple versions of binaries for different environments \- it’s hard to keep track of whether what passed tests before is the same code that is failing now. Any environment-specific behaviour should be detailed in a configuration file that the binaries read and act upon.  
* Deploy the Same Way to Every Environment: don’t use a different deployment script for each environment; any environment-specific settings should be in a configuration file that is processed by the deployment script if needed.  
* Smoke-Test Your Deployments: check that the software starts as expected, and that it connects to any third party services that are needed in production.  
* Deploy into a Copy of Production: have a “dummy” production environment set up that is as close as possible to the actual production environment so that you can be confident everything will work at the user’s end.  
* Each Change Should Propagate Through the Pipeline Instantly: automated stages should recognize when the code base has changed every time they reach completion, and should start again straight away if the code base has changed. (Some systems abort a job when the code base has changed, avoiding time wasted checking an old version.)  
* If Any Part of the Pipeline Fails, Stop the Line: any stage that fails means the build is broken, so stop testing it and start fixing it.

##### The Commit Stage

The more errors you can pick up during unit testing the better; if the unit tests pass and an integration test fails, try to write a unit test that fails for the same reason and then modify the code to get that test passing.

##### The Automated Acceptance Test Gate

Automated acceptance tests should be written in the language of the domain/business  problem rather than the technology solution. They are owned by the whole team, as is the rest of the pipeline so don’t be tempted to set up a separate team to manage them.

They can be difficult to maintain if not well designed \[I’m experiencing this now\], in which case look for ways to redesign them so that they are decoupled from the underlying technology.

##### Subsequent Test Stages

Once automated acceptance tests have passed, you are likely to want to deploy the binaries to various production-like environments for manual testing, capacity testing and exploratory testing. Having a CI/CD framework that enables you to do this at the push of a button is very valuable, and should keep a record of which builds are deployed to which environments and from which source code version they are built.

Automated acceptance tests are a resource that frees up the human testers so that they can spend more time on more complex manual tests that add more value to the pipeline.

Nonfunctional tests assume that the core functionality is sound but that certain situations will cause the system to break. These are often down to capacity or security ‘exceptions’ and these exceptional conditions should be tested either automatically or manually or both. A human should decide whether the failure modes of nonfunctional testing are sufficiently severe to rule out a release candidate.

##### Preparing to Release

Have a release plan that is maintained by the whole team, and automate as much of the release process as possible; it should be possible to deploy any release with the push of a button, and rollback to an earlier build also at the push of a button.

Any developer or tester should be able to “pull” any version of the build into their environment and run tests or explore the functionality of the build. Similarly, when deployment is so simple and fast, it can be executed (and therefore tried and tested) many times a day which has the very human impact of reducing stress among the team.

Maintain access to the last known “good” build, both for developers and testers, and for users. Rolling back should be as simple as changing a symbolic link pointing to the “current” working (and not necessarily latest) version. Rolling back data is usually much harder than the executable code, and this is covered later.

By the time a candidate is considered ready for release, it will have passed enough tests for you to be confident that it works as expected. Doing so in stages is “building upon success.”

##### Implementing a Deployment Pipeline

The first step is to model the value stream: draw out a timeline that identifies periods/intervals where value is being added, and where time is being wasted. This is the starting point from which you will design a pipeline that minimises the time not spent adding value.

Next, create a “walking skeleton” where all of the stages are implemented (commit first, deployment second, acceptance testing third so that they can be run using the deployed build) even if they do the very bare minimum (e.g., “Hello World”). Some of these stages might (and should) be triggered manually whereas others should be triggered automatically.

Next, flesh out the build process and then the Deploy Stage (to a non-development environment); don’t worry about testing yet. \[Non-development would mean that none of the tools used to develop the system are installed, as would be the case for a new user’s machine.\] Once deployment has been automated, you should implement a system for deploying a specific build to a specific machine with the push of a button.

Fill out the Commit Stage by adding unit testing and code analysis. Run a few slower tests in parallel with many faster ones so that the testing can complete in a short space of time (5 minutes or less).

Fill out the Acceptance Testing Stage with operations on the installed binaries and also that test connections to third party services.

The pipeline will be a living, evolving system that you add to incrementally over time. Starting with a working end-to-end pipeline \- even if some stages are triggered manually \- enables you to measure progress over time.

The pipeline will give you valuable feedback on how long each process takes, and where the bottlenecks are; look after it as you would a living creature 🙂

##### Metrics

The most valuable metric \[in the opinion of the author and others\] is cycle time: how long does it take to get from a user story to a modified release in the hands of the user. (Imagine the change was to a single line of code, such that the cycle time is entirely spent in build-test-deploy.)

Identify the bottleneck in the process, and maximize throughput through that bottleneck. Everywhere else should be running at *just enough* capacity to keep the bottleneck running at 100%; any more than that would create a backlog which is a waste. (E.g., if developers are completing stories with the bottleneck running at 100% with time to spare, they would be better off using the spare time writing new tests to minimize time spent testing manually.)

Do this while you figure out how to widen the bottleneck, or find other ways around it.

Compute various diagnostics, but be careful about how you present them so that they can be interpreted quickly and easily. They should be produced for every build and archived in a database for posterity (and future reflection).

### Build and Deployment Scripting

##### Build Scripting

Various tools are available for automating building: Make (old faithful; now falling out of favour); Ant (Java-based, uses XML); NAnt/MSBuild (Microsoft versions of Ant for .NET); Maven (Java-based, uses enforced code structure to simplify build tasks); Rake (Make for Ruby); Buildr (good for C/C++); PSake (pronounced, sak-ee like the drink; uses PowerShell scripts).

* Create a script for every stage of the deployment pipeline, and use the same scripts for every environment (including development).  
* Use the OS’s packaging tools rather than platform-dependent ones (e.g., pip for Python).  
* Ensure the deployment process is “idempotent” (i.e., does not change the state in ways it should not).  
* Evolve the deployment system incrementally: start with scripts that deploy to development environment, then to a staging environment for acceptance testing, then to a production environment for final testing and deployment.  
* Certain project structures work better than others and the authors give an example of a Java project that follows the structure demanded by Maven.  
  * Keep source code separate from tests separate from build outputs (binaries) from thirdparty libraries

##### Deployment Scripting

As with building, deployment should involve nothing more than running version-controlled scripts.

Deployment tools such as ControlTier, BMC BladeLogic, CfEngine and Puppet ensure that the environments you deploy to are well-controlled and set up exactly as expected. Use these wherever possible \- they manage deployment as well as infrastructure.

If this isn’t an option, use an agent-based system (e.g., Jenkins) to run scripts on remote machines where needed. This will take care of error reporting, artifact management and provide a handy dashboard for quick visualization.

Failing that, shell scripts and scp/rsync/psexec will do the job. It’s just a lot more hairy.

Start by deploying the OS, then middleware on which your application depends, and then the application. Test each layer in turn before deploying the next layer to identify faults in the correct place.

Test the environment’s configuration before testing the application itself. If the configuration is wrong, you could be left chasing a bug “in your code” for ages before realising the true source of the problem. Check that you have a working network connection, can access the database, etc.

##### Tips and Tricks

* Always use relative paths: if nothing else, it ensures you can install more than one instance of your application on the same machine.  
* Eliminate manual steps: put ***everything*** in an executable script. Ditch bullet point lists of instructions \- that’s what programming is all about\!  
* Build in Traceability from Binaries to Code: add an identifier to the binary so that you can see exactly which commit of the source tree was used to build it.  
* Don’t check in binaries: if nothing else, your binary will always be one commit behind your source so the identifier above won’t match \- dead confusing. These are artifacts and should be handled separately.  
* Test Targets should not Fail the Build: some tests should be run together as a unit, e.g., unit tests and acceptance tests. Let them all run to completion rather than terminating at the first failure. If a test fails, don’t move onto the next unit, but do let all tests within one unit run to completion.

### The Commit Stage

This is where it all starts: a suite of tests that check the code is sound, either before or after compilation, which, if they fail, trigger a repair job by whatever developer committed the change.

It is vital that these initial tests run quickly so that there is rapid feedback to the developers. This feedback should ideally include test results (itemized), test coverage reports, linting, static analysis, cyclomatic complexity, etc. to give an overall view of the code’s health.

The Commit Stage can fail for one of three reasons: compilation failure; test failure; or environmental problems.

Make sure you have enough tests and enough coverage running fast enough to give you valuable feedback in sensible time.

Look after the Commit Stage of the pipeline; don’t let it go to ruin by thinking that other stages or the code itself are “more important”. They aren’t.

The Commit Stage should be owned by developers just as much (or more) than operations.

A Build Master \- knowledgeable in all things Build \- can be useful but should be limited to a short period of time. If possible, rotate the role among the team so that everyone gets to experience it a bit.

Maintain an Artifact Repository to store binary outputs of the build system in a way that agents of the CI system can access them; don’t put binaries in your version control system itself.

##### Principles and Practices

The majority of your code should be unit tested; then the integration (or service) tests to check middleware and performance using real filesystems, databases, networks, etc.; and finally the UI tests which should be very lightweight because the UI should be lightweight and definitely not intimately tied to your business logic.

[https://www.codurance.com/publications/tdd-anti-patterns-chapter-1](https://www.codurance.com/publications/tdd-anti-patterns-chapter-1)

[https://marabesi.com/tdd/tdd-anti-patterns.html](https://marabesi.com/tdd/tdd-anti-patterns.html)

[https://bryanwilhite.github.io/the-funky-knowledge-base/entry/kb2076072213/](https://bryanwilhite.github.io/the-funky-knowledge-base/entry/kb2076072213/)

Avoid the User Interface: testing the UI is difficult full stop. Don’t dedicate much (if any) time to testing it.

Use Dependency Injection: create objects by passing in their components rather than have them create components themselves, so that when testing you can pass in test versions of the components.

Avoid the Database: this is not only slow but stateful such that running the same test twice in quick succession could trip over its own feet. Instead, access the database through an interface in the code that you can Double, and do that.

Avoid Asynchrony in Unit Tests: if an operation involves message-passing, test first the part leading up to the message and then separately test the part after the message pass. Asynchronous behaviour belongs in component tests.

Using Test Doubles: create Stubs or Mocks of components in unit tests to make tests independent of other classes that aren’t under test, and to make them run much faster. Mocks that count function calls and similar statistics are useful for Behaviour-Driven Design.

Minimizing State in Tests: the less you have to SetUp and tearDown (state) between tests, the better. If you’re having to create loads of junk to make the test run, that probably indicates a flaw in the design of your code.

Faking Time: clocks are on a par with databases and file systems. If you need to know the time, do it through a class that can be Mock-ed during testing.

Brute Force: If you really can’t get your tests down to a few minutes, parallelise the tests over several servers to get feedback quicker. It’s worth the cost. 

### Automated Acceptance Testing

There are plenty of advocates for automated acceptance testing, and many critics too.

[https://blog.objectmentor.com/articles/2009/09/29/ruining-your-test-automation-strategy](https://blog.objectmentor.com/articles/2009/09/29/ruining-your-test-automation-strategy)

Manual testing is slow, costly and error prone. Automate acceptance tests wherever possible to ensure rapid development.

Acceptance tests encode the requirements of the system in the form of coded user stories.

Some people think that unit tests and component tests are enough, but these will never give you full confidence in the system as a whole. It should be tested as if a user were using it in a production environment.

There are other types of error (e.g., threading) that are hard to catch with unit tests.

They also encourage collaboration between everyone from the users down to the developers.

Acceptance Tests should be layered so that requirements follow the “Given, When, Then, ...” structure, followed by a test implementation layer that tests the code via an Application Driver Layer that make the API calls. Given that the API (and especially the UI) are likely to change, don’t make the calls directly from the tests themselves.

Avoid testing against the GUI (again), especially unless there is a very clear separation between the minimal UI and the business logic in which case there’s very little argument to test the UI anyway.

##### Analyst and Tester Roles

A Business Analyst should be on the team somewhere to “translate” the user requirements into a language that testers and developers can understand. (Ideally, the BA would be the customer.)

The Tester’s job in this is to explain to the business analyst which criteria would be the most appropriate to test against for each user story or requirement.

Iterative projects tend to cycle around the analyst, developer and tester to inch toward acceptance tests that genuinely assert the desirable behaviour of the system.

Once Upon A Time, requirements or acceptance criteria would be written down in a Word document and then forgotten about, possibly diverging from what the system actually did. Nowadays, people have realised that acceptance criteria can be written as executable tests that are run as part of the deployment pipeline same as any other test. Therefore, if the requirements ever fail to be satisfied, you know straight away because a test fails. And if a test fails, you know the requirements are no longer being met and the system should not be deployed.

The Application Driver Layer is a level of abstraction between the tests and the code under test. It is implemented using business language (e.g., “adminUI.add\_user()” or “adminUI.login()”) rather than the specifics of the API or UI. This decouples the interaction from the code base such that changes to the way we add a user or login can be made in a single place and applied to all tests simultaneously.

This is particularly handy when using a GUI where Window Driver Pattern classes handle interactions with specific GUI elements, and can be substituted for alternative classes if the GUI changes significantly.

##### Implementing Acceptance Tests

Getting the test into the correct state (“Given…”) before testing (“When…”) and then checking the state after the test (“Then…”) can sometimes involve a lot of work. Try your best to minimize the work done, often by creating minimal test data examples that can be created quickly. Do not take dumps of production data.

Avoid creating entry points into your code that exist purely for the purpose of acceptance testing \- they are liable to be exploited if discovered, and suggest that your code is poorly designed.

When testing asynchronous code or anything with a timeout, beware that these can massively increase the time it takes to test the code. If a wait() is required, consider turning  a long wait() into a short one inside a loop that terminates as soon as the desired condition is met.

Doubles aren’t just for unit tests; acceptance tests use them, too, especially for testing communication with external libraries or systems. Make sure you route calls to external objects (file systems, databases, peripherals) through an interface so that you can not only keep changes in a single place but also easily mock error conditions and corner cases.

In order to ensure that these mocks are accurate representations of the external objects they simulate, have some tests that interface with the external objects and check for agreement. Bear in mind that external interfaces are slow and running loads of tests on them several times a day might not be wise (e.g., sending a big prompt to an LLM many times over during testing could get very costly very quickly).

[https://www.amazon.co.uk/Release-Production-Ready-Software-Pragmatic-Programmers/dp/0978739213](https://www.amazon.co.uk/Release-Production-Ready-Software-Pragmatic-Programmers/dp/0978739213)

##### The Acceptance Test Stage

Passing acceptance tests is a necessary but not sufficient condition for deployment. You may choose to deploy a product that has failed some capacity tests or manual tests, but you must never deploy one that has failed ATs.

Create them as early as possible because adding them at the end can be incredibly difficult.

Keeping ATs green is as important as keeping the UTs green, but this is not often enforced because the feedback loop with ATs is much longer so failures can tend to be ignored. ***Do not ignore them;*** fix them immediately. If several commits have occurred between AT failures (only one commit will have happened between UT failures, at least most of the time) then extra effort may be needed to identify the culprit, but it will be worth it.

When running acceptance tests on a freshly created environment (from scripts, of course) it is worth first “smoke testing” the environment to ensure it is set up as you expect before running the acceptance tests. Otherwise, you can end up waiting a long time for tests to complete that were always doomed to fail. (The authors give one example of an asynchronous system where the environment was not set up correctly, leading to timeouts being triggered for every test such that the suite took 30 hours to fail rather than the expected 90 minutes to pass.)

##### Acceptance Test Performance

Don’t worry if your ATs take hours to run, but if the cycle time becomes too long then consider ways to shorten it.

Profile each test and invest some time in speeding up the slowest ones.

Although sharing state between tests is discouraged, it can save a lot of time in your ATs so use it carefully when you can be sure that each test starts from a known initial state.

Distribute tests across processes to run them in parallel, and across a grid of machines in the extreme. If you need a bigger grid than you have on-premises then look into cloud providers such as AWS.

### Testing Nonfunctional Requirements

Now that we’ve checked the answers are correct (the functional requirements) we can check a few other properties of the system (the nonfunctional requirements).

First, some definitions:

* *Performance* is a measure of the time taken to perform one operation, often under tightly controlled conditions.  
* *Throughput* is how many operations can be performed per unit of time and will be limited by the biggest bottleneck in the system.  
* *Capacity* is related to the number of operations that can be performed at once without degrading performance below acceptable levels.

Customers are typically interested in capacity and throughput rather than performance.

Nonfunctional tests are no less important than functional ones; it is no good having a system that gives you one correct answer at a time and takes a year to find it. Dividing resources between the two is a delicate balancing act.

##### Managing Nonfunctional Requirements

NFRs are tricky in that they often interact with each other (more of one means less of another) and with functional requirements (by cutting across many functional layers at once). They also inform the software architecture so need to be considered early on in the design process.

When listing NFRs, they must come with quantitative targets that enable you to estimate the resources required and therefore the tradeoff between requirements. “As fast as possible” is no good.

NFRs should ideally be expressed as stories that can be written up as automated acceptance tests just as functional requirements are.

Performance should not be confused with usability. Having to wait a long time for any kind of feedback is a usability problem, not a performance problem (because the frequency of feedback is a development decision).

##### Programming for Capacity

Planning too far ahead for capacity purposes is fraught with danger: optimize and complexify code only where there is a measurable problem and complexity is the only measurable solution. Don’t write overly complex code in the expectation that capacity will need it.

Avoid threads \- they’re a massive source of complexity and headaches.

Measure the things that matter and don’t try to fix problems you can’t reproduce in an automated test.

##### Measuring Capacity

Measure how the system performs as you:

* Increase the number of users or services  
* Increase the length of time the system runs for (does it degrade with time?)  
* Increase the frequency of requests to measure throughput  
* Increase load to production levels

Ultimately, whether the system meets business needs is paramount, not what capacity you think certain components should be able to manage.

Testing capacity one strand at a time is not advised; most systems will be multitasking and capacity tests should stretch these different strands in parallel.

Capacity measurement gives you a quantitative picture of how well the system performs, and can be plotted for human eyes to inspect. However, it is also necessary to have automated capacity testing that determines whether measured performance is within acceptable bounds and flags it as a Fail if not.

Setting these bounds is tricky: too high and the slightest random flutter (e.g., due to other network or disk activity) can cause it to fail; too low and a real performance reduction can slip through the net. In a worse case scenario, a threshold of 50% will not be triggered if a commit reduces it from 100% to 51% but a subsequent commit (perhaps many months later) that reduces it from 51% to 49% will trigger a bug hunt (for the wrong bug).

The more you can measure and predict loads (and how they vary over time) the better.

Tests should model specific scenarios with specific acceptance thresholds as closely as possible to be as useful as possible.

##### The Capacity-Testing Environment

The environment you use for testing should be as close as is practical to the production environment on which the system will be deployed. A replica of the production environment is not practical when the prod env is very big (e.g., a cloud cluster serving millions of users) or when the system is very small such that it isn’t worth the effort of replicating production.

Remember that bottlenecks do not scale linearly; replacing a CPU with one twice as fast might speed up the system by a factor of two, or it might simply replace the CPU-bound bottleneck with an IO-bound one. Do not make too many assumptions about where the bottlenecks are in a scaled version of the test environment. Having a number of scaling runs gives extra information on where the bottlenecks lie.

##### Automating Capacity Testing

### Deploying and Releasing Applications

## The Delivery Ecosystem

### Managing Infrastructure and Environments

### Managing Data

Mostly concerned with database management since this is a common source of data.

In short, use scripts to migrate databases from one version to another, and store the current version of the database in a table of the database. Then, when you run your app you can check whether the database version is correct and either rollback or rollforward until the desired version (schema, etc.) is reached.

This can be complicated when scripts make irreversible changes, so bear this in mind.

When testing code, ideally every test should be independent of all other tests. Sharing data between tests, while it can be efficient, is strongly discouraged because it leads to coupling between tests and errors that are very hard to find and fix.

If you need to share data between tests for efficiency reasons, that’s a warning sign that your code/tests aren’t “atomic” enough and should be broken up such that they can be tested with data that is quick to generate in memory.

### Managing Components and Dependencies

### Advanced Version Control

### Managing Continuous Delivery

