# Extreme Programming Explained (Beck, 2005\)

Extreme Programming (XP) is a social concept \- values, principles and practices to help teams deliver more value to the customer quicker, and feel happier about it.

It can be distilled into Values, Principles and Practices.

Values have nothing to do with Software Engineering or the project at hand. They are simply what you feel are important in any endeavour.

*Practices* are the things you do \[Tactics, in my mind\] to enshrine the Values you espouse.

*Principles* are the glue in between \[strategic, if you like\] that are related to Software Engineering but not related to concrete actions. 

*Values* are why you follow certain Principles; Principles are why you follow certain Practices.

Practices are how you follow certain Principles; Principles are how you embody certain Values.

## Values

##### Communication

The right amount of communication, using the correct channels, is essential for effective teamwork. Valuing communication means using the most effective means an appropriate amount of time (i.e., valuing/respecting other people's time and attention).

##### Feedback

Change is inevitable, and feedback is how you monitor whether change is necessary and in which direction. The more often you can get feedback, the better. You won't "get it right the first time" because the first time will quickly be in the past and out-of-date.

##### Simplicity

The simplest solution is kinder to yourself and your colleagues (both current and future). Valuing the simplest solution avoids wasteful complexity.

##### Courage

Have the courage to face up to fear of conflict, to act when there is urgency, and to be patient when there is not.

##### Respect

Care about your team members and value their opinions and ideas. Everybody in the team has something of value to contribute.

## Principles

##### Humanity

Everyone has (or should have) a life outside of work, and their needs beyond the job should be respected. Work fulfills some needs, but non-work fulfills the rest. Try to keep an appropriate separation between work and "life".

##### Economics

Everything you do in work should have an economic benefit to the company (whether by increasing income or reducing wasteful expenditure). Don't tinker around doing nothing work. Use tools such as accounting to make the case for subprojects or to prioritise between competing tasks.

##### Mutual Benefit

Look for win-win(-win) tasks that benefit you and your colleagues and your users/customers, both now and in the future. Extensive documentation is an example of a task that is not mutually beneficial \- you probably won't benefit from your own documentation in the future, and it's a cost to you now. Writing better code with thorough tests is a much better use of your time, both for you now and your future colleagues.

##### Self-Similarity

Many beneficial practices work at many scales (acceptance tests, integration tests and unit tests, for example). Try to use practices that transfer between scales.

##### Improvement

Don't aim for perfect, but always aim for better. You can start with the minimal solution that works, and improve it a bit at a time in a process of constant improvement. This is also the best approach for a changing environment where you can improve the parts that are most impacted by a change in the environment.

##### Diversity

Increasing, and valuing, the range of opinions is vital to widen the pool of possible solutions before closing in on the most promising one. A team that is not diverse is redundant.

##### Redundancy

Have more resources and solutions than you need, though not so many that there is \*unnecessary\* waste. 

##### Reflection

Take time to think about what you've done already, but don't spend all day on it.

##### Failure

Don't spend all day talking about what might work until you come to the "right" solution; (quickly) try some out to see what works and what fails, and learn from the process.

##### Flow

Make time to speed up the deployment cycle so that improvements can get to deployment quicker and more frequently so that they "flow." Weekly, fortnightly or monthly release cycles do not sit well with "flow" \- they indicate that the deployment process is so long-winded and fraught that it takes days or weeks, which interrupts the flow of the development cycle. \[You have to stop developing to concentrate on deployment, and by the time you get back to developing you've forgotten where you left off.\]

##### Opportunity

Problems are opportunities for change, maximizing strengths and minimizing weaknesses. Find solutions to common problems (e.g., a programmer who makes too many errors would benefit from pair programming).

##### Quality

This is not only an economic/business consideration but people want to feel good about their work. But don't let an obsession with quality paralyse you; if you can't think of a clean solution, do one that works and then refine it.

##### Baby Steps

Take steps that are small enough not to overwhelm or overcommit. Acknowledge that the overhead of small steps is smaller than the costs of reverting big changes. Continuous Integration is the embodiment of this.

##### Accepted Responsibility

Nothing is your responsibility unless you accept responsibility for it in advance. If you accept a task, you are responsible for estimating what resources it will consume.

## (Primary) Practices

##### Sit together

"No matter what the client says is the problem, it is always a people problem." If workers are separated physically/geographically, it makes it much harder to share information quickly and informally, interrupting that flow of knowledge. Sit teams close together so that information can move around much more freely. If the team is already siloed, find an extra room for them to convene rather than tear down the partitions (see Baby Steps).

##### Whole Team

Ensure every role is covered in team, and structure teams to maximize sense of belonging and value. Smaller teams work better than larger ones (12 is "optimal"). Time-sharing is particularly counterproductive because it interrupts flow; allow people to stay focussed for a long as possible by allocating roles accordingly.

##### Informative Workspace

Workers should be surrounded by feedback to keep them informed of progress. Story walls and information radiators keep them \- and any visitors \- informed of how the project is going.

##### Energized Work

Don't burn out yourself or your team; respect your health and lifestyle by maintaining a healthy work-life balance.

##### Pair Programming

Sit programmers together in pairs. You'll add more value with a partner to bounce ideas off and get feedback from. Rotate partners to maximize diversity in thinking and knowledge shared. It's hard work and tiring, but pays off. (As an aside, respect your partner's personal space and take care of your personal hygiene; failing to do either will be off-putting and disrupt the pairing process. Cultural differences could make pair programming tricky, but every problem has a solution.)

##### Stories

Use "stories" rather than "requirements" (which most of the time aren't really required). Stories involve the customer at the earliest opportunity and use the language that the customer is familiar with. Write stories on physical cards and place them on a physical wall; it almost always works better than any computerized solution. Be specific if there are actual requirements involved, and estimate resources as early as possible to help prioritise stories.

##### Weekly Cycle

Plan (tactically) work one week at a time. Start the week deciding which stories would add the most value, then divide stories up into tasks that workers can accept. Write the acceptance tests first, and then begin the implementation. If it looks as though the work will not be complete in one week, prioritise further and defer less-valuable stories for the following week.

##### Quarterly Cycle

Plan (strategically) work one quarter at a time. Focus on "themes" rather than stories to maintain an eye on the bigger picture (rather than getting excited over particular stories), identify bottlenecks and necessary repairs, and shortlist a quarter's worth of stories related to the theme from which you'll pick work at the weekly meetings.

##### Slack

Don't attempt to work at 100% \- everything will grind to a halt the minute something unexpected happens, which will undo all of your previous "productivity" (which won't have been that productive in reality). Aim for 80-90%, taking time off to do unplanned "fun" tasks or some downtime.

##### Ten-Minute Build

*Automatically* build the *whole* system in *ten minutes*. Anything less than that is too slow a cycle time, and will reduce the number of check-ins to the repository (and the feedback cycle that is so important). Start by automating all builds; don't try to estimate which parts of it need building and testing (the answer is always "all of it"); optimize until you can get the cycle time down to ten minutes. \[The book is fairly old \- modern ones suggest 1-2 minutes.\]

\[My build time is currently 15-30 minutes, but I honestly can't see how to reduce that. Testing, however, could be reduced dramatically with better designed tests.\]

##### Continuous Integration

Integrate code and build & test the system frequently. Waiting a short amount of time for that feedback is good \- it gives us time to reflect \- but waiting hours is terrible.

##### Test-First Programming

Test-Driven Development (as it's now known) helps to avoid scope creep (because you're focussed on solving a specific problem), reduces coupling and increases cohesion ("If it's hard to write a test, it's a signal you have a design problem not a testing problem."), increases trust (because your teammates can see that you care about your code working), and improves rhythm (because you always know what you need to do next \- either get the broken test to work, or write another test).

##### Incremental Design

Design your code to be amenable to change (when change is, inevitably, required). Consider your design daily, and improve the design whenever the conditions are right. Much of the "design-first" approach comes from a study in the 1960s that identified design changes as particularly costly, but that is much to do with the fact that systems weren't designed with change in mind in the first place. Accept that your design will be suboptimal, that it can be improved, and use that as an opportunity to learn.

### Getting Started

If you're already in the middle of a project and looking to transition to XP, making that transition can be difficult. Try using XP planning concepts to identify "stories" based on practices, and try one or two per week. Identify the values and principles that each supports for reassurance, and draw a map of how those practices feed in and why we use them. Don't try to force a team to transition against their will \- put out feelers until you grasp when the time is right to introduce a change that will be embraced rather than rejected.

## (Corollary) Practices

##### Real Customer Involvement

Get the customer onto your team, making real contributions as early (and quietly) as possible. They will be a valuable addition.

##### Incremental Deployment

Avoid Big Bang deployments as much as possible. Make time to override existing functionality a piece at a time so that you always have a working system in which you have confidence. At the very least have a system for rolling back to the previous system.

##### Team Continuity

Keep teams together for as long as possible. Programmers are not typists who can be “returned to the pool” and team dynamics are as important to getting things done as programming skills.

##### Shrinking Teams

As a team becomes more capable, one or more team members will become gradually redundant. Use this as an opportunity to create new teams from “spare” team members, but keep the new teams together until it is time to split those, too.

##### Root-Cause Analysis

When something goes wrong, find out what the “people problem” was that led to it. (And it’s almost always a people problem.) Use the Five Whys to get to the root cause and try to resolve that in addition to fixing the immediate (technology) problem unless you want the problem to recur.

##### Shared Code

Every coder should be able to change any code; do not have locks or gatekeeping. This does, however, require a level of trust among team members to act responsibly, so use this practice with care.

##### Code and Tests

Code and tests should be the only permanent artifacts; everything else of value (e.g., documentation) should be derived from code and tests. The less burden you have of maintaining overweight requirements documents, the better \- requirements are better stated as acceptance/customer tests.

##### Single Code Base

Everyone commits to one code branch; there are no branches or forks. The burden of maintaining multiple code bases is not worth the effort for the value it adds.

##### Daily Deployment

Put new software into production every night to minimize the feedback cycle time; the greater the disparity between development code and production code, the riskier each deployment becomes. Aiming for this encourages you to complete work in smaller chunks with better testing.

##### Negotiated Scope Contract

Contracts should fix time, costs and quality, but leave scope open to negotiation. A sequence of small, short contracts is better than one big one. Smaller contracts also encourage a shorter feedback cycle.

##### Pay-Per-Use

Again, shorten the feedback cycle through your payment schedule. Extracting smaller payments more often gives you a sense of who wants your software and how badly. Other options include Pay-Per-Release (unpopular among customers) or a subscription model, all of which serve the purpose.

## The Whole XP Team

Everyone has to be on the same page. Otherwise, you’re like a group of hikers heading up a glacier, all tied together but arguing over who goes first \- it really doesn’t matter, so walking all abreast is a perfectly sensible approach.

Testers ensure that automated system tests are in place at the beginning of the work unit so that there is evidence that the work unit was successful. After writing the system tests, testers continue writing tests as new details emerge about how the system should behave, and they help programmers when a particularly thorny issue arises.

Interaction Designers ensure that the interface between the software and its user is fit for purpose, working closely with potential users to maximize flow and feedback.

Architects look for large-scale refactorings that would benefit the system as a whole, perhaps using stress testing to uncover or shine a light on areas where the architecture could be better. Once a failing test has been written, the architecture can be updated so that the test passes.

Project Managers ensure optimal communication among the members of the team, and between the team and outside customers. They are constantly planning (rather than doing it all up-front) and make introductions between people that speed up communication (rather than acting as a go-between/bottleneck).

Product Managers choose stories and themes for the work unit, and help team members prioritize stories by focussing on the business (rather than technical) desires. They also facilitate communication between the team and customers.

Executives are responsible for the overall success of the project/product and control things like funding and how the team is presented to the rest of the organization. They must have frequent and accurate information/communication and must not be hoodwinked when things aren’t going well. They use metrics such as Defect Rate and Time to Return on Investment to judge the success of a project, and know that silence around the group is a problem being stored up.

Technical Writers turn features into prose and media (pictures, video, audio) that explain the features in domain-specific language that the user will understand. The difficulty in XP is doing this very shortly after (or, ideally, at the same time as) the feature is written. Reducing this lag should be a goal of the team. If you store manuals online at your site, monitor their usage and ditch anything that never gets read; if manuals are packaged with the software, add monitoring to the software so that you can track manual usage off-line.

Users are a conduit to the larger user community and have experience using systems like the one under development. They can help to write stories and themes, serve as an ambassador when the time is right, and ask helpful questions as development proceeds.

Programmers work closely together to write stories, themes and code, so they need good social skills.

Human Resources are in charge of reviews and hiring (among other things) which poses the question of how individual merit should be rewarded in a team-based system, and whether to hire an extremely skilled loner over a competent-but-social programmer (always the latter).

Roles in an XP team are not fixed or rigid, though this may be more the case at the outset. As the team matures and develops trust in each other, some workers can act in roles different to their nominal value if they show specific aptitude for the task. Authority, accountability and chain of command, however, must be respected.

## The Theory of Constraints

Every system has a bottleneck somewhere \- the narrowest pipe through which work can pass, which in turn defines the fastest your system can operate. Finding and resolving this bottleneck is the key to efficient optimization \- look for where work is piling up, waiting to be done.

XP does not propose to address bottlenecks elsewhere in the organization, only the software development. This does not mean that it won’t happen \- a more efficient and productive development team shifts the bottleneck to another department in the organization, and that department may not be happy. Without a strong executive on your side, be prepared to take the blame for others’ failures that have suddenly become apparent.

## Planning: Managing Scope

p92