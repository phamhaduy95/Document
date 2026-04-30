What make a successful testing?

It’s integrated into the development cycleThe only point in having automated tests is if you constantly use them. All tests should be integrated into the development cycle. Ideally, you should execute them on every code change, even the smallest one.

It targets only the most important parts of your code base

It’s important to direct your unit testing efforts to the most critical parts of the system and verify the others only briefly or indirectly. In most applications, the most important part is the part that contains business logic—**the** **_domain model_**.1 Testing business logic gives you the best return on your time investment.  
All other parts can be divided into three categories:  
¡ Infrastructure code  
¡ External services and dependencies, such as the database and third-party systems  
¡ Code that glues everything together

It provides maximum value with minimum maintenance costs

The most difficult part of unit testing is achieving maximum value with minimum  
maintenance costs

¡ Recognizing a valuable test (and, by extension, a test of low value)  
¡ Writing a valuable test

To recognize a test of high value, you need a frame of reference. On the other hand, _writing_ a valuable  
test requires you to also know code design techniques. Unit tests and the underlying code are highly intertwined, and it’s impossible to create valuable tests without putting significant effort into the code base they cover

_The definition of “unit test_

A unit test is an automated test that  
¡ Verifies a small piece of code (also known as a _unit_),  
¡ Does it quickly,  
¡ And does it in an isolated manner

¡ Verifies a _single unit of behavior_,  
¡ Does it quickly,  
¡ And does it in isolation _from other tests_.

Isolation

Classical isolation. Use production-ready collaborator

London isolation: use test double as the replacement for real class or module.

The London school describes it as isolating the system under test from its collaborators. It  
means if a class has a dependency on another class, or several classes, you need to  
replace all such dependencies with test doubles.

A _mock_ is a special kind of test double that allows you to examine interactions between the system under test and its collaborators.

_A test should tell a story about the problem your code helps to solve, and this story should  
be cohesive and meaningful to a non-programmer