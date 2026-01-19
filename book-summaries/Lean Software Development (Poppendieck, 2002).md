# Lean Software Development (Poppendieck, 2002\)

22 tools for developing software in an Agile and Lean way.

## Eliminate Waste

### Seeing Waste

Waste is anything that doesn’t add value to the customer or the business: partially done work (waste of code space); extra processes (waste of time and effort); extra features (waste of maintenance effort); task-switching (waste of attention); waiting (waste of time); motion (waste of energy); defects (waste of good-will).

* Code that never makes it into production is waste.  
* Paperwork and meetings that nobody needs is waste; use templates to reduce mental workloads; is anybody really waiting on that piece of paperwork? Delay documentation until the iteration in which they are implemented.  
* If code is not needed *now* then it is a waste.  
* Attack problems one at a time; don’t try to multitask.  
* Minimize delays  
* Keep things together to minimize movement; put developers, testers and analysts in the same room, for example \- no point having to walk miles to have a conversation. (And being in close proximity increases the frequency of small conversations.)  
* Detect defects as early as possible with automated testing.  
* Management activities sometimes add value; often they just add waste. Set up systems so that management burden is minimal.

### Value Stream Mapping

Draw out a timeline of your product development from start to finish, showing time spent adding value vs. not adding value (e.g., waiting for something such as approval). Then rework the workflow to spent as much time adding value and as little time not.

## Amplify Learning

Product development is as much about learning as it is about developing. Learn as much as possible as quickly as possible. Standardization can be the enemy of learning; use it sparingly.

Planning everything up-front assumes that everything is known at the start and nothing needs to be learned. Then, when conditions change (as they always do) there is little opportunity to learn from and capitalize on the new conditions.

### Feedback

Get as much feedback into your process as possible (e.g., have automated tests that run quickly on every push and report results back). The more feedback loops you have in your system, the more opportunities for learning you’ll spot.

“Increasing feedback, rather than decreasing it, is the single most effective way to deal with troubled software development.”

Have many, short feedback loops rather than fewer, longer ones.

### Iterations

Every iteration of the development cycle should add some value, often one small piece at a time.

Start with a planning session where tasks and features are prioritized; high-risk tasks should get high priority; set a time-scale, the shorter the better; short enough to get feedback but long enough to build in quality; don’t overcommit \- some features on time is better than all features late.

Teams must be committed to the workload for the current iteration, and management must offer the correct support.

One risk is that the software will not reach *convergence*; too-frequent feedback can lead to *thrashing* where the team changes direction too often. Find the right balance to ensure you keep moving in the right direction.

Ask the customer for their N highest-priority features and deliver them quickly. Then go back and ask for the next N features, and so on. Either state up-front that scope is negotiable, or define only the high-level scope and negotiate on the details.

Make progress visible to the team by producing *burn-down charts* that show not only the amount of work still to complete but also the speed at which work is being completed. This will give an early indication of whether the scope needs to be adjusted to meet the next deadline.

### Synchronization

Keeping developers in synchronization involves having version control, checking for conflicts when pulling code, running tests locally before committing, and having automated tests that run at least daily (preferably more frequently).

A *spanning application* is an end-to-end version of the system with dummy components in place that are gradually replaced. This ensures the full system can be put under test from the very early stages of development and regression tested throughout.

Developing interfaces at the very beginning can be useful in partitioning the work to be done. After the interfaces are designed jointly, the implementations can be written almost independently.

### Set-Based Development

Point-based development involves making decisions early, which can lead to problems down the line.

Set-based development identifies only the current constraints and therefore the region in which you can work. As time goes on, more constraints will come it and tighten the working region until it heads closer and closer to a point.

It is a way of deferring decisions until the last responsible moment. Keep as many options on the table as it is practical to do \- discard only those that are definitely bad.

Don’t be afraid to try out multiple solutions and then integrate the best of them.

Communicate constraints, not solutions. Say what you aren’t going to do without limiting what you are going to do. Solutions often emerge over time that would not have been dreamed up in the beginning.

## Decide as Late as Possible

Concurrent development is effectively breath-first development, allowing many branches to evolve simultaneously and you to discover costly problems early on rather than after lots of work has been done (and needs to be undone).

It is also the best way to deal with changing requirements and avoid cost escalation.

Some decisions are fixed early (e.g., what language to develop in) and can’t be changed. Many other decisions can be left largely open until more information presents itself.

### Options Thinking

The financial industry uses Options as a means to incentivise workers and investors. You have the option to purchase shares at a given price on a given day in the future. If, on the day, the option looks like a profitable one you can exercise it; if not, you can pass on it.

This process enables delayed decision making while you wait for more information to become available, and the same approach can be taken in software development. Having adaptive processes rather than predictive processes enables this kind of thinking.

### The Last Responsible Moment

The Last Responsible Moment is the point at which not making a decision eliminates an important alternative. Delay making decisions until the LRM, and no later.

Share partially complete design information; organize for direct, worker-to-worker collaboration; develop a sense of how to absorb changes (e.g., use coding practices that minimize coupling and maximize flexibility); avoid repetition (DRY); separate concerns; encapsulate variation; defer implementation of future capabilities; avoid extra features.

Develop a sense of what is critically important in the domain; develop a sense of when decisions must be made; develop a quick-response capability.

### Making Decisions

Having domain knowledge will help you to spot where changes are most likely to occur and build in breadth-first thinking.

People with lots of domain knowledge and experience will make decisions intuitively, calling on their expertise without much thought. People are often much more important than processes.

If you are going to have rules, keep them simple such that people can be trusted to follow them without having to be supervised or organized. The seven chapters in this book constitute simple rules to follow.

## Deliver as Fast as Possible

This does not mean rushing and making a mess; haste makes waste (and waste should be eliminated).

The purpose of rapid delivery is to enable short feedback cycles and amplify learning. Customers also like having new features delivered regularly rather than having to wait (because waiting is waste).

The faster you can deliver, the longer you can delay decisions (which is good).

### Pull Systems

Rather than a manager *pushing* work to workers, set up a system where the workers can *pull* work for themselves, allowing them to choose the tasks they find most interesting or have the most experience.

Just-in-time development minimizes “inventory” (partially developed work) by developing only the features the customer has asked for, as quickly as sensible.

One popular software equivalent follows a *kanban* (“sign” or “placard” in Japanese) board where “cards” (tasks) are stacked in the “to do” column, placed in the “checked out” column when a worker takes responsibility for them, and placed on the “tests passed” (“complete”) column when the work is provably finished.

Regular meetings (daily, ideally) help the flow of work onto and off the kanban board by discussing what has been done and whether issues have arisen that justify another specific meeting.

To keep iterations short, the tasks should be kept small.

An *Information Radiator* is a highly visible display that shows progress. It is crucial that it is prominently displayed so that everyone can benefit from its “warmth.”

### Queueing Theory

Use queueing theory to ensure a smooth flow of work through the system and eliminate bottlenecks (where delays and waiting create waste).

Ensure a steady rate of arrival by splitting work into smaller parcels that can be received more frequently and regularly. Minimize variability in the amount of time required to complete a task such that they flow more regularly through the system.

Make sure you have *slack* in the pipeline because progress slows nonlinearly as you approach 100% of your capacity. The closer you get, the exponentially slower you go, so stay at a sensible level of resource utilization. This also gives you time to think, change and grow.

### Cost of Delay

Work with an accountant to compute the approximate costs of adding features and delaying features. This not only gives you a useful tool when negotiating with management but also helps to prioritize features during planning meetings.

The cost of delay in an emerging market can be great when the first-mover advantage is big. Being second or third competitor can suppress market share, and growing market share can take a long time during which you’re missing out on revenue every single year.

Turning decisions into dollar amounts enables easier comparison when making trade-offs because everything is then in the same units.

## Empower the Team

Managing people is not as powerful as enabling and allowing them to manage themselves.

### Self-Determination

Allowing workers to take control and ownership of their work increases morale, productivity and host of other positive metrics. Most improvement programs, however, teach managers how to control the workflow and teach workers how to do their jobs. 

GE’s Work-Out was an exception in that it showed workers how to teach managers to let them get on with their work. Teams would propose ways to get rid of processes that were hindering them and do their work more efficiently. Managers would make decisions on these proposals in at most two days (ideally on the spot) and ways to make work more productive were put in place by the people who proposed them.

Treat workers like volunteers to get the best out of them.

### Motivation

3M is the model for keeping workers motivated, giving them freedom to come up with new ideas for products, and the means to turn those ideas into output for the company.

Giving workers a purpose (and maintaining it) is key to them being motivated. 

1. Start with a clear and compelling purpose  
2. Be sure the purpose is achievable  
3. Give the team \[direct\] access to customers  
4. Let the team make its own commitments  
5. Management’s role is to run interference: managers can serve as the barrier between a productive team and outside influences who would interrupt the team’s flow.  
6. Keep skeptics away from the team; they don’t need to hear naysayers.

Motivation has many building blocks: 

* Belonging: ensure that the team “gels”; don’t single out members for team achievements.  
* Safety: allow individual mistakes to happen \- they are learning opportunities and are likely to be less damaging than a sanitised “zero defects” environment.  
* Competence: team members need to feel confident that they will achieve the goal, which requires competence on their part and on the part of managers, systems and processes. Everything should be “fit for purpose”.  
* Progress: team members need to feel that they are making progress. Feedback from customers, managers and information radiators can contribute, as can rewards for reaching milestones.


Don’t think that long days and late nights are a good thing; they can’t continue indefinitely, often foster a culture that can quickly become toxic, and leave little slack for dealing with emergencies.

### Leadership

The old “management vs. leadership” debate again 🙂

Managers cope with complexity; they plan, organize, monitor and control. Leaders cope with change, they set direction, align people and enable motivation. 

\[In my mind, management is tactical/operational; Leadership is strategic.\]

Leaders often emerge rather than being appointed, and can be exceptional designers in their field. People feel compelled to listen internally (rather than externally with managers). Leaders are knowledgeable, wise and have a solid grasp of the domain. Master developers know when enough design has been done to start implementing.

There is little room for project *management*, but plenty of room for project *leadership*.

* *Software Craftsmanship, Breen*

### Expertise

Allow communities of expertise to flourish; they will serve you well.

Nucor (a common case study) rewarded staff based on the performance at the level above their pay-grade in order to promote team-building.

Standards are required for consistent communication and styles. Allow communities of expertise to devise the standards that will enable them to work best.

## Build Integrity In

The key to integrity is an excellent, detailed flow of information between customers and developers.

### Perceived Integrity

Perceived Integrity \= when it “feels right” to the customer (a front end view). The customer should feel at home with the product and feel as though it was designed just for them.

Starting with the application domain is a good way to ensure that the view from the customer’s side makes sense.

There are several models for ensuring good flow of information:

* Conceptual domain model: class model of entities/actors in the system, expressed in the language of the domain.  
* Glossary: define all terms used in the domain model to ensure a consistent language. *Everything should be expressed in domain terms*.  
* Use Case model: describe typical scenarios, often in some semi-structured form (see SWEBOK)  
* Qualifiers: understand which parameters might be different between the development environment and production environment, and the impact these differences might have on the integrity.


Some models may be more useful or suitable than others. Feel free to initiate several of them and discontinue the ones that are rarely used (because that’s often an indication that they are not useful).

An automated test suite is a better way to maintain institutional knowledge about the system than documentation that quickly becomes obsolete.

References

* *Domain Driven Design, Evans*

### Conceptual Integrity

Conceptual Integrity \= when it “feels right” to the developer (a back end view). Well structured code with a decent architecture will be satisfying and intuitive to any developer who joins the team.

Don’t get all your requirements at the beginning and then leave them. Keep iterating as you release more product to ensure the requirements you have are fit for purpose.

A layered architecture is often a good start to ensuring solid conceptual integrity: presentation; domain and data layers at the very least (perhaps with services and translation layers in between).

It should be possible to test layers independently of each other; there should be strong cohesion within layers and loose coupling between them; the Presentation layer is often hard to change because once a user has created habits, it’s very hard to change them.

Ensure integrity emerges by:

* Using existing parts as much as possible, including exporting data in a flexible format (e.g., CSV) to allow the user to use existing parts of their choice.  
* Applying integrated problem solving that involves regular communication with and feedback from customers, problem-solving before all information is available, and using customer tests from as early as possible.

References

* *Software for Use, Constantine and Lockwood.*  
* *Patterns of Enterprise Application Architecture, Fowler*  
* *Agile Software Development, Martin*

### Refactoring

Good design is rarely thought up at the very beginning of a project; often it emerges as the project progresses. This means that there will be work to do in improving the design as the system develops if we are to avoid structures becoming calcified and fragile.

*Refactoring is not rework (i.e., not waste).*

Maintaining conceptual integrity requires simplicity, clarity, suitability for use, no repetition and no extra (redundant) features. (The last of these is difficult to deal with once something has made its way into the software; someone somewhere will depend upon it and howl if you take it away.)  
If a problem is discovered, *stop developing and start fixing immediately*.

Without refactoring, it becomes more and more difficult to add value-adding features that customers will appreciate.

### Testing

The line between unit testing and integration testing is a little blurry; just call these “developer tests.”

Conversely, “acceptance” tests can be referred to as “customer” tests since they exercise the conceptual integrity of the system.

Tests are essential “documentation” of the system since they spell out exactly the expected behaviour of the system. \[Bob Martin argues that tests are at least as important as the code; if you lose the code but retain the tests, you can recreate the code by getting it to pass one test at a time. If you lose the tests but retain the code, it’s really hard to write new tests that exercise every line of the code.\]

Tests are often released along with the code to indicate what is an acceptable product. Customer tests can often serve as a substitute for requirements. (Software satisfies the requirements if it passes the customer tests.)

Tests provide immediate feedback to the developer (and customer) that the software is working as intended. This feedback is a crucial part of iterative development.

Automate tests as much as possible so that they run at every push of the code.

Tests serve as scaffolding, allowing developers to make changes in the knowledge that any unintended consequences will be detected before they can cause havoc.

Tests serve as “As-Built” documentation \- forced to be kept up-to-date because the tests will fail if they aren’t.

Tests are especially necessary for maintenance, ensuring that refactoring changes, for example, genuinely do not change behaviour.

## See the Whole

Keep asking why to get through the many layers at which a problem is apparent; it won’t be just one.

### Measurements

Don’t be tempted to break a problem down into tiny pieces and compute metrics for every bit. Chances are you’ll miss the bigger picture and suboptimize. Often the act of measuring engenders behaviours that are counterproductive.

You shouldn’t try to win a Decathlon by coming first in every event \- it won’t happen. Concentrate on being first overall. The way to measure everything that counts is by aggregation, not disaggregation.

Look out for decisions and measurements based on superstition and habit (“we’ve always done it that way”).

Don’t try to measure the performance of knowledge workers; it positively courts dysfunction.

Attribute failures to the team, systems and processes as a whole. It won’t be the fault of one individual.

### Contracts

Contracts exist to fill a void in trust between a producer and a consumer. This arises out of uncertainty about the success of a project that grows over time. Using lean development processes with regular feedback closes the loop so that uncertainty is reduced at regular intervals and does not grow without bound.

A rigid specification begs for changes at a later date, for which the producer can and will charge the consumer heavily. A more flexible approach to software development can create a more equitable distribution of risk.

Bringing work in-house is better than outsourcing, but is often not feasible.

Contracts come in different flavours:

* Fixed-price: transfers risk to the supplier: If the project overruns, the supplier bears the cost; if it underruns, the supplier makes a profit. If changes are requested by the consumer that were not included, the supplier will charge heavily for it, and this creates an incentive for suppliers to make the lowest bid (to get the contract) then squeeze the consumer for every little change that wasn’t written into the contract. Often, you’ll get what you asked for on-time and on-budget, but it isn’t what you wanted.  
* Time-and-Materials (per hour/day): transfers the risk to the consumer, since overruns incur more payments to the supplier. However, the supplier then has an incentive to drag their feet and get paid more, and the customer has to spend more time (waste) ensuring that the costs don’t spiral. Once deployed, the customer becomes dependent on the supplier (because they owe them money even if the finished product is not delivered) which puts the ball in the supplier’s court.  
* Multistage: sometimes, a supplier will be paid a small amount (one contract) to produce the outline/plan/spec and then paid on a second contract to deliver it. This can control costs to a degree (allowing an early exit at one point) but can also make a commitment to a particular supplier who is then expected even more to “get it right.” An alternative approach does payment-on-milestones such that payments are staged for meeting certain targets as the project progresses. This kind of contract can also build trust incrementally over the project.  
* Target-Cost: In this set-up overspends are borne by both sides, giving both an incentive to ensure delivery is on-time and on-cost. This can be done by agreeing to work at-cost (no-profit) once the original budget has been spent, or by agreeing a cost without profit plus a bonus if the targets are met. These kind of contracts promote working together and collaboration to ensure the project’s success.  
* Target-Schedule: In this case, the cost and schedule are fixed and a minimal solution is delivered well before the deadline (on-schedule by definition). This can then be refined by adding highest priority features until the money or time runs out. What had then been delivered is a working system with as many features as time and money permitted.  
* Shared-Benefit: In this set-up, both parties take responsibility for doing the work. Sometimes this can take the form of the contractor doing much of the work while also training the purchaser’s staff so that they are in a better position to do the work internally.

All of these contracts apart from Fixed Price try to avoid fixing scope in detail up-front (which is an objective of lean development).

Often, the main question is “what *aren’t* we going to do?” This cuts out unnecessary work and costs.

The main objective is to produce something that fosters a collaborative and trusting relationship between producer and consumer.

\[One other option I thought of is a hybrid between Fixed Price and Time-and-Materials: a fixed cost plus an hourly/daily rate that is lower than the typical rate. These can be fixed such that if the expected completion time is correct, the price works out the same as it would in a Fixed Price or Time-and-Materials contract, but if the project overruns then the added loss to the supplier is less than in a FP contract but the added cost to the consumer is less than in a TaM contract.\]  
