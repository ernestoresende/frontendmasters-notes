# An Overview of the Three Pillars of JavaScript - Kyle Simpson

### Table of Contents
- [An Overview of the Three Pillars of JavaScript - Kyle Simpson]
- [Types and Coercion](#types-and-coercion)
  - [Primitive Types](#primitive-types)
  - [NaN](#nan)
  - [new](#new)
  - [Converting Types](#converting-types)
  - [Booleans](#booleans)
  - [Coercion Best Practices](#coercion-best-practices)
  - [Checking equality](#checking-equality)
  - [Types Summary](#types-summary)
- [Scope / Closures](#scope--closures)
  - [Undefined / Undeclared](#undefined--undeclared)
  - [Function Expression](#function-expression)
  - [IFEEs](#ifees)
  - [Closure](#closure)
- [this / Prototypes](#this--prototypes)
  - [Keyword this](#keyword-this)
  - [Prototypes](#prototypes)


# Types and Coercion

The three things we want to take a look at here are the **primitive types**, the act of **converting types**, and **checking equality.**

## Primitive Types

- "*In JavaScript everything is an object*"... That's bullshit. There are some historical reasons why people believe that things are this way, but for the most part, they are wrong.
- It is important to advance here with the knowledge that, in JavaScript, variables don't have types, values do.

```jsx
var v;
typeof v; // "undefined"

v = '1';
typeof v; // "string"

v = 1;
typeof v; // "number"

v = false;
typeof v; // "boolean"

// .. AND SO ON
```

- It is wildly known that an declared variable with no value assigned to them will always return `undefined`. But the same is true if the variable itself was never declared in the first place.

---

There are some edge cases:

```jsx
var v = null;
typeof v; // "object" OOPS!
```

This is an old bug in JavaScript that will unfortunately never be fixed, as doing this now would break far too many existing codebases.

```jsx
var v = function() {}
typeof v // "function" hmmm?
```

It might be surprising that we have function as a JavaScript return value, even tough it is only a subtype of the type `object`. It is actually very useful, as we need to know if we have a value that can be called as a function.

```jsx
var v = [1, 2, 3]
typeof v // "object" hmmm?
```

This is clearly an array, but typeof shows us an object. The reason is... who the fuck knows? There are other operations that are able to further distinguish subtypes like this that will be covered in the future.

## NaN

If we try to do some nonsense like this:

```jsx
var greetings = 'Hello World'
var somethingElse = greeting / 2 // ????
```

JavaScript will be like "hey buddy, what the fuck is this?", and you're get an special kind of value: `Nan`. 

People thing that NaN stands for "Not a Number", but that is not the case. If you take the string `'Hello World'` and test it against `Number.isNan()` you would get `false` as a result.

`Nan` stands for an invalid numeric operation. At first there will seem to be no difference, but put your brain to work a little bit and you will clearly see it.

## new

There are some built-in fundamental objects that we wanna take a look at. Some of them can be called upon with `new` keyword:

- `Object()`
- `Array()`
- `Function()`
- `Date()`
- `RegExp()`
- `Error()`

Other are best being avoided in conjunction with the `new` keyword:

- `String()`
- `Number()`
- `Boolean()`

Those are better used as functions, being actively called to convert to a given type based on their arguments.

## Converting Types

This is absolutely necessary in all programs, every programming language. And in a dynamically typed language like JavaScript, that way we do that is by using **coercion.**

The plus operator that we usually use to make string concatenations of any form is overloaded with some implicit behaviors of how thing are going to be converted:

- Number + Number = Number
- Number + String = String
- String + Number = String
- String + String = String

Looking at this makes it very clear that the only situation where a mathematic operation is going to be performed is when all values are numbers. If any other values are a string, all values from the suffer from coercion and become a string.

In order to avoid having surprises with implicit coercion:

```jsx
function addAStudent(numStudents) {
	return numStudents + 1
}

addAStudent (
	Number(studentInputElem.value)
)
```

When we're getting a value from a input element for example, the value will always gonna be a string. The built-in `Number()` solves that.

## Booleans

The concept of booleans are already known. But there are also **truthy** and **falsy** values.

It means: 'Which values, if we tried to convert them to a boolean, would become false, and which ones would become true?"

**Falsy**: "", 0 and -0, null, Nan, false, undefined.

**Truthy**: Everything else bud, objects, arrays and functions included.

The most common case of others things getting converted to a boolean are the test clauses from if statements.

## Coercion Best Practices

A quality JavaScript programs will always embrace coercions, making sure the types involved in every operation are clear.

From the moment you have the care to make sure that types are being correctly manipulated, most of the corner cases that people usually bitch and complain about go away.

## Checking equality

Common knowledge puts equality checking in ground where:

```jsx
== checks values (loose)
=== checks values and types (strict)
```

When in reality it's something like this:

```jsx
== allows coercion (different types)
=== disallows coercion (types are the same)
```

Limiting our equality checking to being only triple-equals is an educated way of saying: "I only want to allow the check to be successful if the data is of the same type, and If I somehow messed up, I want JavaScript to yell at me". This might not be the mental model for all operations.

## Types Summary

Like every other operation, is coercion helpful in an equality comparison or not?

This question puts the onus on you, to be the one responsible for the critical decisions yourself.

`==` is not about comparisons with unknown types.

`==` is about comparisons with known types, and optionally, where conversions can be helpful.

# Scope / Closures

Scope means: where too look for thing in JavaScript. MDN defines it as: "the context in which vallues and expressions are *visible* and can be referenced.

A function serves as closure in JavaScript, and thus creates a scope. For example, a variable defined inside of a function cannot be accessed outside of it.

## Undefined / Undeclared

A variable that is undefined was declared somewhere, but holds no value.

A varibale that is undeclared was never declared anywhere in the visible scope.

## Function Expression

A function that is assigned as a value somewhere.

In the course, Kyle makes a case that, as much as we are used to seeing and using unnamed function expressions on our everyday code, we should create a stronger emphasis on using descriptive function names to achieve better code readability.

There is an in-depth guide to the pros and cons of both code declaration types [here](https://www.freecodecamp.org/news/when-to-use-a-function-declarations-vs-a-function-expression-70f15152a0a0/).

## IFEEs

Stands for Imediatly Invoked Function Expression, a syntax pattern that in most modern use-cases is made obsolete by the use of block-scoped variables introduced in ECMAScript 2015. As such, there could be edge-cases where this proves useful, as seen [here](https://mariusschulz.com/blog/use-cases-for-javascripts-iifes).

## Closure

Is when a function remembers the variables outside of it, even if you pass that function elsewhere.

Reference Material: [https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/scope-closures](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/scope-closures)

# this / Prototypes

## Keyword this

A function's `this` keyword references the execution context for that call, determined entirely by how the function was called. Its all about the call, not about the definition, or where the function is, or where it belongs to.

A this aware function can thus have a different context each time it is called, which makes it more flexible and reusable.

```jsx
function ask(question) {
	console.log(this.teacher, question)
}

function otherClass() {
	var myContext = {
		teacher: 'Suzy'
  }
	ask.call(myContext, 'Why?') // Suzy Why?
}

otherClass()
```

## Prototypes

Goes hand-in-hand with `this`.

The above is an example of what an prototypal class code. This is less common these days, since the `class` keyword was added, but it is important to understand it as it unveils what the `class` keyword is actually doing under the wraps.

```jsx
function Workshop(teacher) {
	this.teacher = teacher
}

Workshop.prototype.ask = function(question) {
	console.log(this.teacher, question)
}

var deepJS = new Workshop('Kyle')
var reactJS = new Workshop('Suzy')

deepJS.ask('Is "prototype" a class?')
// Kyle is 'prototype' a class?

reactJS.ask('Isnt "prototype" ugly?'
// Suzy isnt "prototype" ugly?
```

If we wanted to make a "thing that was like a class" called `Workshop` that could be instantiated several times, it's possible to define a function called `Workshop` (notice that it is `this` aware), and that function is going to become what we call a **constructor**; it is going to act as if it is a constructor for instances of this so-called `Workshop` class.

To add methods into the definition of our `Workshop` class, we add them to the prototype of the of the `Workshop` constructor.
