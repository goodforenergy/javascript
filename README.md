# Leafnode JavaScript Standards

*Adapted from the [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).*

## Table of Contents

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas & Semicolons](#commas--semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [AngularJS](#angularjs)
  1. [Testing](#testing)

## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. Use readable synonyms instead.

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      class: 'alien'
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation

    ```javascript
    // bad
    var items = new Array();

    // good
    var items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    var someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    var len = items.length,
        itemsCopy = [],
        i;

    // bad
    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    itemsCopy = items.slice();
    ```

  - To convert an array-like object to an array, use Array#slice.

    ```javascript
    function trigger() {
      var args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Strings

  - Use single quotes `''` for strings

    ```javascript
    // bad
    var name = "Bob";

    // good
    var name = 'Bob';
    ```

  - Strings longer than 140 characters should be written across multiple lines using string concatenation.

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

**[⬆ back to top](#table-of-contents)**

## Functions

  - Function expressions:

    ```javascript
    // anonymous function expression
    var anonymous = function() {
      return true;
    };

    // named function expression
    var named = function named() {
      return true;
    };

    // immediately-invoked function expression (IIFE)
    (function() {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - Never declare a function in a non-function block (if, while, etc). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently.

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    var test;
    if (currentUser) {
      test = function test() {
        console.log('Yup.');
      };
    }
    ```

  - Never name a parameter `arguments`, this will take precedence over the `arguments` object that is given to every function scope.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Properties

  - Use dot notation when accessing properties.

    ```javascript
    var luke = {
      jedi: true
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**

## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace.

    ```javascript
    // bad
    superPower = 'Strength';

    // good
    var superPower = 'Strength';
    ```

  - Use one `var` declaration for multiple variables and declare each variable on a new line, indented by one tab.

    ```javascript
    // bad
    var items = getItems();
    var goSportsTeam = true;

    // good
    var items = getItems(),
        goSportsTeam = true;
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i, items = getItems(),
        dragonball,
        goSportsTeam = true,
        len;

    // good
    var items = getItems(),
        goSportsTeam = true,
        dragonball,
        length,
        i;
    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      var name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      var name;

      test();
      console.log('doing stuff..');

      //..other stuff..
      
      name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Hoisting

  - Variable declarations get hoisted to the top of their scope, their assignment does not.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // The interpreter is hoisting the variable
    // declaration to the top of the scope.
    // Which means our example could be rewritten as:
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Conditional Expressions & Equality

  - Use `===` and `!==` instead of `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

  - Use shortcuts.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Blocks

  - Use braces with all blocks. Always place the opening brace on the same line as the control structure declaration with a space before it.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
    if (test) return false;

    // good
    if (test) {
        return false;
    }

    // good
    for (initialization; condition; update) {
        statements;
    }

    // good
    if (someCondition) {
        statements;
    } else if (someOtherCondition) {
        statements;
    } else {
        statements;
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Comments

  - Use `/* ... */` for multiline comments. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param <String> tag
    // @return <Element> element
    function make(tag) {
      // ...stuff...
      return element;
    }

    // good
    /*
    make() returns a new element based on the passed in tag name

    @param <String> tag
    @return <Element> element
    */
    function make(tag) {
      // ...stuff...
      return element;
    }
    ```

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment.

    ```javascript
    // bad
    var active = true;  // is current tab

    // good
    // is current tab
    var active = true;
    ```

  - If using `TODO` or `FIXME` ensure that the issue is tracked somewhere other than the code as well. Address existing `TODO`s or `FIXME`s when you are working with code in that area.

**[⬆ back to top](#table-of-contents)**

## Whitespace

  - Use hard tabs (set to 4 spaces)

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // good
    function() {
        var name;
    }
    ```

  - Place 1 space before the leading brace.

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog'
    });
    ```

  - Surround operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

**[⬆ back to top](#table-of-contents)**

## Commas & Semicolons

  - Do not leave trailing commas at the end of objects or arrays.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    // bad
    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    // good
    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

  - Use semicolons at the end of every statement.

**[⬆ back to top](#table-of-contents)**

## Type Casting & Coercion

  - Use `parseInt` for numbers and always with a radix for type casting.

    ```javascript
    var inputValue = '4';

    // bad
    var val = new Number(inputValue);

    // bad
    var val = +inputValue;

    // bad
    var val = inputValue >> 0;

    // bad
    var val = parseInt(inputValue);

    // good
    var val = parseInt(inputValue, 10);
    ```

  - Booleans:

    ```javascript
    var age = 0;

    // bad
    var hasAge = new Boolean(age);

    // good
    var hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**

## Naming Conventions

  - Avoid single letter names unless as a local loop variable. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  - Use camelCase when naming objects, functions, and instances

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {};
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {};
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use Screaming Snake Case for constants

    ```javascript
    // bad
    var some_constant = 5;

    // good
    var SOME_CONSTANT = 5;
    ```

  - When saving a reference to `this` name it descriptively or use `_this`.

    ```javascript
    // bad
    function() {
      var self = this;
      return function() {
        console.log(self);
      };
    }

    // bad
    function() {
      var that = this;
      return function() {
        console.log(that);
      };
    }

    // good
    function() {
      var _this = this;
      return function() {
        console.log(_this);
      };
    }
    ```

**[⬆ back to top](#table-of-contents)**

## Modules

  - The file should be named with camelCase, live in a folder with the same name but hyphenated, and match the name of the module in the JavaScript.
  - Always declare `'use strict';` at the top of the module.

**[⬆ back to top](#table-of-contents)**

## jQuery

  - Prefix jQuery object variables with a `$`.

    ```javascript
    // bad
    var sidebar = $('.sidebar');

    // good
    var $sidebar = $('.sidebar');
    ```

  - Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      var $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**

## AngularJS

  - AngularJS should be used for new development.
  - Do not mix AngularJS and JQuery. Only use one or the other in a particular feature as JQuery will cause complications with the Angular [$digest cycle](https://docs.angularjs.org/guide/scope#scope-life-cycle).
  - Wrap all Angular code in an [IFFE](http://en.wikipedia.org/wiki/Immediately-invoked_function_expression). It removes variables from the global scope and provides a variable scope for each file once all the JavaScript has been concatenated and minified.

````javascript
    // bad
    // logger.js
    var app = angular.module('app');
    app.factory('logger', logger);

    // logger function is added as a global variable
    function logger() { }

    // good
    // logger.js
    (function() {
        'use strict';

        var app = angular.module('app');
        app.factory('logger', logger);

        // no globals are left behind
        function logger() { }
    })();
````

  - Don't use controllers for business logic, use a service or factory instead.
  - Factories and services should have a single responsibility.
  - DOM manipulation should occur in directives.
  - Prefix directives and attributes used by directives with a short meaningful prefix (this prefix should match your CSS class prefix) to avoid naming clashes.
  - Restrict directives to elements or attributes.
 
**[⬆ back to top](#table-of-contents)**

## Testing

  - All AngularJS code should have unit tests. 
  - 100% test code coverage is achievable with AngularJS. Use [istanbul](http://gotwarlost.github.io/istanbul/) to measure code coverage.

**[⬆ back to top](#table-of-contents)**
