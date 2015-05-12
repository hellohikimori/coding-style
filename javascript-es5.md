# HKI JavaScript Style Guide

Based on [Jam3 JavaScript Style Guide](https://github.com/Jam3/Javascript-Code-Conventions).


## Table of Contents

  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Conditional Expressions & Equality](#conditional-expressions--equality)
  1. [Blocks](#blocks)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Naming Conventions](#naming-conventions)
  1. [Constructors](#constructors)
  1. [Modules](#modules)
  1. [jQuery](#jquery)
  1. [Tools](#tools)
  1. [License](#license)

## Objects

  - Use the literal syntax for object creation.

    ```javascript
    // bad
    var item = new Object();

    // good
    var item = {};
    ```

  - Don't use [reserved words](http://es5.github.io/#x7.6.1) as keys. It won't work in IE8. [More info](https://github.com/airbnb/javascript/issues/61).

    ```javascript
    // bad
    var superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    var superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    var superman = {
      class: 'alien'
    };

    // bad
    var superman = {
      klass: 'alien'
    };

    // good
    var superman = {
      type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation.

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
    var len = items.length;
    var itemsCopy = [];
    var i;

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

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    var name = "Bob Parr";

    // good
    var name = 'Bob Parr';

    // bad
    var fullName = "Bob " + this.lastName;

    // good
    var fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines using string concatenation.
  - Note: If overused, long strings with concatenation could impact performance. [jsPerf](http://jsperf.com/ya-string-concat) & [Discussion](https://github.com/airbnb/javascript/issues/40).

    ```javascript
    // bad
    var errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // bad
    var errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // good
    var errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  - When programmatically building up a string, use Array#join instead of string concatenation. Mostly for IE: [jsPerf](http://jsperf.com/string-vs-array-concat/2).

    ```javascript
    var items;
    var messages;
    var length;
    var i;

    messages = [{
      state: 'success',
      message: 'This one worked.'
    }, {
      state: 'success',
      message: 'This one worked as well.'
    }, {
      state: 'error',
      message: 'This one did not work.'
    }];

    length = messages.length;

    // bad
    function inbox(messages) {
      items = '<ul>';

      for (i = 0; i < length; i++) {
        items += '<li>' + messages[i].message + '</li>';
      }

      return items + '</ul>';
    }

    // good
    function inbox(messages) {
      items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Functions

  - Never declare a function in a non-function block (if, while, etc).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    if (currentUser) {
      test()
    }

    //..other stuff..

    function test() {
      console.log('Yup.');
    };
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
      jedi: true,
      age: 28
    };

    // bad
    var isJedi = luke['jedi'];

    // good
    var isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    var luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    var isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - Always use `var` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    var superPower = new SuperPower();
    ```

  - Use one `var` declaration per variable.
    It's easier to add new variable declarations this way, and you never have
    to worry about swapping out a `;` for a `,` or introducing punctuation-only
    diffs.

    ```javascript
    // bad
    var items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    var i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    var i;
    var items = getItems();
    var dragonball;
    var goSportsTeam = true;
    var len;

    // good
    var items = getItems();
    var goSportsTeam = true;
    var dragonball;
    var length;
    var i;
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
      var name = getName();

      test();
      console.log('doing stuff..');

      //..other stuff..

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // bad
    function() {
      var name = getName();

      if (!arguments.length) {
        return false;
      }

      return true;
    }

    // good
    function() {
      if (!arguments.length) {
        return false;
      }

      var name = getName();

      return true;
    }
    ```

**[⬆ back to top](#table-of-contents)**



## Conditional Expressions & Equality

  - Use `===` and `!==` over `==` and `!=`.
  - Conditional expressions are evaluated using coercion with the `ToBoolean` method and always follow these simple rules:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0]) {
      // true
      // An array is an object, objects evaluate to true
    }
    ```

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

  - For more information see [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

**[⬆ back to top](#table-of-contents)**


## Blocks

  - Use braces with all multi-line blocks.

    ```javascript
    // bad
    if (test)
      return false;

    // bad
    function() { return false; }

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // good
    function() {
      return false;
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Comments

  - Use `/** ... */` to document public function definitions. Include a description, specify types and values for all parameters and return values.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
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

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      var type = this._type || 'no type';

      return type;
    }
    ```

  - Use `//` for multiline comments when a single line comment is longer than 80 charachters. Or use `/* .. */`

    ```javascript
    // bad
    // this method will return a fantabulous variable that you can use to do lots of calculations with. If you don't know how to use it see the docs at http://someFakeSite.com
    var fantabulousValue = fantabulous();

    // good
    // this method will return a fantabulous variable that you can use to do
    // lots of calculations with. If you don't know how to use it see the docs // at http://someFakeSite.com
    var fantabulousValue = fantabulous();

    // good
    /*
     * this method will return a fantabulous variable that you can use to do
     * lots of calculations with. If you don't know how to use it see the docs
     * at http://someFakeSite.com
     */
    var fantabulousValue = fantabulous();
    ```

  - Use `// FIXME:` to annotate problems.

    ```javascript
    function Calculator() {

      // FIXME: shouldn't use a global here
      total = 0;

      return this;
    }
    ```

  - Use `// TODO:` to annotate solutions to problems.

    ```javascript
    function Calculator() {

      // TODO: total should be configurable by an options param
      this.total = 0;

      return this;
    }
  ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  - Use soft tabs set to 2 spaces.

    ```javascript
    // bad
    function() {
    ∙∙∙∙var name;
    }

    // bad
    function() {
    ∙var name;
    }

    // good
    function() {
    ∙∙var name;
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

  - Set off operators with spaces.

    ```javascript
    // bad
    var x=y+5;

    // good
    var x = y + 5;
    ```

  - End files with a single newline character.

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - Use indentation when making long method chains. Use a leading dot, which
    emphasizes that the line is a method call, not a new statement.

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    var leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    var leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .class('led', true)
        .attr('width',  (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - Leave a blank line after blocks and before the next statement

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    var obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    var obj = {
      foo: function() {
      },

      bar: function() {
      }
    };

    return obj;
    ```

 - Magic Numbers and Fancy Stuff should be commented. Magic Numbers are constant values used for calculations. Fancy Stuff are things out of the norm done with good reason.

    ```javascript
    // Magic Numbers

    // +10 to move over from border
    element.style.left = left + 10 + 'px';


    // Fancy Stuff

    // parseInt was the reason my code was slow.
    // Bitshifting the String to coerce it to a
    // Number made it a lot faster.
    var val = '10' >> 0;
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    var hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    var hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode.

    ```javascript
    // bad
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    var heroes = [
      'Batman',
      'Superman',
    ];

    // good
    var hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    var heroes = [
      'Batman',
      'Superman'
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  - **Yup.**

    ```javascript
    // bad
    (function() {
      var name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      var name = 'Skywalker';
      return name;
    })();
    ```

    [Read more](http://stackoverflow.com/a/7365214/1712802).

**[⬆ back to top](#table-of-contents)**



## Naming Conventions

  - Avoid single letter names and abreviations. Be descriptive with your naming.

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }

    // bad
    var perc = 0;

    // good
    var percentage = 0;
    ```

  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    var OBJEcttsssss = {};
    var this_is_my_object = {};
    function c() {}
    var u = new user({
      name: 'Bob Parr'
    });

    // good
    var thisIsMyObject = {};
    function thisIsMyFunction() {}
    var user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming constructors or classes.

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    var bad = new user({
      name: 'nope'
    });

    // good
    function User(options) {
      this.name = options.name;
    }

    var good = new User({
      name: 'yup'
    });
    ```

  - Use a leading underscore `_` when naming private properties.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // good
    this._firstName = 'Panda';
    ```

  - Use a leading underscore `_` when naming private methods. Private methods should exist outside of the scope of object and should be invoked using the call method.

  ```javascript
  var dog = {
    _name: 'Spot',

    bark: function(sound) {
      _bark.call( this, sound );
    }
  };

  function _bark(sound) {
    console.log(this._name + ':' + sound);
  }
  ```

  - `Boolean` variables should prefixed with is or has.

    ```javascript
    // bad
    var cat = false;
    var eggs = true;

    // good
    var isCat = false;
    var hasEggs = true;
    ```

  - When saving a reference to `this` use `_this`.

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

  - Use a leading dollar `$` when naming variables or properties that refer to a DOM Element

    ```javascript
    // bad
    var el = document.querySelector('#my-el');

    // good
    var $el = document.querySelector('#my-el');
    ```

**[⬆ back to top](#table-of-contents)**



## Constructors

  - Assign methods to the prototype object, instead of overwriting the prototype with a new object. Overwriting the prototype makes inheritance impossible: by resetting the prototype you'll overwrite the base!

    ```javascript
    function Jedi() {
      console.log('new jedi');
    }

    // bad
    Jedi.prototype = {
      fight: function fight() {
        console.log('fighting');
      },

      block: function block() {
        console.log('blocking');
      }
    };

    // good
    Jedi.prototype.fight = function fight() {
      console.log('fighting');
    };

    Jedi.prototype.block = function block() {
      console.log('blocking');
    };
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // bad
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    var luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return this;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
      return this;
    };

    var luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    function Jedi(options) {
      options || (options = {});
      this.name = options.name || 'no name';
    }

    Jedi.prototype.getName = function getName() {
      return this.name;
    };

    Jedi.prototype.toString = function toString() {
      return 'Jedi - ' + this.getName();
    };
    ```

**[⬆ back to top](#table-of-contents)**



## Modules

  - Modules should be built using the CommonJS pattern

    ```javascript
    // Creating a module in ./doSomething.js
    module.exports = function doSomething() {

      console.log('BOO YA!');
    };
    ```

    ```javascript
    // Consuming module from ./doSomething.js in another file
    var doSomething = require('./doSomething');

    doSomething();
    ```
  - Resuable modules should be distributed on NPM

**[⬆ back to top](#table-of-contents)**


## jQuery

  - You should shy away from using jQuery. Instead use modules off NPM like:
    + `dom-select`
    + `dom-style`
    + `dom-tree`
    + `dom-event`
  - Visit [https://github.com/npm-dom](https://github.com/npm-dom) for more info.
  - Or search an equivalent of the function you need in [You Might Not Need jQuery](http://youmightnotneedjquery.com/)

**[⬆ back to top](#table-of-contents)**


## Tools

  - [eslint](http://eslint.org/) shows you when you don't respect the convention. If you are using Sublime Text, you can install [SublimeLinter-eslint](https://github.com/roadhump/SublimeLinter-eslint) in addition of [SublimeLinter](http://sublimelinter.readthedocs.org/en/latest/).
  - [esformatter](https://github.com/millermedeiros/esformatter) will formate your JavaScript file to fit the coding style. If you are using Sublime Text, you can install [Sublime EsFormatter](https://github.com/piuccio/sublime-esformatter) and do it directly in your IDE.

**[⬆ back to top](#table-of-contents)**



## License

(The MIT License)

Copyright (c) 2014 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
