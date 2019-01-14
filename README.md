# DesignPatternsAndTechniques

 Design patterns are solutions to general problems that software developers faced during software development. 
 
 ## DESIGN BY CONTRACT - GETTING RID OF NULL PARAMETERS IN SPRING
 
 *It prescribes that software designers should define formal, precise and verifiable interface specifications for software components, which extend the ordinary definition of abstract data types with preconditions, postconditions and invariants. These specifications are referred to as "contracts", in accordance with a conceptual metaphor with the conditions and obligations of business contracts.*
 
 Design by Contract means:

- each method has a precondition (what it expects before being called)
- each method has a postcondition (what it guarantees, what is returned)
- each class has an constraint on its state (class invariant)

## FIELD DEPENDENCY INJECTION CONSIDERED HARMFUL

       Field
       @Autowired
       private DependencyA dependencyA;

       @Autowired
       private DependencyB dependencyB;

       @Autowired
       private DependencyC dependencyC;
       
       
**What is wrong?**

As you can see, the Field variant looks very nice. It is very short, concise, there is no boilerplate code. The code is easy to read and navigate. Your class can just focus on the important and is not polluted by DI boilerplate. You just put the @Autowired annotation above the fields and that's it. No special constructors or setters just for DI container to provide your dependencies. Java is very verbose as is, so every opportunity to make your code shorter is welcome, right?

 **Field injection drawbacks**

The reasons why field injection is frowned upon are as follows:

- You cannot create immutable objects, as you can with constructor injection
- Your classes have tight coupling with your DI container and cannot be used outside of it
- Your classes cannot be instantiated (for example in unit tests) without reflection. You need the DI container to instantiate them, which makes your tests more like integration tests
- Your real dependencies are hidden from the outside and are not reflected in your interface (either constructors or methods)
- It is really easy to have like ten dependencies. If you were using constructor injection, you would have a constructor with ten arguments, which would signal that something is fishy. But you can add injected fields using field injection indefinitely. Having too many dependencies is a red flag that the class usually does more than one thing, and that it may violate the Single Responsibility Principle.


This is one of the never-ending discussions in software development, but major influencers in the industry are getting more opinionated about the topic and started to suggest constructor injection as the better option.

**Constructor injection** 

Should usually be used for required dependencies allowing them to be immutable.

**_Pros:_**

- Better testability. You do not need any mocking library or a Spring context in unit tests. You can create an object that you want to test with the new keyword. Such tests are always faster because they not rely on the reflection mechanism. (This question was asked 30 minutes later. If the author had used constructor injection it would have not appeared).
- Immutability. Once the dependencies are set they cannot be changed.
- Safer code. After execution of a constructor your object is ready to use as you can validate anything that was passed as a parameter. The object can be either ready or not, there is no state in-between. With field injection you an introduce intermediate step when the object is fragile.
- Cleaner expression of mandatory dependencies. Field injection is ambiguous in this matter.
- Makes developers think about the design. dit wrote about a constructor with 8 parameters, which actually is the sign of a bad design and the God object anti-pattern. It does not matter whether a class has 8 dependencies in its constructor or in fields, it is always wrong. People are more reluctant to add more dependencies to a constructor than via fields. It works as a signal to your brain that you should stop for a while and think about your code structure.

**_Cons:_**

- More code (but modern IDEs alleviate the pain).

 **Setter injection**
 
 Should be usually used for optional dependencies.
 
 ## SINGLE RESPONSABILITY PRINCIPLE
 
 The single responsibility principle is a computer programming principle that states that every module or class should have responsibility over a single part of the functionality provided by the software, and that responsibility should be entirely encapsulated by the class. All its services should be narrowly aligned with that responsibility. 
 
 **Robert C. Martin expresses the principle as, "A class should have only one reason to change,"[1] although, because of confusion around the word "reason" he more recently stated "This principle is about people."**
 
 ### *Art.*
 
  **Article : http://blog.ploeh.dk/2010/02/02/RefactoringtoAggregateServices/**
    
  In outline form, the Introduce Facade Service refactoring follows these steps:

       1. Analyze how dependencies interact to identify clusters of behavior.
       2. Extract an interface from these clusters.
       3. Copy the original implementation to a class that implements the new interface.
       4. Inject the new interface into the consumer.
       5. Replace the original implementation with a call the new dependency.
       6. Remove the redundant dependencies.
       7. Rinse and repeat :)


The beauty of Facade Services is that we can keep wrapping one Facade Service in new Facade Services to define more and more coarse-grained building blocks as we get closer and closer to the application boundary.

Keeping each class and its dependencies to simple interactions also makes it much easier to unit test all of them because none of them do anything particularly complex.

Adhering strictly to Constructor Injection makes it easy to see when one violates the SRP and should refactor to an Facade Service.

  **Personal opinion :** **_This looks like a pretty good idea of creating domain concepts out of unnecessary dependencies and a good way of reducing them. Also, it looks really easy to implement and easy to test. If it has to do with SRP I still have a long way to go to completely understand that but in general, it's a good practice to apply for smelly code._**
  
 ### *Art.*
  
  Article: https://hackernoon.com/you-dont-understand-the-single-responsibility-principle-abfdd005b137
  
 ## OPEN CLOSED PRINCIPLE
 
**A class must be open for extension but closed for modifications.That is, such an entity can allow its behaviour to be extended without modifying its source code.**
