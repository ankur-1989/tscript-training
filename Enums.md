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
