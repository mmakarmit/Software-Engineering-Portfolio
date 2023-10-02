# Code review

## Introduction

This weeks entry into my personal development repository is all about reviewing code. This includes
reviewing whether the code is incorrect in any areas and the overall quality of the code.
The three main areas I am looking at are; [General Principles of Good Coding](https://charitydigital.org.uk/topics/topics/ten-basic-principles-of-coding-9002),
[Best Practice Rules of Good Coding](https://www.thinkful.com/blog/coding-best-practices/),
and [Code Smells](https://refactoring.guru/refactoring/smells).

I feel it is worth mentioning, that I don't feel experienced enough to adequaely review
code yet. This is predominantly due to completing the first two years at Fife College and then
walking into year 3 at university. What I was previously taught were good coding conventions
on previous projects, do not line up with the current conventions I am being taught at all.
For instance, in all my previous work it was expected that everything in a program was commented.
Header comments, and a comment on every variable, method, class, and so on. This is literally
the complete opposite of what is expected now. Moreover, I have never used C# before now. In fact,
my only experience with any C language is a miniscule amount of C++ for a previous project. Therefore, 
I am not going to notice syntax errors and such for a long time, until I familiarise myself with the
language. Finding the time to do this is difficult with the current rate of hand-ins and reading
expected. Due to these frustrating parameters I really struggled with some of the code reviewing
but I will keep sharpening the skills required to improve.


## Code with Errors

```
 1 - class HumanResources
 2 - {
 3 -    static void Main(string[] args)
 4 -     {
 5 -        Person person = new Person();
 6 -        person = person.GetManager();
 7 -     }
 8 - }
 9 - 
10 - 
11 - class Person
12 - {
13 -     public Department Department { get; set; }
14 -     public Person GetManager()
15 -     {
16 -         return Department.GetManager();
17 -     }
18 - }
19 - 
20 - 
21 - class Department
22 - {
23 -     private readonly Person _manager;
24 -     public Department(Person manager)
25 -     {
26 -         _manager = manager;
27 -     }
28 -     public Person GetManager()
29 -     {
30 -         return _manager;
31 -     }
32 - }
```

As can be seen from the code, this code is for a human resources department. A Person object
can be created and can belong to a Department, while also retrieving the manager from that 
department.

For the first category, general principles of good coding, my answer does not line up with correct
answer in the marking schema. Even though I accept I must be wrong, after considering this and
some of the other examples, I feel like a lot of code reviewing is down to personal opinion and
interpretation. This is due to the fact that I believe my answers are in some way still partially
correct. The marking schema says there are no violations to the general principles. My thinking
was there is a slight violation of both the single responsibility principle and the dependency inversion
principle. The single responsibility violation is in the person class, starting at line 11, as it as 
responsible for personal information and determining a manager. The dependency violation is due to the person class
being dependant on the department class with no interface between the two classes for abstraction.
However, I yield these are minor cases and can definitely see the argument for no violations as well.

Best Practice Rules of Good Coding is the second category, and I am glad to say I totally agreed
with the marking schema on this one as I could not find any issues here. The format is in a clear
and consistent manner, appropriate naming is used throughout, good commenting conventions are used,
and the objects and data structures are distinguished between.

The final category, code smells, I agreed with the marking schema again. I am especially glad of
this one as code smells is a completely new aspect of coding to me. I had never heard of the term, or
any of the code smells before. We both agreed, middle man was the code smell apparent here.
This is due to the person class delegating a call to the department class without anything else
taking place. Thus, acting as a middle man. I feel that the presence of middle man here hints
at a chance for better abstraction, possibly reinforcing my idea of dependency inversion being
violated. However, I do understand that adding abstraction doesn't guarantee the removal of the
middle man coding smell.


## Code with my Suggested Changes

```
class HumanResources
{
    static void Main(string[] args)
    {
        Person person = new Person();
        var manager = person.Department?.Manager;
    }
}

class Person
{
    public Department Department { get; set; }
}

class Department
{
    public Person Manager { get; }

    public Department(Person manager)
    {
        Manager = manager;
    }
}
```

What I have done here is first and foremost, removed the GetManager method from the person class.
Thus, eliminating the middle man issue completely. Secondly, I have made accessing the manager
more direct and intuitive by removing the method used to retrieve the manager, and instead exposing
it as a public property. From my limited understanding, this is a better way of using C#.