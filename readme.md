
Workshop: Code
==============

London vs Detroit
-----------------

Ther are two schools: London and Classicicsts.
London schools is propagated by Steve Freeman and Nat (Growing objective designed software driven by tests).

This aproach according to Marcus:
* is more descriptive
* more hard and expensivve on start
* oriented as outside-in aproach

Flow:

<pre>

                                         [ T ] - - > [ D ]
                                          
                     [T] - - > [ B ]---> [   ]
                                
              ,---> [   ]
             /
[T]  - - > [ A ] 
             \ 
              `---> [   ]
                   
                     [T] - - > [ C ]

Legend:
[T] : Test
[ A..D ] : Classes
[   ] : Mock
- - - : testing
----- : depends

</pre>

Classicists school knowna also as Detroit, Chicago and probably Otwocka is backed by Kent Beck. 
This one:
* could be in worse case as Euro tunel(which was builded from 2 ends and didn't met in the middle stright)
* tends to create selfish classes
* delays disign in time, it happens after many classes alredy not exisitng. (here it is where euro tunel cames)

<pre>

                   [ T ] - - \
                              \
           [T] - - \   ,---> [ D ]
                    \ /
              ,---> [ B ]
             /
[T]  - - > [ A ] 
             \ 
              `---> [ C ]
                    /
                   /
           [T] - -'

</pre>

London school:
--------------

### Test Driven - Alghoritms (First exercise - Roman Numbers):
Roman number conversion up to 3559 (@me to be verified)
Our class should be able to convert decimal number into roman number.
As we learned we should start from simplest possible implementation of posibly simplest case. For example it would be number 1. For this case we are using simple String "I" return, without any variable etc. Then we should add next test for multiplications, like II and III it would would make us to write some loop or recursion. Then we could add next letter. After adding test for next letter we are trying to combine them (like ex. VI ). We are ommiting all possible exceptions and hard values like IV, as our alghoritm is not stable enough now. When we are confident about cases like (VII, XX) we can consider adding (IV  and IX). Also do not forget about removing all code duplication as soon as you are on green bar, because TDD may make you to do many code duplication. Refactor it!

Marcus show us his fav. convention: Class name is ending wiht Should, and methods are reading as statement. Example: 
RomanConverterShould.convert_decimal_to_roman.

If you see duplcation of tests, you can agreagate them into parametrized test or to one with many similiar assertions. Depends on simplicity of assertion and trigger.

In roman converter example there was used clever idea of adding IV and IX etc. exceptions as separate letter in enum which was storing all the number-letter mappings.

We learned here how to grow our alghoritm from most simple case to full alghorithm.

### Transformation Priority Promise - TPP

TPP was introduced by Martin Fowler (@me check that)

* ({}-> nil) no code at all -> code that employs nil

* (nil -> constant)

* (constant -> constant+) a simple constant to a more complex constant

* (constant -> scalar) replacing a constant with a variable or an argument

* (statement -> statements) adding more unconditional statements

* ( unconditional -> if ) splitting the execution path

* ( sclar -> array )

* ( array -> container )

* ( statement -> recursion )

* ( if -> while )

* ( expression 0< function ) replacing an expression with a function or ... 
(@me incomplite)

* (variable -> assigngment) replacint the value of a variable

### Assertions / Mocking - (Second excercse - Bank Kata):

When doing assertions, we can use the rule that when we have query method (the one returnin values) we can just save output to variable and then just to our assertions. However, when it comes to command methods (void) things are little more difficult, we need to search for side effects of running this particular command. How to look for side effects? First we should check nearest colaborators of our class, maybe there is some public property which is changing it state, or maybe in worst case there is some operation on DAO or repository. The best aprach is too look for most obvious and nearest collaborators, both in classes or in concept of non existing classes. If we found such we can mock it and verify the behaviour (call). 

Assertions were excercised on 'Bank Kata', where we head to built account and few transactions printed in recipe on console. We need to mock console and time. We learnt how to controll time during test, which is very important part of design.


#### First test:
Marcus explained us mentioned earlier flows. And focused on London school flow. 
We should start from acceptance test, which we should decide which level it is. For example it could be domian level or rest-api level. Depends on how BDD freaks we are. 
This 
First of all, we should start our implementation from _outside_ means from our entry point, the place where message comes in, or rest api call, user is clicking button to start something, into _in_ - inside direction. 


( @me here comes sequence diagram )

Depending on our aproach and dependencies, we could gain differ sequences.

### Working with legacy code (Third excercise - Trip Service)

First of all look on legacy method and decide which path is shortest, it would often case when some GOD IF is not true, or case when some Exception is thrown.
Then write a test which is putting class into state that this shortest path would be invoked. And assert the expected result. 
Do it until all path are covered. Marcus showed us some Test Coverege highlighting tool in eclipse (@me maybe there is similiar tool for Idea, look for it.)

<pre>

chose shortest path -----> understand it -------> write a passing test 
  ^                                                           |
  |___________________________________________________________|

</pre>

However, legacy code coulbe be like minefiled full of dengerous like hard dependencies (singleton calls or objects construction:'new'). 
What you like to do in such cases when you are not able to inject mock for such dependency? You are bending a rules a little and with use of _automatic refactoring tool in IDE_ extracting only the little pice which is creating such instance to protected method. (notice that class api is not changing.)
Then try to change this new method behaviour:

The usefull techincs are: 

* using yur mock lib for doing some spy on your testing subject object and then stub real method with provided in test to return mock. In mockito remember about 'doReturn' style not 'when'

* in your test suit (test class) create a private class which is extending the legacy class and overwrite only the new protected method with one returning mock. This apprach is named as 'seam' (more about it in 'Working With Legacy Code book')

Now when you have 100% test coverage, you would be enough convinient to start refactoring, which would be in oposite direction. Now we are looking for deepest code branch, but be aware of hidden depth. Like the one in Trip excample, with a flag pointing outside. 

<pre>

choose deepest branch ----> extract it 
   ^                              |
   |__________ test ______________|

</pre>

When you are doing extraction, try to keep your variables as close as you can (same scope) and remove it when it could be reduced in favour of in line.
During this process disign should pop out, form the patterns you would see.
Also in such tasks you can find that you need to fight with unballanced abstraction. It's when statements are varied on a abstraction level, like some parts are using domain languge, and other technical one.

(@me add attachement)
[ pdf from codeboard ]

Conference
==========

(@me fix the titles )

Steve Freeman: Test-Driven Development: Thats not what we ment
--------------------------------------------------------------

Started with question: TDD is dead?

I found nice abbreviation to help memorize:
<pre>
D Difficult
O ... (@me check from slides, when they comes)
O Obscure
M Minigless
+
B Britle (frigile ?)
</pre>

TDD is recently evolving and changing it's language toward BDD ideas.
Detroid school could make ppl. do common mistake, that they are writting test per classes. Which is not true. Test are not for every class.

TDD is:

* Steady incremental progress

* constant positive reinforcement ( we are constantly close to green bar, easy to revert, we are not losing controll over what we are doing)

* forcing to thinking about class functionality before coding, we are doing hypothesis

* things breaks when we suppose them to break, they are not unexpected

* supprising simple design is emerging, much simpler then the one form ourr mind

Tips:

* interfaces, not internals

* protocols not interfaces

* from simple to more general

* should explain domian not properties of code

* always test on right level

Then where some thoughts about TDD loop (simple one). (@me attach diagram)
After that Freeman jump to ports and adapters architeture. ( it make us to test our domain with very fast and sipmle tests.) (@me attach diagram)

Freeman emphasis, why TDD is cool:

* TDD is focuse

* it's fav. concrete examples (examples are most effective in communication)

* TDD is empirical - we are constatly checking if we are right.

Freeman is showing realistic diagram:
( @me - here comse bigger diagram of loop design with tdd )
<pre>

understand problem -> Broad bruch desing (architecture) -> Autmoate ( Build, Deployment, E to E tests ) ---(smaller loop)-->  write failure e to e test --- (tdd loop)--> write failing test --> write impl. ---> refactor --- (back to) ---> write failing test ----> (...), (from refactor back to outre loop) --->
 write failure e to e test ----(back to external loop)---> deploy to production -----> Automate & ----> prodution release -----(back to)----> understand problem

</pre>

Another diagram of loop, more realistic inner loop: 

<pre>

write failing test --> make test pass
   ^           |                 |
   |        (..) to write        |
   |           |                 |
   |           >                 |
   |__________Refactor___________|


</pre>

### QA

Freeman in mentined Testing by Money aproach which interesting for click based business. It's monitoring of money per / something in cases of relaese, if what we are doing is increasing money or not. This could build another external loop of testing.


Michal Piotrkowski @mpidev: How we Test-Drive our REST API with Concordion
--------------------------------------------------------------------------

(@me desperatly needs their slides)
Learn about Gojko Adzic, find his blod image of selenium adopters and also his book (mentined on differ presentation)

Author assured as that they are using proposed by Martin Fowler, tests called Subcutaneus tests (pl.: testy podskorne).

Presentatin show power of html dsl for BDD tests. Which could eveolve into living documentatin. Their company creats few addons (now their are close sources) which are for example sending rest messages and checks response, or checks collaboration of components and displays it as diagram.
Also presenter shows some examples with decorated tables looking as invoices, etc. 

### Buzz words

Hateos - remind, term connected to rest
SVG in HTML5 - check possibilities of this solution
Swager - wth is  this?

Jakub Milkiewicz: BDD Lessons learned
-------------------------------------
(@me wait for slides)

Dawid Weiss: Randomized Testing: When a Monkey Can do Better than a Human
-------------------------------------------------------------------------

Check why divide of 10 by 1L shifted << 32 is randomly crashes.

Search in web for page: forbidden-api's checker - page where we can find list which of standard library apis which fails randomized test.

Raodmized test are great thing to work on havy calculation alghortims, for example coordination calculation, sorting and so on.. 
In business cases use of randomized tests are marginal, for example this could be some text alghoritms or thing connected with locals.

As interesting fact, Weiss showed example of newest bug in JDK, connected with Turkish locals. Whe there is for example "DAWID" it would be in turkey "DAWiD" with big I with dot above, which is complytly differ ascii. This could blow JVM. This error was found by randomized test.

When you are using huge amount of ranomized test, it's finding supprisingly huge amount of errors even in well designed tdd code. This is becase tdd is focusing on expected errors which are in fact just excepional misbehaviour which we are introducing to our test. But every else errors which we are not aware, are still in our code. To find them we are running randomized tests.

Thera 3 possibilities:
1) During the randomized test we are doing assertions to some referal implementation of alghoritm (some times naive or not optimized) to check if new version under test is working well.

2) We are not doing assertions and just looking for uncought Runtime Exceptions or other unexpected behaviour, like blowing out JVM.

3) (...) (@me don't remember, search slides afer publication)

Whats important, randomized test needs special environment, or multiple environments, because they should be runing in the infinite loop. Because each run make us more convinient, but we would never achive 100%. Also after each commit, we should update this tests which are running in the loop to start testing new, updated code.

Ranomized test could test not only implementation with ranomized parameters. But environment it self too. For example on differ operating system combination, or differ hardware etc. 
The more variables would be randomized, better results would be.

Randomized tests, needs less work to figure out parameters because randomizer and loop do all the monkey work.

This apraoch to testng of course is not exchanging other type of tests, it is complimentatory. It's just a way to find a lot of new bugs in places where we where not looking doing tdd.

In some aspects, Nat in closing presentation was agree with this idea of mutation/randomized test.

Jakub Kubrynski, Marcin Grzejszczak: Stick to the rules! Consumer Driven Contracts
----------------------------------------------------------------------------------------

Guys show us some tools they put toghether to be able to mock their services. And keep them synchronized during parallel development. 
Server tests are separate from consuer tests. Stub is publicli available for clients as an contract which should not be vaiolated. 

[Kaczmarzyk | http://blog.kaczmarzyk.net] wrote:

"Microservice oriented architecture is very popular nowadays. New testing techniques are required to support release cycles and assure that services comply with their contracts.

Jakub Kubryński and Marcin Grzejszczak presented a very promising way to achieve that – consumer driven contracts. It is like applying TDD to API on the architectural level. In a nutshell, they use Wiremock to create stubs that are used in consumer tests. Then, with Accurate REST, they create tests for the server to verify that it sticks to the contract described by the stub. Server developers maintain and publish the stubs. The apropriate version of the stub can be fetched from the repository when testing a new version of the consumer service in the delivery pipeline.""

Tools:

* wire.mock.org - for consumer tests

* accurest - https://github.com/Codearte/accurest for server tests

* mvcspock

* rest assured

Github: [http://github.com/marcingrzejszczak]
Examples: [github.com/marcingrzejszczak/geecon_tdd_cdc_examples]

Thomas Sundberg: Behaviour Driven Development, BDD, with Cucumber
-----------------------------------------------------------------

This guy just showed Belly Groming example driven by Cucumber tests. 

[https://thomassundberg.wordpress.com/2014/05/29/cucumber-jvm-hello-world/]

There was big emphasis on readability of BDD test as an tool for comunication with business.

Specs should be written in declarative style rather then imperative. This rule would make tests shilden from change and easier to maintain.
Thomas said that easier to maintain are harder to write that means that leasiness wins on the end.

Books:

* Cynefin - Dave Snowden

Konstantin Kudryashov: Moving away from Legacy code with BDD
------------------------------------------------------------

In general: 

* Do not rewrite your entire legacy app. Just beacuse it's legacy and hard to maintain. You need to show benefits, for example find some new functionalities to get our product owners interested in adding new changes.

* New changes should be choose carefully and should map impact on system. You should analyze which parts of old system should be refactored/rewrited. This could be good oportunity to exchanege some parts underhood.

* Changing legacy system to just support new framework is pointless, this could least months, and when time is out, nothing is to be show to your boss.

* When new fetures are planned create BDD specification for them, and work with it until tests would be green.

To deal with Legacy application with bdd there are few approaches:

1. Rewrite the software with proper bdd stories. Boring and would not lead to anything.

2. Write BDD tests for current implemention to cover it behaviour. This would make us safe. Then in small steps, refactor application and glue code for tests to be ready for new features.
This is called _Inside Job_.

3. Choose smallest possible chunks of code and implement them from scrach as new modules driven by BDD. Then redirect application flow to this new modules. This modules would then be oboslete but do not delete them autmatically, because you might be not sure if it would brak anything. 
This new modules could be integrated with the same shared resoures, for example database. Given new module could write into some tables, which are used by legacy parts of system. 
This is called _Bridge_.

We have to ask our self a questions:

1. What is the project gain ( new features )

2. What to fix ( what parts of system needs to be changed)

3. How to fix ( plan the strategy for change )

4. How to support a change

Terms:

* TCIAM - this code is a mass

Books:

* Gajko Adzic - Specification by Example

Clement Delafargue: TDD, as in Type-Directed Development
--------------------------------------------------------

Programming in test-driven or with exeptions is like "Pokemon Driven Development - you must catch them all"

Clement touches very interesting topic of creating apps. with use of language _typing system_. He favorites functional languages, especially Scala and Haskell. However it could be adopten also in Java world. 

With this aprach your apllication is mathematically proven to be correct. This is called Algebraic Design.

Each new type should have smart constructor which assures to have always correct object. (We can use preconditions here)

What should be avoided on all cost in Type-DD?

* nulls! nulls could be anything, there is not way to check by compiler when we pass null reference of what type it is. Also possible bugs are cought on runtime instead of validation.

* Reflection! Reflection also is messing type safety. Also it could introduce some side-effects and mutability.

Important features of language: 

* Tagged types ( something as alias on some type, more powerfull then boxing some type)

* Generics / Templates (Clement said, that languages without generics are not worth of an interest)

Clement recomends Haskell as an very good semantic for Type-Directed Development. He showed us inspiring amd interesting search: Hoogle, which uses types and function signatures to find specific functions.

Clement is using some variation over well known testing pyramid:
<pre>

                                /\
                               /  \
                              /    \
                             / Unit \
                            /  Tests \
                           /----------\
                          / Properties \
                         /  Unit tests  \
                        /----------------\
                       /       TYPES      \
                      /____________________\

</pre>


Read:

* Fast and loose reasoning is morally correct.

* Theorems for free

* P (...) function programing language

* Type and proven languages

* Functional Programming In Scala

* Functional and Reactive Domain Modeling

* @Parametricity - interesting twaeter channel

Terms:

* Hole Driven Development - [ video | http://matthew.brecknell.net/post/hole-driven-haskell/]


Marcin Zajaczkowski: Java 8 brings power to testing!
----------------------------------------------------

This short presentations was about sucesses in improving some popular tools used in TDD comunity with Java8. For example mockito argument captor could benefit from lambdas, with only small prepaeration.

Marcins said that he is commiting to some AssertJ branch which is specially created to benefit from Java 8. To make assertion even more verbose.

There is also interesting tool named catch-exception, this tool is going to be deprecated by new java 8 features. [http://blog.jooq.org/2014/05/23/java-8-friday-better-exceptions/]

Tweeter: @SolitSoftBlog

PS. Marcin has also blog with especially interesting highlights of new Spock 1.0 features.

Jakub Marchwicki: Characterization Tests with JUnit
---------------------------------------------------

Jakub has presented his on the field experience with Characterization Tests proposed in book: Working Effectively With Legacy Code.

Problem which he was facing:
Big system with JSP and Ruby code inside. Code was in PHP style.
Enormous amount of line codes which were totally unreadable.

According to create safe tests net, and get it ready to TDD refactor, he find out, that if he put high amount of data through the system, with logging all outputs and inputs, he would be able to use it to prove his refactored version of application. 

Test were just running system with the same input and that looking for smallest possible change and failing if any was found.

Tweeter: @kubem

Nat Pryce: Lessons Learned Breaking the (TDD) Rules.
----------------------------------------------------

Nat has introduced some of the curved in stone rules introduced by Foler, Martin, Kent. Such us: 'Never write any new line of code without test' etc. 

Nat said:

"(...) it's not about just writing tests, but about (...) the feedback you get from your tests — Nat "

From [Kaczmarzyk | http://blog.kaczmarzyk.net]: 

"That's it. When a test does not give you any real feedback, then it probably has no value. Sometimes it's better not to have a test at all when all that it gives you is just additional code to maintain. Don't write tests just for the sake of doing it. Focus on the stories that you want to tell through your tests."

Nat also introduced some of his projects where he was breaking the rules of tdd just because TDD was not enough, he was solving some non standard problem and only way was introducing some debugging code into the platform production ready platform, just to make thair tests runnable.

There was also big part about randomized tests which are giving very prommising reasults. This tests are similiar to normal tests, but with randomized arguments. And also this tests are breaking a lot of rules of standard TDD. However Nat said that: "Test automation is a search problem". 

TDD is about looking for happy paths in general, rather then looking for bugs, but property based tests are about looking for unknown errors.

As Donald Rumsfeld said:
	
"There are known knowns. These are things we know that we know. There are known unknowns. That is to say, there are things that we know we don't know. But there are also unknown unknowns. There are things we don't know we don't "