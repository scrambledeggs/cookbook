# Booky JavaScript Style Guide
- **2 space** indention
- Follows
 [Airbnb's Style Guide](https://github.com/airbnb/javascript/tree/es5-deprecated/es5) with the following exceptions below

## Variables 
- Always end with **semicolon** `;` when declaring/initialising variables
    ```
    // declaration
    var declaration;

    // initialization
    var declaration = 'This is an initialization';
    ```

## Scope

Scope defines where variables and functions are accessible inside of your program. In JavaScript, there are two kinds of scope - **global scope** and **function scope**.

**ES6** introduced new [keywords](http://es5.github.io/#x7.6.1.2) `let` and `const` to improve readability of variable scopes.

**Important**
> While `var` can still be used when declaring/initializing **global** and **function** scopes, use `let` or `const` to emphasize proper context for variables whenever possible.

`let` is the new `var`. Read more about it [here](https://babeljs.io/docs/en/learn#let-const)


  ```
  // This is bad
  var a = 'www.someurl.com'
  function foo() {
    var b = false;
    var c = 123;
  }


  // This is good
  const abc = 'www.google.com';
  
  function foo() {
    const a = 'my variable';
    let b = 1;
    b = b + 1;
  }
  ```

  ```
  // This is bad
  for (var i = 0; i < 10; i++) {
    foo();
  }

  // This is good
  for (let i = 0; i < 10; i++) {
    foo();
  }
  ```

## Lambda
Use lambda expression when passing functions as a parameter.
```
// This is bad
[1,2,3,4].filter(function (value) {return value % 2 === 0});

// This is good
[1,2,3,4].filter(value => value % 2 === 0);
```

## Importing and Exporting Modules

Use ES6 syntax when importing and exporting modules
```
// This is bad
const myModule = { x: 1, y: function(){ console.log('This is a module') }};

module.exports = myModule;
```

```
// This is good
const myModule = { x: 1, y: function(){ console.log('This is a module') }};

export default myModule;
```

```
// This is bad
const myModule = require('./myModule');

// This is good
import myModule from './myModule';
```

## Named vs Default Export
- Use **default export** for classes that are already self-explanatory by classname
```
//------ findFoo.js ------
export default const findFoo = (a, b) => { ... };

//------ main.js ------
import findFoo from './services/findFoo';
findFoo(x, y);
```

- Use **named export** for more complex classes
```
//------ foo.js ------
export function fibConverter(a) { ... };
export function fibParser(a) { ... };
export function isFib(a) { ... };

//------ main.js ------
import { fibConverter, fibParser, isFib } from './foo';

const fib = fibParser(abc);
```
**Important**
> Consider also your [file structure](https://reactjs.org/docs/faq-structure.html) when deciding between the two types of export

## File Structure

- ReactJS
- [ReactNative](javascript/react-native.md)

## External Libraries

- [Lodash](https://lodash.com/docs/4.17.11)
    - Use `_.isNil()` when checking for `undefined` and `null` values. See docs for [4.0](https://lodash.com/docs/4.17.11#isNil)
    - Be mindful when using `_.isNil()` with `arrays` as it will still return `false` if array is empty. Use `_.isEmpty()` instead.
    - Don't use `_.isUndefined()` and `_.isNull()` anymore for cleaner conditional statements

    ```
    // This is bad
    if (_.isUndefined(someFoo) || _.isNull(someFoo)) {
      ...
    }

    // This is good
    if (_.isNil(someFoo)) {
      ...
    }
    ```
    ```
    // This is bad
    const foo = (_.isUndefined(someFoo) || _.isNull(someFoo)) ? 0 : 1;

    // This is good
    const foo = _.isNil(someFoo) ? 0 : 1;
    ```

# Frameworks
- React
- [React Native](javascript/react-native.md)


## Design Patterns and Principles
The following are best practices we have found valuable while developing **React** and **React-Native** apps.

[YouArentGonnaNeedIt](http://c2.com/xp/YouArentGonnaNeedIt.html)
> "Always implement things when you **actually** need them, never when you just **foresee** that you need them."
- Don't implement a library just because it is trending within the Open-Source community.
- When you do need a library, **review** first how maintained it is.
    - Is its __latest commit__ within a year?
    - Are the __commit messages__ clear and concise?
    - Is the __documentation__ comprehensive?
    - Have you reviewed its __issues__? How many are __closed__ vs __open__?
    - Does it have an acceptable amount of __stars__ in relation to the __number of issues__?
- **REMEMBER: a deprecated library within your project could implicate unneccesary dependencies in the future when upgrading React and React-Native**

[KISS](https://people.apache.org/~fhanik/kiss.html)
> "Keep It Simple, Stupid."
  - Don't over-complicate things and introduce unnecessary abstractions. 
  - Consider **DRY** and **Rule of Three** principles before refactoring.
  - This applies from **naming** your **classes**, **variables** and **functions** up to organizing your **file structure**.
  - **REMEMBER: It will be more difficult for newly onboarded developers to understand your code if things were not simple to begin with.**

[DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)
> "Don't Repeat Yourself"
- **Never Cut and Paste within a project**.

[Rule of Three](https://blog.codinghorror.com/rule-of-three/)
> "Three strikes and you refactor"
- **Don't fall into the delusion of reuse.**
- [Do The Simplest Thing That Could Possibly Work](http://c2.com/xp/DoTheSimplestThingThatCouldPossiblyWork.html) first and then consider applying **KISS**, **DRY** and **LoD**

[Law of Demeter (LoD) or Principle of Least Knowledge](https://en.wikipedia.org/wiki/Law_of_Demeter)
> "Talk only to your immediate friends."
- Keep your functions **pure**. Avoid inserting **side-effects** in your code
- Think of a **side-effect** as something that [does two things at once](https://stackoverflow.com/questions/8129105/javascript-closures-and-side-effects-in-plain-english-separately).
- Be wary of input and output that goes through each function.
- We don’t want our functions to know about the entire object map of the system. [Individual functions should have a limited amount of knowledge](https://hackernoon.com/object-oriented-tricks-2-law-of-demeter-4ecc9becad85).
  
  ```
  function printFoo(a) {
    // this is a side-effect
    this.storeFoo(a);
    console.log(a);
  }
  ```

[SOLID](https://medium.com/@cramirez92/s-o-l-i-d-the-first-5-priciples-of-object-oriented-design-with-javascript-790f6ac9b9fa)

[Observer Design Pattern](https://en.wikipedia.org/wiki/Observer_pattern)



---
## Resources

[ES6 Features](https://github.com/lukehoban/es6features#readme)

[Babel](https://babeljs.io/docs/en/learn#introduction)

[`var` vs `let` vs `const`](
https://tylermcginnis.com/var-let-const/)

[Extreme Programming](http://c2.com/xp/ExtremeProgramming.html)

[Head First Design Patterns](http://shop.oreilly.com/product/9780596007126.do)