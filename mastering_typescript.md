# Mastering TypeScript

Notes for [Mastering TypeScript 2023][1] on Udemy

## General

-   Use the `new` keyword to create instances of classes
-   The project specific version of TypeScript can be included in _package.json_ by simply installing it via `npm install --save-dev typescript`

### Literal types

-   Combining union with literal types is useful to limit potential inputs to an argument, i.e. `let answer: "yes" | "no" | maybe`

### Tuples & Enums

-   Tuples and Enums are exclusive to TypeScript, they don't exist in JavaScript
-   For some reason we can `push` and `pop` values from a tuple once it has been created
-   Use `const enum` to delete any enum-specific code from compiled JavaScript

### Interfaces

-   Interfaces are very similar to type aliases, yet important differences exist:
    -   Interfaces can only describe the shape of objects, while type aliases can describe any type
    -   Interfaces can extended
-   Properties of interfaces can be _readonly_, via `readonly param: str`, or _optional_ via `param?: str`
-   Interfaces can also define required methods via `someMethod(param: str): str`
-   Interfaces can be extended (re-opened) by re-declaring it and adding properties (or methods). A more elegant to extend (inherit) interfaces is using the _extend_ keyword: `interface TypeScriptProgrammer extends Programmer`. You can also extend an multiple interfaces" `interface PythonProgrammer extends Employee, Programmer`

## Configuration

-   Run `tsc --init` to create the configuration file _tsconfig.json_
-   Common [configuration options][2] include:
    -   specifying the files to compile via _files_, _include_ or _exclude_
    -   specifying the output directory for complied files via _outDir_
    -   specifying the version of JavaScript to compile to via _target_
-   Run `tsc file.ts` or simply `tsc` to compile to JavaScript. Use _watch mode_ to compile a file continuously via `tsc -w file.tsc` or simply `tsc -w`

## Excursion to the DOM

-   TypeScript knows about the DOM (try playing with `document`) out of the box
-   Use the [lib][3] option to modify this behavior
-   Use the _none null assertion operator (!)_ to tell TypeScript that something is _guaranteed_ to be not null
-   Use type assertions (var as type) to make TypeScript treat an unknown type as a specific type

## Classes

### JavaScript

-   `constructor()` and `this` are for JavaScript what `__init__()` and `self` are for Python
-   Class fields are syntactic sugar for setting static values via `this.value = 42`
-   JavaScript supports private class fields and methods via prepending hashmarks to the front of the field/method name
-   JavaScript also support getters and setters by prepending _get_ (or _set_) in front of class methods
-   Static properties and methods only exist on the class, not on an instance of the class
-   Classes can inherit from another class via the _extends_ keyword
-   In order to add class fields to child classes, call `super(parentVal)` in the constructor of the child class, followed by the new class fields

### TypeScript

-   Init values must be typed in TypeScript -> Use the parameter properties shorthand syntax to do so
-   Class fields can be declared readonly by putting the `readonly` modifier inf front of them.
-   By default all properties and methods of classes are public. You may put `public` in front of properties/methods in order to signal safe public use to other developers
-   Prepend `private` to a property or method in order to hide them from public access and any other classes
-   Use the `protected` modifier to make properties accessible to child classes
-   `abstract` classes define methods which must be implemented by a child classes (similar to what interfaces do for regular classes)

## Generics

-   Generics are reusable functions (and classes) which support multiple types
-   Generics support type arguments via a special syntax: `genericFunc<Type>(param: Type): Type {}`
-   Generic function can accept more than one parameter: `genericFunc<T,U>(param1: T, param2: U)`
-   We can put constraints on the types generic functions accept: `genericFunc<T extends object>(param: T): T {}` - This function will only accept types of type object
-   We can also assign default types via `genericFunc<T = number>()`
-   While TypeScript is usually able to infer the actual type passed to a generic function, we may have to explicitly state it via `genericFunc<string>("stringParam")`
-   Note: Add a trailing comma (<T,>) to the type definition of arrow functions if you are working with TSX and React

## Type Narrowing

-   Type (`typeof(param === "type")`) and truthiness (`if(param)`) guards help us to narrow down types
-   Triple equals ensure that variables have the same type _and_ value
-   The `in` and `instanceof` operators work the same way as in Python
-   TypeScript allows us to define functions returning _type predicates_ which allow TypeScript to do the actual type narrowing
-   _Discriminated Unions_ help us to do type narrowing on objects containing the same attributes by adding a literal type, i.e. `kind: "literal type"`, to all objects and switching logic on the `kind` property
-   Use the `never` type to guard yourself against forgetting to handling types

## Type Declarations

-   Type declaration files contain the `.d.ts` suffix
-   Install type declaration files via `npm install --save-dev @types/<package>`

## Modules

-   Use ES Modules syntax, ditch namespaces
-   If working within scripts (files without export) all variables and types exist in a shared global scope
-   Changing the _modules_ parameter in _tsconfig.json_ from `commonJS` to `ES6` allows using modules in the browser
-   Use `export default` to export one (and only one) specific object - this object can be imported without named imports (curly braces)
-   Named imports can be renamed via `import {myFunc as myAlias} from ./utils`

## Webpack

-   [Webpack][4] bundles files and dependencies for usage in the browser
-   Using Webpack with TypeScript requires the _ts-loader_ package
-   SourceMaps allows us to debug bundled code

## React

-   Use [create-react-app][5] to easily create React apps with [TypeScript integration][6]
-   TSX allows us to write TypeScript with HTML markup
-   Check out the React + TypeScript [cheatsheet][7]
-   Check out [React Developer Tools][8]

[1]: https://www.udemy.com/course/learn-typescript/
[2]: https://www.typescriptlang.org/tsconfig#references
[3]: https://www.typescriptlang.org/tsconfig#lib
[4]: https://webpack.js.org/concepts/#entry
[5]: https://create-react-app.dev/
[6]: https://create-react-app.dev/docs/adding-typescript/
[7]: https://github.com/typescript-cheatsheets/react
[8]: https://react.dev/learn/react-developer-tools
