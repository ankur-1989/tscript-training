
# Modules

After Ecmascript 2015, concepts of modules came into picture. 
They are executed with in their scope instead of global scope. Any variable, function, object etc declared in a particular module are not visible outside the module unless they are explicitly exported using one of the export forms.


## Module Loaders

* Node.js loaders for CommonJs modules
* [RequireJs](https://requirejs.org/) loader for [AMD](https://github.com/amdjs/amdjs-api/blob/master/AMD.md) modules.

In typescript, a file without any import or export is considered a script and is available in global scope or to all modules.

## Export forms

```js
export interface StringValidator {
  isAcceptable(s: string): boolean;
}
```

```js
import { StringValidator } from "./StringValidator";
export const numberRegexp = /^[0-9]+$/;
export class ZipCodeValidator implements StringValidator {
  isAcceptable(s: string) {
    return s.length === 5 && numberRegexp.test(s);
  }
}
```

* Exports can be renamed
* A module can wrap one or more modules and combine all their exports.

### Default exports

Each module can optionally export a default export. Default exports are marked with the keyword default; and there can only be one default export per module. default exports are imported using a different import form.

* export all as x

```js
export * as utilities from "./utilities";

import { utilities } from "./index";
```

* export for require

```js
let numberRegexp = /^[0-9]+$/;
class ZipCodeValidator {
  isAcceptable(s: string) {
    return s.length === 5 && numberRegexp.test(s);
  }
}
export = ZipCodeValidator;
```


## Import forms

```js
import { ZipCodeValidator } from "./ZipCodeValidator";
let myValidator = new ZipCodeValidator();
```

* imports can also be renamed
* import the entire module into single variable and use it to access the module exports.

```js
import * as validator from "./ZipCodeValidator";
let myValidator = new validator.ZipCodeValidator();
```

* import a module for side effect only, though not recommended.

* import for require

```js
import zip = require("./ZipCodeValidator");
// Some samples to try
let strings = ["Hello", "98052", "101"];
// Validators to use
let validator = new zip();
// Show whether each string passed each validator
strings.forEach((s) => {
  console.log(
    `"${s}" - ${validator.isAcceptable(s) ? "matches" : "does not match"}`
  );
});
```

* Importing types

```js
// Re-using the same import
import { APIResponseType } from "./api";
// Explicitly use import type
import type { APIResponseType } from "./api";
```
