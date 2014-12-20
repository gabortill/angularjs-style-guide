# AngularJS Style Guide
This is my guide for writing consistent AngularJS code.



## Table of contents
1. [Single Responsibility](#single-responsibility)
1. [IIFE](#iife)
1. [Module](#module)
1. [Controller](#controller)
1. [Service and Factory](#service-and-factory)
1. [Directive](#directive)
1. [Filter](#filter)
1. [Angular $ Wrapper Service](#angular-$-wrapper-service)
1. [Performance](#performance)
1. [](#)



## Single Responsibility
Define 1 component per file.

**why?**

* Improve readability.
* Easier to understand the component.
* Single responsibility principle is part of the OOP.

```js
// BAD
function SomeController() {}

function someFactory() {}

angular
      .module('app', [])
      .controller('SomeController', SomeController)
      .factory('someFactory', someFactory);
```

```js
// GOOD
function SomeController() {}

angular
      .module('app', [])
      .controller('SomeController', SomeController)
```

```js
// GOOD
function SomeFactory() {}

angular
      .module('app')
      .controller('SomeFactory', SomeFactory);
```



## IIFE
1. Wrap AngularJS components in an Immediately Invoked Function Expression.
1. Use strict mode.

**why?**

1. Avoid polluting the global namespace helps prevent variables and function declarations from living longer than expected in the global scope, which also helps avoid variable collisions.
1. Strict mode enables more warnings and makes JavaScript a cleaner language.

```js
// BAD
function logger() {}

angular
    .module('app')
    .factory('logger', logger);
```

```js
// GOOD
(function () {
    'use strict';

    function logger() {}

    angular
        .module('app')
        .factory('logger', logger);
}());
```



## Module
1. Use lowerCamel for naming.
1. Use the getter syntax instead of stored in a variable.

**why?**

1. Improve readability.
1. Angular recommendation.

```js
// BAD
var badnaming = angular.module('badnaming', []);
app.controller();
```

```js
// GOOD
angular
  .module('goodNaming')
  .controller();
```

**[⬆ back to top](#table-of-contents)**



## Controller

### controllerAs
Use the controllerAs syntax over the classic controller with $scope syntax.

**why?**

The controllerAs is <a href="http://en.wikipedia.org/wiki/Syntactic_sugar" target="_blank">syntactic sugar</a> over $scope.

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



## Service and Factory

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



## Directive

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



## Filter

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



## Angular $ Wrapper Service

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



## Performance

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**



##

**why?**

```js
// BAD
```

```js
// GOOD
```

**[⬆ back to top](#table-of-contents)**