# Mastering TypeScript

Notes on the 2023 edition of the course from [Udemy][1]

## General

* Compile to JavaScript using `tsc my_code.ts`
* Execute compiled code via `node my_code.js`
* Use [prettier][2] to format TypeScript (TS) code
* Functions are declared using the _function_ keyword.
* Objects _seem_ similar to _dicts_ in Python

## Questions

* How do formatted string literals work in TS? (Similar to f-string in Python?)

## Annotations

* Type inference is usually working good enough, so that we do not need to annotate types
* By using the _any_ type, all of TS rules are no longer enforced
    - In order to prevent this, always annotate variables which are not assigned a value initially
* Strings are annotated via _string_
* All numeric types are annotated using _number_
* Boolean types are annotated using _boolean_

### Function Types

* Type annotations and default parameters for functions work just like in Python 
    - mind the usage of semicolons to separate properties of objects
    - be aware of inline objects containing excess properties
* If a function does not return anything, annotate it with with _void_ (same as _None_ in Python)
* A special case it he _never_ type, which is used to annotate functions that never should return anything (i.e. infinite loops)

### Object types

* Type aliases are created using the `type` keyword, i.e. `type person = {first_name: string, last_name: string}`
* We can make a property of an object optional by placing a `?` after its name
* We can declare a property of an object as _readonly_ by placing the keyword before the name of the argument
* We can combine the intersection of two types to define a new type by using `&`

### Array Types





[1]: https://covestro.udemy.com/course/learn-typescript/
[2]: https://prettier.io/