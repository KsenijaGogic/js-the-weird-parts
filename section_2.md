# JavaScript, The Weird Parts

## Section 2, Lecture 6
### Conceptual Aside - Syntax Parser, Lexical Environments, Execution Contexts
1. Syntax Parser
"A program that reads your code, determines what it does, and if its grammar is valid." 
Programs such as compilers and interpreters that do the process/work of reading your code, and then converts it to something the computer can understand and inerpret. *It's important to note that these intermediate programs affect how your code executes.*

2. Lexical Environments
"Where something sits physically in the code you write."
Lexical -> to do with words or grammar. "A lexical environment exists in programming languages in which *where* you write something is important. We're talking about where something sits, what's around it. That's important." 

3. Execution Contexts
"A wrapper to help manage the code that is running." Within the many lexical environments in your code, the execution context is what determines what is actually running. An execution context contains your code, but also beyond it.

*"Just remember. It's important," _he says._*

## Section 3, Lecture 7
### Conceptual Aside - Name/Value Pairs, Objects
1. Name/Value Pairs
"A name which maps to a unique value." The name can be defined more than once, but it can only have *one value in any given _context._* "Remmeber we're talking about execution context, so in any particular execution context, a name can only *exist* and be *defined* with one value."
```
Address = '100 Main St.'
```
2. Objects
"A collection of Name/Value Pairs."
```
Address:
    {
        Street: 'Main',
        Number: 100,
        Apartment: {
            Floor: 3,
            Number: 301
        }
    }
```
Address is still the name of this object, but now, the value of it become a collection.

## Section 3, Lecture 9
### Global Environment & Global Object
Within the exection contexts created when running your code, the base one is the global execution context/environment. "It has a couple of special things that come along for the ride." It's *global* because it's accessible everywhere to everything in your code.

The global execution context (via the JavaScript engine) creates two things for you each time your code runs:
1. Global Object
2. `this`, "a special variable for you"

Note that whenever you run a JavaScript file, even if empty, an execution context is created. In the case of the execution context in a browser, the global execution context created is the window, which can also be access via the keyword `this`.

When we refer to something as *global*, in JavaScript, we mean something that's *not in a function.* In JavaScript, when you create variables and functions outside of functions, those get attached to the global object.

*The important thing to note here is that there are all these other things, in the global execution context _that you didn't write._*

## Section 3, Lecture 9
### Exectution Context, Creation & Hoisting
When the global context is created, what's created, and in memory are you Global Object, `this`, the Outer Environment and memory space is set up for Variables and Functions -- this is called hoisting. What this means is that, before your code executes line-by-line, JavaScript has set aside memory for each variable and function you've defined.

So, these functions and variables exist in memory. But when the code exectues line-by-line, the entirety of a function is placed into memory, while with variables, on setup, all variables are set to `undefined`. It isn't until the code exectutes top-down, that defined values as assigned.

tl;dr the JavaScript engine creates the global context, as well as sets functions and variables to memory space. Functions are assigned in their intirety, while variables assigned a value of `undefined` until the code executes.








