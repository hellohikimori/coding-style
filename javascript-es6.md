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
    let item = new Object();

    // good
    let item = {};
    ```

  - Don't use [reserved words](https://people.mozilla.org/~jorendorff/es6-draft.html#sec-reserved-words) as keys.

    ```javascript
    // bad
    let superman = {
      default: { clark: 'kent' },
      private: true
    };

    // good
    let superman = {
      defaults: { clark: 'kent' },
      hidden: true
    };
    ```

  - Use readable synonyms in place of reserved words.

    ```javascript
    // bad
    let superman = {
      class: 'alien'
    };

    // bad
    let superman = {
      klass: 'alien'
    };

    // good
    let superman = {
      type: 'alien'
    };
    ```

**[⬆ back to top](#table-of-contents)**

## Arrays

  - Use the literal syntax for array creation.

    ```javascript
    // bad
    let items = new Array();

    // good
    let items = [];
    ```

  - If you don't know array length use Array#push.

    ```javascript
    let someStack = [];


    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  - When you need to copy an array use Array#slice. [jsPerf](http://jsperf.com/converting-arguments-to-an-array/7)

    ```javascript
    let len = items.length;
    let itemsCopy = [];
    let i;

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
      let args = Array.prototype.slice.call(arguments);
      ...
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Strings

  - Use single quotes `''` for strings.

    ```javascript
    // bad
    let name = "Bob Parr";

    // good
    let name = 'Bob Parr';

    // bad
    let fullName = "Bob " + this.lastName;

    // good
    let fullName = 'Bob ' + this.lastName;
    ```

  - Strings longer than 80 characters should be written across multiple lines using template strings

    ```javascript
    // bad
    let errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // good
    let errorMessage = `This is a super long error that was thrown because
      of Batman. When you stop to think about how Batman had anything to do
      with this, you would get nowhere fast.`;
    ```

  - When programmatically building up a string, use Array#join instead of string concatenation and a string interpolation.

    ```javascript
    let items;
    let messages;
    let length;
    let i;

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
      let items = [];

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      return '<ul><li>' + items.join('</li><li>') + '</li></ul>';
    }

    // good
    function inbox(messages) {
      let items = [];
      let res;

      for (i = 0; i < length; i++) {
        items[i] = messages[i].message;
      }

      res = items.join('</li><li>');

      return `<ul><li>${res}</li></ul>`;
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
  - Instead of using `arguments`, use a `rest` argument.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // bad
    function nope(name, options, args) {
      // ...stuff...
    }

    // good
    function yup(name, options, ...others) {
      // ...stuff...
    }
    ```

  - EcmaScript 6 let us use default values in arguments. Take an advantage of that!

    ```javascript
    // bad
    function(x, y, z) {
      z = z || 0;
    }

    // good
    function(x, y, z = 0) {
      // ...
    }
    ```

**[⬆ back to top](#table-of-contents)**



## Properties

  - Use dot notation when accessing properties.

    ```javascript
    let luke = {
      jedi: true,
      age: 28
    };

    // bad
    let isJedi = luke['jedi'];

    // good
    let isJedi = luke.jedi;
    ```

  - Use subscript notation `[]` when accessing properties with a variable.

    ```javascript
    let luke = {
      jedi: true,
      age: 28
    };

    function getProp(prop) {
      return luke[prop];
    }

    let isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  - Use `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. Captain Planet warned us of that.
  - Use `const` to declare constant. `const` is single-assignment.

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    let superPower = new SuperPower();

    const MY_CONST = 3;
    // ...
    // error: MY_CONST cant be re-asigned
    MY_CONST = 10
    ```

  - Use one `let` or `const` declaration per variable.
    It's easier to add new variable declarations this way, and you never have
    to worry about swapping out a `;` for a `,` or introducing punctuation-only
    diffs.

    ```javascript
    // bad
    let items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // good
    let items = getItems();
    let goSportsTeam = true;
    let dragonball = 'z';
    ```

  - Declare unassigned variables last. This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    let items = getItems();
    let dragonball;
    let goSportsTeam = true;
    let len;

    // good
    let items = getItems();
    let goSportsTeam = true;
    let dragonball;
    let length;
    let i;
    ```

  - Declare constant at the top of your javascript file.

    ```javascript
    // yup
    const SOME_CONST = 1;

    // ...

    function() {
      // nope
      const OTHER_CONST = 4;
    }

    ```

  - Assign variables at the top of their scope. This helps avoid issues with variable declaration and assignment hoisting related issues.

    ```javascript
    // bad
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      let name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // good
    function() {
      let name = getName();

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
      let name = getName();

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

      let name = getName();

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
    let active = true;  // is current tab

    // good
    // is current tab
    let active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      let type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      let type = this._type || 'no type';

      return type;
    }
    ```

  - Use `//` for multiline comments when a single line comment is longer than 80 charachters. Or use `/* .. */`

    ```javascript
    // bad
    // this method will return a fantabulous variable that you can use to do lots of calculations with. If you don't know how to use it see the docs at http://someFakeSite.com
    let fantabulousValue = fantabulous();

    // good
    // this method will return a fantabulous variable that you can use to do
    // lots of calculations with. If you don't know how to use it see the docs // at http://someFakeSite.com
    let fantabulousValue = fantabulous();

    // good
    /*
     * this method will return a fantabulous variable that you can use to do
     * lots of calculations with. If you don't know how to use it see the docs
     * at http://someFakeSite.com
     */
    let fantabulousValue = fantabulous();
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
    ∙∙∙∙let name;
    }

    // bad
    function() {
    ∙let name;
    }

    // good
    function() {
    ∙∙let name;
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
    let x=y+5;

    // good
    let x = y + 5;
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
    let leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width',  (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    let leds = stage.selectAll('.led')
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
    let obj = {
      foo: function() {
      },
      bar: function() {
      }
    };
    return obj;

    // good
    let obj = {
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
    let val = '10' >> 0;
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

  - Leading commas: **Nope.**

    ```javascript
    // bad
    let hero = {
        firstName: 'Bob'
      , lastName: 'Parr'
      , heroName: 'Mr. Incredible'
      , superPower: 'strength'
    };

    // good
    let hero = {
      firstName: 'Bob',
      lastName: 'Parr',
      heroName: 'Mr. Incredible',
      superPower: 'strength'
    };
    ```

  - Additional trailing comma: **Nope.** This can cause problems with IE6/7 and IE9 if it's in quirksmode.

    ```javascript
    // bad
    let hero = {
      firstName: 'Kevin',
      lastName: 'Flynn',
    };

    let heroes = [
      'Batman',
      'Superman',
    ];

    // good
    let hero = {
      firstName: 'Kevin',
      lastName: 'Flynn'
    };

    let heroes = [
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
      let name = 'Skywalker'
      return name
    })()

    // good
    (function() {
      let name = 'Skywalker';
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
    let perc = 0;

    // good
    let percentage = 0;
    ```

  - Use UPPER_CASE when naming constants.
    ```javascript
    // BAD
    const someCOnstant = 2;

    // GOOD
    const SOME_CONSTANT = 2;
    ```


  - Use camelCase when naming objects, functions, and instances.

    ```javascript
    // bad
    let OBJEcttsssss = {};
    let this_is_my_object = {};
    function c() {}
    let u = new user({
      name: 'Bob Parr'
    });

    // good
    let thisIsMyObject = {};
    function thisIsMyFunction() {}
    let user = new User({
      name: 'Bob Parr'
    });
    ```

  - Use PascalCase when naming classes.

    ```javascript
    // bad
    class user {
      constructor(options) {
        // ...
      }
    }

    let bad = new user({
      name: 'nope'
    });

    // good
    class User {
      constructor(options) {
        // ...
      }
    }

    let good = new User({
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

  - Use a leading underscore `_` when naming private methods.

    ```javascript
    class User {
      constructor(options) {
        // ...
      }

      // public
      getName() {
        // ...
      }

      // private
      _save() {
        // ...
      }
    }
    ```

  - `Boolean` variables should prefixed with is or has.

    ```javascript
    // bad
    let cat = false;
    let eggs = true;

    // good
    let isCat = false;
    let hasEggs = true;
    ```

  - Use a leading dollar `$` when naming variables or properties that refer to a DOM Element

    ```javascript
    // bad
    let el = document.querySelector('#my-el');

    // good
    let $el = document.querySelector('#my-el');
    ```

**[⬆ back to top](#table-of-contents)**



## Constructors

  - Methods can return `this` to help with method chaining.

    ```javascript
    class Jedi() {
      constructor() {
        this.jumping = false;
      }

      jump() {
        this.jumping = true
        return this;
      }

      setLaserSaberColor(color) {
        this.laserSaberColor = color;
        return this;
      }
    }

    let luke = new Jedi();

    luke.jump()
      .setLaserSaberColor('green');
    ```


  - It's okay to write a custom toString() method, just make sure it works successfully and causes no side effects.

    ```javascript
    class Jedi() {
      constructor(name) {
        this.name = name;
      }

      toString() {
        return `Hello, I'am ${this.name} the Jedi`;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**



## Modules

  - A module can export all of its content or some selected stuff
    ```javascript

    // Your module, eg math.js
    export const PI = 3.1415;

    export function sum(a, b) {
      return a + b;
    }

    // export this function if nothing is specified during the import
    export default function default () {

    }

    // In your main file
    // import default()
    import math from 'math.js';

    // ... OR ...
    // import everything
    import * as math from 'math.js';

    // ... OR ...
    // import pi and sum
    import {pi, sum} as math from 'math.js';
    ```

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
