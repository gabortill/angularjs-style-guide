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
            templateUrl: 'best.html',
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

### common
* All services are singletons.
* The value, factory, service, constant, and provider methods are all providers. They teach the Injector how to instantiate the services.

### difference
* When declaring a **service** as an injectable argument you will be provided with
<br />
an instance of the function, like: new FunctionYouPassedToService().

* When declaring a **factory** as an injectable argument you will be provided with
<br />
the value that is returned by invoking the function reference passed to module.factory.

### naming
UpperCamelCase (PascalCase) for naming your services.

**why?**

Used as constructor functions.

### create service
Prefer a service instead of a factory.

**why?**

In this way you can take advantage of the "classical" inheritance easier.
<br />
<a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain" target="_blank">Inheritance and the prototype chain</a>

```js
// BAD
function BadService () {
    this.someMethod = function () {};
}

angular
    .module('app')
    .service('BadService', BadService);
```

```js
// GOOD
function GoodService () {}

GoodService.prototype.someMethod = function () {};

angular
    .module('app')
    .service('GoodService', GoodService);
```

### create factory
Create an object with the same name inside the function.

**why?**

* Any bindings to primitives are kept up to date, and it makes internal module namespacing a little easier.
* Easily see any private methods and variables.

```js
// BAD
function BadService () {
    var someValue = '';

    var someMethod = function () {};

    return {
        someValue: someValue,
        someMethod: someMethod
    };
}

angular
    .module('app')
    .factory('BadService', BadService);
```

```js
// GOOD
function GoodService () {
    var GoodService = {};

    GoodService.someValue = '';

    GoodService.someMethod = function () {};

    return GoodService;
}

angular
    .module('app')
    .factory('GoodService', GoodService);
```

**[⬆ back to top](#table-of-contents)**



## Directive

### DOM manipulation
1. Any DOM manipulation should take place inside a directive.
<br />
BUT
<br />
If alternative ways can be used such as using CSS to set styles or the animation services, Angular templating, ngShow or ngHide, then use those instead. For example, if the directive simply hides and shows, use ngHide/ngShow.
1. DOM manipulation should be done inside the link method of a directive.

**why?**
1. DOM manipulation can be difficult to test, debug, and there are often better ways (e.g. CSS, animations, templates).
1. Any code reusability should be encapsulated (behavioural and markup related) too.

### naming
1. Prefer using the dash-delimited naming format.
1. Prefix your own directive names a two or three letter prefix
<br />
BUT
<br />
Do not use ng prefix.

**why?**

1. Angular recommendation.
1. Prevent override future standards.

### usage restriction
1. Use directives as attributes or elements instead of comments or classes.
1. Use an element when you are creating a component that is in control of the template.
1. Use an attribute when you are decorating an existing element with new functionality.

**why?**

1. Make your code more readable.
1. Angular recommendation
1. Angular recommendation

```js
// BAD
<!-- directive: custom-directive -->
<div class="custom-directive"></div>
```

```js
// GOOD
<custom-directive></custom-directive>
<div custom-directive></div>
<div data-custom-directive></div>
```

### scope
1. Create an isolated scope when you develop reusable components.
1. Use scope.$on('$destroy', ...) to run a clean-up function when the directive is removed.

**why?**

1. Normally, a scope prototypically inherits from its parent. An isolated scope does not.
<br />
<a href="https://docs.angularjs.org/api/ng/service/$compile#directive-definition-object" target="_blank">Directive Definition Object</a>
1. * angular recommendation
* Directives should clean up after themselves.

### controller and link
Use controller when you want to expose an API to other directives. Otherwise use link.

**why?**

Angular recommendation.

### template
Use the Array.join method when for templates.

**why?**

Improve readability.

```js
// BAD
function badDirective() {
    return {
        template: '<a class="btn" href="/"><img alt="home" height="40" src="home.png" width="60" /></a>'
    };
}
```

```js
// GOOD
function goodDirective() {
    return {
        template: '<a class="btn" href="/"><img alt="home" height="40" src="home.png" width="60" /></a>'
    };
}
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