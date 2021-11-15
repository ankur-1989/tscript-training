# Classes

Class keyword was introduced in ES2015.

most basic class

```js
class Empty {}
```

- Fields

```js
class Empty {
  x: number;
  y: number;
}
let p = new Empty();
p.x = 10;
```

The strictPropertyInitialization setting controls whether class fields need to be initialized in the constructor.

- definite assignment assertion operator, !

## readonly modifier

Fields may be prefixed with the readonly modifier. This prevents assignments to the field outside of the constructor.

Class constructors are very similar to functions. You can add parameters with type annotations, default values, and overloads:

- Constructors

There are just a few differences between class constructor signatures and function signatures:

- Constructors can’t have type parameters - these belong on the outer class declaration, which we’ll learn about later
- Constructors can’t have return type annotations - the class instance type is always what’s returned

Just as in JavaScript, if you have a base class, you’ll need to call super(); in your constructor body before using any this. members

## Methods

A function property on a class is called a method. Methods can use all the same type annotations as functions and constructors:

## getters/ setters

```js
class C {
  _length = 0;
  get length() {
    return this._length;
  }
  set length(value) {
    this._length = value;
  }
}
```

TypeScript has some special inference rules for accessors:

- If get exists but no set, the property is automatically readonly
- If the type of the setter parameter is not specified, it is inferred from the return type of the getter
- Getters and setters must have the same Member Visibility

## Class implements clause

Implements can be used to implement interface to make sure class satisfy all the properties that a interface needs. You can implement multiple interfaces.

## extends clause

Classes can be extended from the base class.
derived class can override base class methods.

```js
class Base {
  greet() {
    console.log("Hello, world!");
  }
}

class Derived extends Base {
  greet(name?: string) {
    if (name === undefined) {
      super.greet();
    } else {
      console.log(`Hello, ${name.toUpperCase()}`);
    }
  }
}

const d = new Derived();
d.greet();
d.greet("reader");
```

But, Derived class must follow the base class contract. what does this mean?

## initialization Order

```js
class Base {
  name = "base";
  constructor() {
    console.log("My name is " + this.name);
  }
}

class Derived extends Base {
  name = "derived";
}

// Prints "base", not "derived"
const d = new Derived();
```

What happened here?

The order of class initialization, as defined by JavaScript, is:

- The base class fields are initialized
- The base class constructor runs
- The derived class fields are initialized
- The derived class constructor runs

## Member visibility

- public
  The default visibility of class members is public. A public member can be accessed anywhere:

* protected
* private
  Typescript allows cross instance private access. but there are some caveats which allows to access private members outside class.

```js
class Dog {
  #barkAmount = 0;
  personality = "happy";

  constructor() {}
}
```

## static members

Classes may have static members. These members aren’t associated with a particular instance of the class. They can be accessed through the class constructor object itsel

```js
class MyClass {
  static x = 0;
  static printX() {
    console.log(MyClass.x);
  }
}
console.log(MyClass.x);
MyClass.printX();
```

Static members can also use the same public, protected, and private visibility modifiers
It’s generally not safe/possible to overwrite properties from the Function prototype. Because classes are themselves functions that can be invoked with new, certain static names can’t be used. Function properties like name, length, and call aren’t valid to define as static members

- Type Parameters in Static Members

```js
class Box<Type> {
  static defaultValue: Type; // not legal
}
```

Remember that types are always fully erased! At runtime, there’s only one Box.defaultValue property slot. This means that setting `jsBox<string>.defaultValue` (if that were possible) would also change `jsBox<number>.defaultValue` - not good. The static members of a generic class can never refer to the class’s type parameters
