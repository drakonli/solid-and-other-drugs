> Everything written here is my interpretation of Robert C. Martin's work and tons and tons of googling and practicing as a php developer.
If you just laughed at php know this: if you think that language matters, then you are probably the one making it worse.

<h1> Single Responsibility Principle, MOTHERFUCKER! </h1>

<h2>WHAT IS THAT, MOTHERFUCKER?!</h2>

[SRP is a programming principle introduced by Robert C. Martin's][1], that states that <b>"a class should have only one reason to change"</b>.
A reason to change is when we have to modify the way the class handles a responsibility. So one reason to change per one responsibility.
And "there should only be ONE reason to change", therefore there should be only one - <b>A SINGLE</b> - responsibility.

The simplest way to understand SRP is to read about [coupling][2].
Code coupling is interdependence between modules (packages/bundles/etc.) and violating SRP basically means coupling inside of a class.

<h2>EXAMPLE, MOTHERFUCKER!</h2>

Simplest example is code that handles database operations.

Violation of the SRP:

```php
class BadUserEntity
{
    public function getId();

    public function getUsername();
    
    public function connect();

    public function sendRequest();
}
```

After fixing the violation of SRP:

```php
class GoodUserEntity
{
    public function getId();

    public function getUsername();
}

class DB
{
    public function connect();

    public function sendRequest();
}
```

*Class "DB" could further be separated into "Connection" and "Repository", but you should choose how to separate you classes yourself.*

Does BadUserEntity class seem familiar? That's because it's Active Record pattern. [And yes, it violates the SRP principle][3]. 
Remember all those protected/private (God, I hope you at least know about encapsulation) methods you cramped into one class? 
Well, my friend, you've created a lot of problems for someone. Maybe even for yourself.


<h2>WHAT'S THE BIG PROBLEM, MOTHERFUCKER?!</h2> 

1. When adding new fields to BadUserEntity (like password) you will have to make sure you didnt break other responsibilities 
(sending request, making a connection).
1. When modifying BadUserEntity in any way you have to modify tests, that you wouldn't have to otherwise 
(if Connection was a separate object).  
1. When a class consists of different methods most developers assume that you can depend one method on another and that
means that when the time comes to change one responsibility another developer would be forced to change the dependent responsibility.
1.  When sending a request to user table you will always have to access BadUserEntity object. For example, when trying to find out
if the entity exists in database the code would have to know about the entity and all it's fields and functions, 
which means more test cases and more situations where changes to fields of entity would break the code that 
uses "sendRequest" part of the entity. Basically, this means a violation of [Law of demeter][4] which I will review in another article.
1. Your class becomes a lot bigger and a lot harder to read and maintain. The point of good code is always readability 
and maintainability, otherwise we would all write in a single file of code all the time.

<h2>WHAT SHOULD I DO, MOTHERFUCKER?!</h2>

Every time you write a new class (or modify an old one, if you're crazy like me) you should ask yourself
"How many reasons does this class have to change? How many responsibilities does it have?" if the answer is more then one, then <b>SEPARATE THE CODE INTO ANOTHER CLASS, MOTHERFUCKER!</b>

**P.S. the style is inspired by [motherfucking programmers!][5]**

[1]: https://en.wikipedia.org/wiki/Single_responsibility_principle
[2]: https://en.wikipedia.org/wiki/Coupling_(computer_programming)
[3]: https://en.wikipedia.org/wiki/Active_record_pattern#Single_responsibility_principle_and_separation_of_concerns
[4]: https://en.wikipedia.org/wiki/Law_of_Demeter
[5]: http://programming-motherfucker.com/
