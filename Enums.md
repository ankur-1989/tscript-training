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

In normal enums, at runtime, it will lookup for Direction and Up. To get the performance boost we use const

```js
const enum Tristate {
    False,
    True,
    Unknown
}

var lie = Tristate.False;
```

generates JS

```js
var lie = 0
```

i.e the compiler:

* Inlines any usages of the enum (0 instead of Tristate.False).
* Does not generate any JavaScript for the enum definition (there is no Tristate variable at runtime) as its usages are inlined.

Inlining has obvious performance benefits. The fact that there is no Tristate variable at runtime is simply the compiler helping you out by not generating JavaScript that is not actually used at runtime. However, you might want the compiler to still generate the JavaScript version of the enum definition for stuff like number to string or string to number lookups as we saw. In this case you can use the compiler flag --preserveConstEnums and it will still generate the var Tristate definition so that you can use Tristate["False"] or Tristate[0] manually at runtime if you want. This does not impact inlining in any way.

## Enums are open ended

You can split (and extend) an enum definition across multiple files. For example below we have split the definition for Color into two blocks.

```js
enum Color {
    Red,
    Green,
    Blue
}

enum Color {
    DarkRed = 3,
    DarkGreen,
    DarkBlue
}
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

## Number Enums as flags

```js
enum AnimalFlags {    
    None = 0,    
    HasClaws = 1 << 0,    
    CanFly = 1 << 1
}
type Animal = { flags: AnimalFlags}

function printAnimalAbilities(animal: Animal) {    
    var animalFlags = animal.flags;    
    if (animalFlags & AnimalFlags.HasClaws) {        
        console.log('animal has claws');    
    }    
    if (animalFlags & AnimalFlags.CanFly) {        
        console.log('animal can fly');    
    }    
    if (animalFlags == AnimalFlags.None) {        
        console.log('nothing');    
    }
}
let animal: Animal = { flags: AnimalFlags.None };
printAnimalAbilities(animal); // nothing
animal.flags |= AnimalFlags.HasClaws;
printAnimalAbilities(animal); // animal has claws
animal.flags &= ~AnimalFlags.HasClaws;
printAnimalAbilities(animal); // nothing
animal.flags |= AnimalFlags.HasClaws | AnimalFlags.CanFly;
printAnimalAbilities(animal); // animal has claws, animal can fly
```

Here:

* we used |= to add flags
* a combination of &= and ~ to clear a flag
* | to combine flags

### Useful links on the topic

[Enum Variants](https://stackoverflow.com/questions/28818849/how-do-the-different-enum-variants-work-in-typescript)
