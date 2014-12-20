# AngularJS Style Guide
This is my guide for writing consistent AngularJS code.



## Table of contents
1. [test](#test)

## Module
Use the getter syntax at all times instead of stored in a variable.

**why?**

angular recommendation

```js
// BAD
var app = angular.module('app', []);
app.controller();
```
```js
// GOOD
angular
  .module('app')
  .controller();
```

**[⬆ back to top](#table-of-contents)**