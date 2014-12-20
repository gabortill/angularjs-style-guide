# AngularJS Style Guide
This is my guide for writing consistent AngularJS code.



## Table of contents
1. [Single Responsibility](#single-responsibility)
1. [File Naming](#file-naming)
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
* <a href="http://en.wikipedia.org/wiki/Single_responsibility_principle" target="_blank">Single responsibility principle</a> is part of the OOP.

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



## File naming
Use the following a pattern for naming files that describes the component's feature then its type:
<br />
feature.type.js

**why?**

Provides a consistent way to quickly identify components.

```js
// BAD

// userLoggerFactory.js
// Model.Bio.controller.js
```

```js
// GOOD

// userLogger.factory.js
// ModelBio.controller.js
```

**[⬆ back to top](#table-of-contents)**



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
1. Use lowerCamelCase pattern for naming.
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

### naming
1. Use UpperCamelCase naming convention.
1. Append the controller name with the suffix Controller.

**why?**

1. UpperCamelCase is conventional for identifying object that can be instantiated using a constructor.
1. The Controller suffix is more commonly used and is more explicitly descriptive.

```js
// BAD

// ModelBio.controller.js
function ModelBio() {}

angular
    .module
    .controller('ModelBio', ModelBio);
```

```js
// GOOD

// ModelBio.controller.js
function ModelBioController() {}

angular
    .module
    .controller('ModelBioController', ModelBioController);
```

### controller logic
Avoid writing logic in Controllers, delegate to Factories/Services.

**why?**

Maximises reusability, encapsulated functionality and makes testing far easier and persistent.

```js
// BAD
function BadController() {
    var vm = this;
    vm.doSomething = function () {};
}
```

```js
// GOOD
function GoodController(someService) {
    var vm = this;
    vm.doSomething = someService.doSomething;
}
```

### controllerAs
Use the controllerAs syntax over the classic controller with $scope syntax.

**why?**

The controllerAs is <a href="http://en.wikipedia.org/wiki/Syntactic_sugar" target="_blank">syntactic sugar</a> over $scope.

```js
// BAD
<div ng-controller="BadCtrl">
    {{ badObject }}
</div>
```

```js
// BETTER
<div ng-controller="BetterCtrl as better">
    {{ better.betterObject }}
</div>
```

```js
// BEST

// config.js
function config ($routeProvider) {
    $routeProvider
        .when('/', {
            templateUrl: 'views/best.html',
            controller: 'BestController',
            controllerAs: 'best'
        });
}
angular
    .module('app')
    .config(config);

// best.html
<div>
    {{ best.bestObject }}
</div>
```

### this keyword
Use a capture variable for this when using the controllerAs syntax. Choose a consistent variable name such as vm, which stands for ViewModel.

**why?**

The this keyword is contextual and when used within a function inside a controller may change its context. Capturing the context of this avoids encountering this problem.

```js
// BAD
function Customer($scope) {
    $scope.name = {};
    $scope.sendMessage = function() {};
}
```

```js
// BETTER
function Customer() {
    this.name = {};
    this.sendMessage = function() {};
}
```

```js
// BEST
function Customer() {
    var vm = this;
    vm.name = {};
    vm.sendMessage = function() {};
}
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