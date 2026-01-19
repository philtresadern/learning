# The Mythical Man Month (Brooks, 1975/95)

### The Tar Pit

* Software development can be like getting stuck in tar, impossible to break free.  
* We often hear stories of the garage-based developer duo who produce some wonder software.  
* What is often produced in a garage is a program that runs on one system and does one job.  
* In order to turn it into something truly useful, it must become either:  
  * A programming product \- complete with testing, documentation, maintenance, etc \- that does more than one thing. This takes \~3x the time.  
  * A programming system \- complete with interfaces and system integration \- that aims to form part of a larger whole. This also takes \~3x the time.  
  * A programming system product, which is a combination of the above and takes \~9x the time of the program.  
* There are many joys in programming, but also many pitfalls.  
* Programming often needs to be perfect (think of the satellite that flew into the sun because of a missing bracket) and we’re not accustomed to being perfect.  
* We must also accept the possibility of our idea or product becoming obsolete before it is “finished” though this is often a bigger worry than it ought to be.

### The Mythical Man Month

* One of the most dangerous myths is that men and months are interchangeable and that a problem can be solved twice as fast with twice the men \- wrong\!  
* Men and months are indeed proportional to *cost* but not to *progress*.  
* It all depends on how much the problem can be partitioned (i.e. network theory and sequential vs. parallel tasks).  
* For a perfectly partitionable problem *with no communication between men* (e.g. harvesting wheat in a field) there is an inversely proportional relationship (more men or months \= less time).  
* For a non-partitionable problem (e.g. incubating a baby), no addition of personnel will make it take less than 9 months.  
* Most problems fall in between, but then the relationship between men, months and progress depends on the level of communication required.  
* We are all optimists at heart, and programmers especially so; this leads us to underestimate the time taken to do jobs, and to almost ignore some jobs altogether.  
* The author recommends assigning time in the following proportions:  
  * ⅓ planning  
  * ⅙ coding  
  * ¼ component/unit testing  
  * ¼ system testing  
* The ½ set aside for system testing is especially important because this is often neglected until close to the end of the project where costs due to inadequate testing are highest.  
* ***Adding more men to a project that is late only makes it later.***  
* One reason is that it adds more jobs that were not originally conceived such as:  
  * Training the new recruits, using up valuable time of both the inexperienced recruits and the experienced trainer.  
  * Rescheduling and repartitioning: the network diagram must now be redrawn for a bigger team which takes additional time.  
  * If these additional tasks aren’t factored in at the point of review, the plan will be askew all over again.  
* Another is that interactions between men increase *combinatorially* with the number of men, greatly eating into the ‘communication budget’.

### The Surgical Team

* Ideally, a product would be built by a small team (10 members or less) of experts. However, this is a) hard to find and b) does not scale well for very large products.  
* Instead, Mills suggests a team somewhat like that of one doing surgery, with a single all-powerful surgeon who basically implements everything, supported by a crack team:  
  * Surgeon: designs, implements, tests and documents the code.  
  * Copilot: shadows the Surgeon to take over in an emergency but also to bounce ideas off each other and to communicate with the rest of the team. *The Co-Pilot is subordinate to the Surgeon*.  
  * Administrator: handles money and personnel (one Admin can support two teams)  
  * Editor: Takes documentation written by the Surgeon and improves it  
  * Secretary (x2): one each for the Administrator and Editor to handle correspondence  
  * Program Clerk: responsible for documents (including inputs and outputs) across the project, keeping them filed, recorded and accessible. It is vital that these documents are treated as public and owned by the team rather than private and owned by an individual (e.g. the Surgeon).  
  * Toolsmith: creates helpful widgets for the purpose of speeding things along (i.e. automating)  
  * Tester: friendly adversary to the Surgeon, runs tests and also creates new test cases (with data) to be added to the test suite.  
  * Language Lawyer: an expert in the language who can find ways to exploit all of its potential.  
* Effectively, the team works as one mind with many hands. The Surgeon communicates mostly with the Co-Pilot who, in turn, communicates with the Program Clerk, Toolsmith, Tester and Language Lawyer. The Surgeon also communicates \- to a lesser degree \- with the Administrator and Editor, both of whom communicate with their respective Secretary.  
* On a large scale project, a System Architect oversees a population of Surgeons and restricts themself to strictly architecture matters only (not implementation).

### Aristocracy, Democracy and System Design

* In the author’s opinion, *conceptual integrity is essential to success*.  
* As a result, he makes no apology for suggesting that a small team of architects are responsible for all decisions about *what* the system should do while a larger team of implementers define *how* the system does it (because there is more than one way to skin a cat).  
* The arguments against include:  
  * The specifications will be too rich in function: this is a genuine concern and will be addressed shortly  
  * The architects will have all the creative fun: this is nonsense, and in many cases the imposition of constraints leads to far more creative implementations.  
  * The team of implementers will be idle while the architects thrash out the design: this is also false because a) you can delay recruiting an implementation team just as you would in building construction and b) there are lots of jobs the implementation team can work on with just the bare bones of the design at hand.  
* One more argument for an aristocracy rather than democracy in architecture is that a large committee can spend an *awful* lot of time debating design points \- time that could be much better spent elsewhere, albeit at a (usually small) cost in the end product (though it also comes with the benefit of greater conceptual integrity).  
* As an aside, the author notes that simplicity is not the same as straightforwardness and the latter should take precedence.

### The Second-System Effect

* The first system that an architect designs will be sparse, clean and efficient. The unnecessary and frivolous features will be put to the side “for another day.”  
* That other day usually comes in the second system designed by the architect, which will probably be bloated and inefficient.  
* Subsequent systems will learn from this mistake suggesting that, when hiring an architect, you don’t go for one who has designed one and only one system before.  
* The other danger inherent in the Second System is that these frivolous and fancy features are often obsolete by the time you have implemented them, and the effort would have been better off invested in adding features that will be useful in the Third system rather than ones that would have been useful in the First.  
* A clean distinction between the architect’s responsibility and that of the implementer requires open and frequent communication between the two.  
* The architect must also be willing to give suggestions on implementation, privately and diplomatically, while allowing the implementer to take the credit. (And vice versa.)

### Passing The Word

* One good way to ensure conceptual integrity across a large team is to have a written user manual before the implementation begins. This sets out what is allowed (and what is not) and what behaviour is expected. The implementation is up to the implementers (though the architect may give an *example* of a *possible* implementation).  
* The manual should be written by as few people as possible in order to maintain consistent style and substance. It must be precise (even if this means boring).  
* Formal definitions can be used, and there are languages to help in this. An existing implementation can be used as the formal definition because the “correct” output for a given input is simply what the existing implementation says it should be.  
* Meetings of top level managers and architects should take place regularly (e.g. weekly) where people can raise problems and brainstorm solutions; decisions should come later when there has been time to mull over. Minutes should be kept and distributed.  
* Less frequent meetings can be used to thrash out any backlog in issues or features. The author suggests every six months a larger meeting where decisions are made quickly and the manual updated immediately in time for the next day’s meeting.  
* Consider multiple implementations so that when \- and it will be when, not if \- the manual and implementation get out of sync, the errant implementation is the thing that gets changed rather than the manual (because there will be resistance to change the existing compliant implementation \- why should they?).  
* Questions can be raised with the chief architect by phone, and a log kept of every question and its answer. This log should be circulated weekly.  
* The testing team audits the system to keep it honest. Rather they find the big bugs than the customer.

### Why Did The Tower of Babel Fall?

* In short: poor communication and poor organization.  
* Communication should be frequent, thorough, recorded and accessible to as many people who need it as possible. (The internet, which this book preceded, makes this easier.)  
* One solution to this is the Project Workbook that maintains the structure of all documents in the project, but also grows with nonlinear complexity as the number of “men” increases. Nowadays, this would be maintained on a website or Git repository but the chief problem is making sure everyone who needs to reads the change logs (all of them).  
* Regarding organization, the correct team is essential. More often than not there will be a producer and a technical director heading the team:  
  * They can be of equal standing, though this inevitably leads to stalemate at times (unless they are the same man, which works only on small projects and people who can wear both hats are extremely rare).  
  * The producer can outrank the technical director, though this can lead to the producer overruling a technical genius which is not desirable. Good producers who can get the most out of a subordinate technical guru are also very rare.  
  * The technical director can outrank the producer, such that the producer’s role becomes to grease the technical wheels as much as possible. This is the author’s recommended structure.

### Calling The Shot

* Estimating the time it will take to complete a programming job is hard, and estimates are often out by a factor of two or more.  
* \[This is a short chapter that draws on old and old-fashioned data, so I skimmed over it.\]

### Ten Pounds in a Five Pound Sack

* \[This also harks back to days when space considerations were alway important. Less important nowadays so I’ll skip this, too.\]

### The Documentary Hypothesis

* Every serious project manager needs documents that answer the “journalist’s questions”:  
  * What are the objectives?  
  * What are the product specifications? (Starts as a proposal and ends as the manual and internal documentation. This is the first document to be started and the last document to be finished.)  
  * When do things happen? (i.e., the schedule)  
  * How much does it cost? (i.e. a budget)  
  * Where do things go? (i.e. space allocation)  
  * Who has what responsibility? (i.e., an organizational chart which, by Conway’s Law, will mimic the communication model within the system.)  
* Written formal documents that are accessible to the team help enormously in communicating decisions as they are taken during the course of the project, and also for keeping the project from straying off course.  
* The manager’s tasks will mostly then be communication, because most other questions will have been answered in one of those documents.  
* Important decisions usually centre around material changes to one of those documents.

### Plan to Throw One Away

* Plan to throw away your first attempt \- that is what will happen anyway, whether you plan for it or not.  
* Otherwise, the decision you have implicitly made is that you will deliver to the customer the product that should be thrown away,which will bit you on the bum.  
* Design the system to be open to change, and also your organization. People should be free to move between managerial and technical roles, though the rewards for going from manager to technical director should be higher than the other way.  
* Software maintenance is generally bad for the system. Over time the number of new bugs introduced outnumbers the old bugs fixed such that the program is no longer a vehicle for progress and needs to be redesigned from the ground up.

### Sharp Tools

* Appreciate the value of customized tools, but make this the responsibility of one person (the Toolsmith) rather than allowing a free-for-all.  
* Code and documentation should start in a sandpit, then be promoted to the repository where it cannot be changed without the permission of the repo manager. Later, it can be promoted again to deployed code.  
* \[There follows much on machines and their different purposes, though much of this is of its time and now out of date.\]  
* Simulators are useful when the new hardware is still under development, not only because you can start your software development early but also they remain consistent through hardware changes.

### The Whole and the Parts

* Anyone can write software, but only some people can write software that works. How do we avoid the common pitfalls and buggy output.  
* Consider the design carefully and identify any unspoken assumptions. Hand the design document to an outside team who can read it without being too close and mentally filling in the gaps. Employ Top Down Design, slowly decomposing the problem into subproblems into subsubproblems and so on, suppressing detail until it is truly necessary.  
* Component testing and system testing should be thorough, incremental and scrupulously planned. Expect to build lots of scaffolding (which will not form part of the final product) in order to set up the tests you need. Don’t try to cut corners or assume you can get away without it; you can’t.

### Hatching a Catastrophe

* Disaster is usually caused by termites, not tornadoes, and a project becomes a year late one day at a time.  
* Have concrete milestones that are crystal clear so that there can be no ambiguity about whether they have been reached. Nobody likes to give bad news, and fuzzy deadlines will be massaged until the consequences are inescapable.   
* Schedule slippage is a morale sapper.  
* PERT charts and critical path analysis are essential at the beginning of the project to identify which stages must happen on time if the project is to complete on time. It also identifies which legs are about to slip onto the critical path as they get delayed.  
* Don’t sweep anything under the rug. Bosses must establish a clear distinction between *status-update* meetings (that will get no action from them) and *problem-action* meetings (which will require their action). Not acting on a status update gives lower level managers the confidence to report back honestly, knowing that their authority will be respected by their superior.  
* Employ *hustle*: the practice of trying to do everything quicker than you think it needs to be done, trying to be early for *everything* so that when things slip (and they always slip), you have created some margin for error.  
* Keep an updated PERT document that contains frequent milestones with both scheduled completion dates (the responsibility of senior management) and estimated dates (the responsibility of the lowest management that is closest to the coalface).  
* Invest a small amount of talent in a Plans and Controls Team whose responsibility is getting sensible (not fanciful or deliberately pessimistic) estimates of completion dates from low level management. The PCT’s responsibility is purely to keep the PERT documents up to date and accurate and pass this on to upper level management to decide on necessary actions.

### The Other Face

* Software is communication from man to machine. But it has another face: communicating to other humans, especially those who use it, those who rely on it and those who must maintain it.  
* Documentation is therefore essential, even though nobody really likes doing it.  
* At the very least, there should be a document that specifies:  
  * The purpose  
  * The environment (what hardware it will run on)  
  * Domain and range (what can go in and what could come out)  
  * Functions realised and algorithms used  
  * Input-Output formats  
  * Operating instructions  
  * Running time  
  * Accuracy and checking  
* Don’t bother with flow charts \- they’re usually a waste of time. The only one worth having is such a high-level that it really shows the organisation of the software, for which there are better schematics than a flow chart.  
* Documentation is best written in the source code itself, closest to the person using it. \[Modern tools will extract it and turn it into pretty pages.\]

### No Silver Bullet \- Essence and Accident in Software Engineering

* Writing good software is inherently hard and a software project can quickly turn into a monster at the full moon. Sadly, unlike with werewolves there are no silver bullets for monstrous software projects.  
* Productivity gains in hardware are incredible: doubling of speed and capacity every two years for such a long period of time are unheard of elsewhere. No wonder software engineering has struggled to keep up\!  
* On the essence of software engineering, the author looks at four topics:  
  * Complexity: Software is inherently complex \- complexity is the essence of software because scaling up means duplicating, yet it is not simply a case of carbon-copying as in other industries; complexity increases nonlinearly; and complexity increases the management challenges.  
  * Conformity: Software engineers must play by the rules, making sure their interface conforms with others in existence. But those existing components are not created by God or nature (as in physics and maths) but by other people. And they can therefore be changed at any time, putting your endeavours at risk.  
  * Changeability: Pressures to change (update) software are much greater than other manufactured goods, partly because functions change (as opposed to methods), hardware changes rapidly, and new users come up with new use cases that were not thought of by the software engineers.  
  * Invisibility: Most products and ideas can be visualised with a diagram. Not so with software: there are too many dimensions and facets to software that render it almost unvisualisable and any attempt to draw a graph of software usually comes up against cyclic, non-hierarchical graphs that are too complex to comprehend.  
* The more accidental barriers in software engineering tend to be related to how we go about writing and running software, and so can be broken down (unlike the essence of software engineering) with new tools and techniques. Examples include:  
  * High level languages: Enabling concepts to be expressed in a language closer to that of the human programmer has increased productivity enormously.  
  * Time-sharing: Reducing the latency in getting results has enabled programmers to maintain their focus on what really matters, rather than being distracted by the many events that can occur in a long compile-execute cycle.  
  * Programming environments: These enabled the many aspects and tools of software engineering to be stitched together with ease. These tools were always capable of being used together, but in practice it was very difficult until environments came along.  
* 