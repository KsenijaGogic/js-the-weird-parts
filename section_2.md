# JavaScript, The Weird Parts
## Section 2, Exectution Contexts and Lexical Environment

----

## Conceptual Aside - Syntax Parser, Lexical Environments, Execution Contexts
### Lecture 6
* Syntax Parser
"A program that reads your code, determines what it does, and if its grammar is valid." 
Programs such as compilers and interpreters that do the process/work of reading your code, and then converts it to something the computer can understand and inerpret. *It's important to note that these intermediate programs affect how your code executes.*

* Lexical Environments
"Where something sits physically in the code you write."
Lexical -> to do with words or grammar. "A lexical environment exists in programming languages in which *where* you write something is important. We're talking about where something sits, what's around it. That's important." 

* Execution Contexts
"A wrapper to help manage the code that is running." Within the many lexical environments in your code, the execution context is what determines what is actually running. An execution context contains your code, but also beyond it.

*"Just remember. It's important," _he says._*

## Conceptual Aside - Name/Value Pairs, Objects
### Lecture 7
* Name/Value Pairs
"A name which maps to a unique value." The name can be defined more than once, but it can only have *one value in any given _context._* "Remmeber we're talking about execution context, so in any particular execution context, a name can only *exist* and be *defined* with one value."
```
Address = '100 Main St.'
```
* Objects
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

## Global Environment & Global Object
### Lecture 9
Within the exection contexts created when running your code, the base one is the global execution context/environment. "It has a couple of special things that come along for the ride." It's *global* because it's accessible everywhere to everything in your code.

The global execution context (via the JavaScript engine) creates two things for you each time your code runs:
1. Global Object
2. `this`, "a special variable for you"

Note that whenever you run a JavaScript file, even if empty, an execution context is created. In the case of the execution context in a browser, the global execution context created is the window, which can also be access via the keyword `this`.

When we refer to something as *global*, in JavaScript, we mean something that's *not in a function.* In JavaScript, when you create variables and functions outside of functions, those get attached to the Global Object.

*The important thing to note here is that there are all these other things, in the global execution context _that you didn't write._*

## Exectution Context, Creation & Hoisting
### Lecture 10
When the global context is created, what's created, and in memory are you Global Object, `this`, the Outer Environment and memory space is set up for Variables and Functions -- this is called hoisting. What this means is that, before your code executes line-by-line, JavaScript has set aside memory for each variable and function you've defined.

So, these functions and variables exist in memory. But when the code exectues line-by-line, the entirety of a function is placed into memory, while with variables, on setup, all variables are set to `undefined`. It isn't until the code exectutes top-down, that defined values as assigned.

tl;dr the JavaScript engine creates the global context, as well as sets functions and variables to memory space. Functions are assigned in their intirety, while variables assigned a value of `undefined` until the code executes.

## Conceptual Aside - JavaScript & `undefined`
### Lecture 11
All variables, when created in JavaScript (whether or not they have a value assigned to them yet), are set to an initial value of undefined when the global context is created. A value of `undefined` =/= `Uncaught ReferenceError = _x_ is not defined`, which is what the browser returns in an attempt to `console.log()` a variable that hasn't been created yet. This is because when the global context was created, no memory space was set aside for that variable (it doesn't have it in memory.)

"Undefined is a special keyword used by JavaScript to set an initial value to a variable. This leads to a warning: never set a variable to equal `undefined` yourself." Allow `undefined` to only be set to by the JS engine -- this will help you in your debugging in determining what _you_ wrote/affected vs. what the execution context did.

## Execution Context: Code Exectution
### Lecture 12
Following the creation phase is the execution phase. After the Global Object, `this`, and the Outer Environment have been created, and variables and functions have been assigned to memory space, the JS engine then runs (or executes) your code. This is the exectution phase.

## Conceptual Aside - Single Threaded, Sychronous Execution
### Lecture 13
* Single Threaded
One command is being executed at a time. To our perception, as programmers, JavaScript is behaving in a single-threaded fashion, even though many commands may be running in the browser (because JS isn't the only thing happening in the browser.)

* Synchronous
One at a time (and in order that it appears.)

So, single-threaded, synchronous execution refers to how JavaScript behaves in that it runs one command at a time, in order of the commands.

## Function Invocation and the Exection Stack
### Lecture 14
* Invocation
"That just means, running a function. Or, *calling* a function." In JavaScript, we do that by putting the name of the function and then parentheses.

What happens when you invoke a function in JS?

We already know that when you run your JS, the Global Exectution Context is created -- but each function invocation creates it's own execution context, and it's place in what's called the *Execution Stack.* The stack is exactly what it sounds like -- one function on top of the other, with the top-most function being the one that's currently running. And, as each function finishes running, their execution contexts are "popped off" the top of the execution stack.

When you invoke a function in JS, an exection context will be created, and it will create it's own space for variables, and it will run each line of code. This means that, if you invoke a function within a function, that function will create its own execution context, run sychronously, and your initial function will continue to run only run when that second function is done.

This also means that, lexically, it doesn't matter where functions are defined, as they already exist in memory space and run in order of which they're *called*.

Just like for the Global Execution Context, each function's execution context starts with a creation phase -- creating its own exection context, `this`, and setting up variables within it. 

## Functions, Context & Variable Environments
### Lecture 15
* Variable Environment
Where the variables live, where they're created, and how they relate to each other in memory.

Variables created within the `scope` of a function, even if named the same as variables living inside other functions, are unique and distinct. For example:
```
function b() {
    var myVar;
    console.log(myVar);
}
function a() {
  var myVar = 2;
  console.log(myVar);
  b();
}

var myVar = 1;
console.log(myVar);
a();
```
returns, in the console:
```
1
2
undefined
```
Because each `myVar` exists in its own execution context.

## Scope Chain
### Lecture 16
When we request a variable, JavaScript does more than just look in the variable environment of the currently executing context -- it also references its Outer Environment. The Outer Environment doesn't necessarily mean the Execution Context in the *stack below it*, but rather, the Outer Environment is lexically defined.

That is, if you ask for a variable that does not exist in the current execution context, JS will look at the outer reference and attempt to find the variable there. That outer reference is defined by where the function sits lexically.

Execution contexts = not lexically dependent/doesn't matter where you write them (matters where you invoke them.)
Outer Environment = lexically dependent/relational to where that function is sitting, a reference created by the JS engine.

That whole chain is called the Scope Chain.
```
function b() {
  console.log(myVar);   
}
function a() {
  var myVar = 2;  
  b();
}

var myVar = 1;
a();

```
Note that `b();` returns a value of 1, because its outer reference is the global execution context.   

```
function a() {
  
  function b() {
    console.log(myVar);   
  }
  
  var myVar = 2;  
  b();
}

var myVar = 1;
a();

```
This time, however, `b();` now will return a value of 2 -- because its outer reference is now the execution environment is `a();`, because it physically, lexically, is sitting in `a();`.

In the example above, note that we also cannot invoke `b();` from the Global Environment, because it now exists, lexically, in the execution environment of `a();`.
```
function a() {
  
  function b() {
    console.log(myVar);   
  }

  b();
}

var myVar = 1;
a();

```
Now -- `b();` doesn't have a reference to `myVar`, so it looks in it outer reference environment, `a();`, and it again has no reference to it. So, it looks in the outer reference environment of `a();`, the global environment and finds the value of `myVar`. 

Another way to interpret the scope chain is to think, "who made me" -- that is, in which execution context's creation phase was I created. That execution context is the outer environment reference.

## Scope Chain, ES6, and `let`
### Lecture 17
* Scope
Where a variable is available in your code. And, if it's truly the same variable or a new copy.

* `let`
ES6 introduces a way of declaring variables through *block scoping* with the keyword `let`. This is in no way replacing `var`. `let` variables are still set up in memory, but `let` restricts you from accessing the variable (even though it's still in memory) until the line of code is run.

The other important thing to note is that `let` is declared inside a code block, and its only available inside that block, at the time that the code is running.

## Okay -- But What About Asynchronous Callbacks?
### Lecture 18
* Asynchronous
More than one at a time. We may be dealing with code executing other code, and many pieces of code are actually executing within the engine at the same time. But JS is synchronous -- it executes a line at a time. However, we encounter aysnchronous events -- like click events, with many callback functions that run when that action is complete.

So how does JS, synchronous, deal with asynchronous code? The JS engine sits with the browser -- but it's not the only thing happening in the browser. There's the rendering engine, there are HTTP requests, and those may be communicating with the JS engine asychronously -- but still, what's happening *inside _just_ the JS engine is happening synchronously.*

Alongside the Execution Contexts being created when code is being executed line-by-line, there is a parallel list, called the *Event Queue*. So, when we want to access to an event inside JavaScript, the browser, when said event occurs, places it in the Event Queue. 

Once the execution stack is empty (everything invoked has executed), only then does JavaScript look to the Event Queue, and periodically checks on it. As it's watching the Event Queue, it's looking to see if there is any JS dependent on that event to run.

So, JS is still functioning synchronously. It's the browser that's asynchronously placing events in the Event Queue, but JS is still executing line-by-line. *Only once the execution context is complete does JS look at the Event Queue.*






























