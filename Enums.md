# Enums

Enums allow a developer to define a set of named constants. Using enums can make it easier to document intent, or create a set of distinct cases. TypeScript provides both numeric and string-based enums.

## Numeric Enums

```js
enum Direction = {
    Up = 1,
    Down,
    Left,
    Right
}
```

if we leave the initialization entirely, then Up would have the value 0 and so on.

## String Enums

```js
enum Direction {
  Up = "UP",
  Down = "DOWN",
  Left = "LEFT",
  Right = "RIGHT",
}
```

## Heterogeneous Enums

```js
enum BooleanLikeHeterogeneousEnum {
  No = 0,
  Yes = "YES",
}
```

It is not advised to do so.

Each enum member has a value associated with it which can be either constant or computed. An enum member is considered constant if:

```js
enum E {
  X,
}
```
### Example -  Constant Enum Members

```js
enum FileAccess {
  // constant members
  None,
  Read = 1 << 1,
  Write = 1 << 2,
  ReadWrite = Read | Write,
  // computed member
  G = "123".length,
}
```
## Enum Member Types

```js
enum ShapeKind {
  Circle,
  Square,
}
 
interface Circle {
  kind: ShapeKind.Circle;
  radius: number;
}
 
interface Square {
  kind: ShapeKind.Square;
  sideLength: number;
}
 
let c: Circle = {
  kind: ShapeKind.Square, // Error
  radius: 100,
};
```

Enums can also be passed as an object to a function.

```js
enum E {
  X,
  Y,
  Z,
}
 
function f(obj: { X: number }) {
  return obj.X;
}
 
// Works, since 'E' has a property named 'X' which is a number.
f(E);
```

## Reverse mappings 

```js
enum Enum {
  A,
}
 
let a = Enum.A;
let nameOfA = Enum[a]; // "A"
```

## const Enums

const in an enum means the enum is fully erased during compilation. Const enum members are inlined at use sites. You can can't index it by an arbitrary value. In other words, the following TypeScript code

```js
const enum Direction {
  Up,
  Down,
  Left,
  Right,
}
 
let directions = [
  Direction.Up,
  Direction.Down,
  Direction.Left,
  Direction.Right,
];
```

## Ambient Enums

Ambient enums are used to describe the shape of already existing enum types.

```js
declare enum Enum {
  A = 1,
  B,
  C = 2,
}
```


### Useful links on the topic

[Enum Variants](https://stackoverflow.com/questions/28818849/how-do-the-different-enum-variants-work-in-typescript)