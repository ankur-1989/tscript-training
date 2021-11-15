# Generics

## why

To make components resusable

Example: we could describe a function using a specific type or using any.

```js
function identity(arg: number): number {
  return arg;
}
```

Example with type variable in function definition.

```js
function identity<Type>(arg: Type): Type {
  return arg;
}

let withTypeResult = identity < string > "test";

let result = identity("test"); // type argument inference

const anotherResult = identity("test");
```

but, what if we need to use the .length property on the argument?

```js
function identity<Type>(arg: Type): Type {
  console(arg.length);
}
```

it will complain that this property does not exist on this type as it does not know about the type.

## Generic Types

Function call signature styles

```js
let myIdentity: <Input>(arg: Input) => Input = identity;

let secondIdentity: { <Type>(arg: Type): Type } = identity;
```

## Generic Interface

```js
interface GenericIdentityFn {
  <Type>(arg: Type): Type;
}
```

we can also create Generic classes but it is not possible to create generic enums and namespaces.

```js
class GenericNumber<NumType> {
  zeroValue: NumType;
  add: (x: NumType, y: NumType) => NumType;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
  return x + y;
};
```

We can use the above class not only for number type as well as for other types like string.
we can pass multiple type variables as well. The examples above are only useful for a limited amount of cases since we can’t reference any properties in our class definition since the TypeScript compiler doesn’t know what properties T or U contains.

To make the TypeScript compiler aware of the properties that may be used in our class definition, we can use an interface to list all the properties that can be used with the class and the use the extends keyword after the generic type marker to denote the that type may have the list of properties that are listed in the interface.

```js
interface PersonInterface {
  name: string;
  age: number;
}
interface GreetingInterface {
  toString(): string;
}
class GenericPerson<T extends PersonInterface, U extends GreetingInterface> {
  person: T;
  greet(greeting: U): string {
    return `${greeting.toString()} ${this.person.name}. You're ${this.person.age} years old`;
  }
}
let jane = new GenericPerson();
jane.person = {
  name: 'Jane',
  age: 20
};
console.log(jane.greet('Hi'));
```

## Generic Constraints

```js
interface Lengthwise {
  length: number;
}

function loggingIdentity<Type extends Lengthwise>(arg: Type): Type {
  console.log(arg.length); // Now we know it has a .length property, so no more error
  return arg;
}
```

Because the generic function is now constrained, it will no longer work over any and all types

- Using Type parameters in Generic constraints

```js
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

let x = { a: 1, b: 2, c: 3, d: 4 };

getProperty(x, "a");
getProperty(x, "m");
```

- using Class types in Generics
