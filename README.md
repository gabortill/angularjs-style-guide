# AngularJS Style Guide
This is my guide for writing consistent AngularJS code.



## Table of contents
1. [Module](#module)



## Module
1. Use lowerCamel for naming.
2. Use the getter syntax at all times instead of stored in a variable.

**why?**

1. Improve readability.
2. Angular recommendation.

```js
// BAD
var mainmodule = angular.module('mainmodule', []);
app.controller();
```

```js
// GOOD
angular
  .module('mainModule')
  .controller();
```

**[⬆ back to top](#table-of-contents)**