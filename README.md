# AngularJS Style Guide
This is my guide for writing consistent AngularJS code.



## Table of contents
1. [Module](#module)

## Module
1. Use the getter syntax at all times instead of stored in a variable.
2. Use lowerCamel for naming.

**why?**

1. Angular recommendation.
2. Improve readability.

```js
// BAD
var app = angular.module('app', []);
app.controller();
```

```js
// GOOD
angular
  .module('mainModule')
  .controller();
```

**[⬆ back to top](#table-of-contents)**