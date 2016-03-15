# JavaScript, The Weird Parts
### Section 2, Types and Operators

----

## Conceptual Aside - Types & JavaScript
### Lecture 19
* Dynamic Typing
Other programming languages rely on Static Typing, so that you, the user, must instruct the compiler on how to inerpret a variable. The JavaScript engine, on the other hand, dynamically resolves what type of data through the variable's value. That means you can also change the variable type. 

This is both powerful -- but can also cause some problems.

## Primitive Types
### Lecture 20
* Primitive Type
A type of data that represents a single value. So -- not an object, as an object is a collection of values.

* `undefined`
`undefined` represents a lack of existence (you shouldn't set a variable to `undefined` -- this is something the JS automatically sets).

*`null`
`null` represents a lack of existence (this is the value you should be setting when you want them to equal nothing).

*`boolean`
`true` or `false`

*`number`
_Floating point_ number, meaning there's always some decimals. Some programming lanuaages have different types for `integer` and `decimal` numbers, but JS has only one number type.

* `string`
A sequence of characters, and both `''` and `""` can be used to specify a string type.

* `symbol`
Used in ES6 -- not going to cover it here. Just know it's coming.

Understanding that we're working with these types, and that the JS engine inerprets them on the fly is important to know as it can be the cause for a bunch of weird bugs, sometimes making debugging difficult/hard to understand.

## Conceptual Aside - Operators
### Lecture 21
* Operator 
An operator is a special function that is syntactically different. Generally, operators take two params and return one result.

Without operators, an addition function might look like this:
```
var a = 3 + 4;

function +(a, b) {
  // Logic to add #s
}

+(a, b);
```
But instead, JavaScript engine provides the option for the ability to write in *infix* notation. That means the function name sits between the parameters.
```
// Normal function call
+(a, b);

// Infix notation
a + b;

// Prefix notation
+a b;

// Postfix notation
a b+;
```
It's important to remember that these infix-noted operators are essentially functions that return a value. 

## Operartor Precedence & Associativity
### Lecture 22
* Operator Precendence
Which operator function gets called first, with function being called in order of highest precedence.

* Operator Associativity
What order an operator gets called in -- left-to-right (left associativity) or right-to-left (right associativity). So, if I have several functions of the same level of precedence, then operator associativity determines whether those operators will be called right-to-left or left-to-right.

## Conceptual Aside - Coercion
### Lecture 24
* Coercion
Converting a value from one type to another. This happens often in JavaScript as it's a dynamically typed language.

For example,
```
var a = 1 + '2';

console.log(a);
```
returns a result of 12, in the type of a string, because the JavaScript engine will _coerce_ a number type into a string type when the addition operator attemps to add (or in this case, concatenate) them together.

## Comparison Operators
### Lecture 25
```
console.log(1 < 2 < 3);
// returns true

console.log(3 < 2 < 1);
// returns true

// This is because, due to left operator associativity, the first operation returns `false`, which is coerced into the number 0 when the next comparison operator is run (false < 1), or (0 < 1). 

```
However, it isn't always obvious what a particular type is going to coerce to. For example:
```
` > Number(undefined); `
` < NaN `
` > Number(null) `
` < 0 `
```
Even though we know `undefined` and `null` both mean a _lack of existence in value_, they're behave differently when coerced. Here we can see that null behaves/coerces in unexpected ways when comparison operators are applied:
```
` > null == 0 `
` < false `
` > null < 1 `
` < true `
```
To save our asses, there are strict equality/inequality operators, `===` and `!==`, that *don't* attempt to coerce types in operator functions. This way, we can compare values _and_ their types to ensure they're truly the same.

## Existence and Booleans
### Lecture 27
How can coercion be useful? In the case of `if / else` statements, the condition is attempting to resolve as a boolean. We know that values of `undefined`, `null`, `0` and `""` will return `false` -- so we can use that coercion to our advantage.

## Default Values
### Lecture 28
We can use the `or` operator to set a default value in a function as a check. For example, if we call the function below and don't pass a parameter, the function won't throw an error, but `name`, in it's execution context, will remain set to the value of `undefined`.

However, if we use an `or`, that function will attempt to resolve whatever _coerces_ to true. Knowing `undefined` will return false, we can expect that if no parameter is passed, the operator will look to the `string`, which _coerces_ to true, and set that as the value.
```
function greet(name) {
  name = name || '`<Your name here>`';
  console.log("Hello " + name);
}

greet();
greet("Ksenija");

```