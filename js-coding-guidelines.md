The following document outlines a reasonable style guide for JS development in all codebases. These guidelines strongly encourage the use of existing, common, sensible patterns.

This is a living document and new ideas are always welcome. Please contribute.


The JS Coding Guidelines are divides classes in to several categories:


- [Whitespace](#whitespace)
- [Beautiful Syntax](#beautiful-syntax)
- [Type Checking](#type-checking)
- [Conditional Evaluation](#conditional-evaluation)
- [Practical Style](#practical-style)
- [Naming](#naming)
- [Misc](#misc)

---

## Whitespace
- Never mix spaces and tabs.
- Use soft tabs with two spaces — they're the only way to guarantee code renders the same in any environment.
  - If using Sublime Text you can do this by hitting `Cmd + ,`.  In the tab that opens put the following code between the 2 `{ }` so it looks like this:

    ```json
    {
      "tab_size": 2,
      "translate_tabs_to_spaces": true
    }
    ```

---

## Beautiful Syntax
### Parens, Braces, Linebreaks

*(if/else/for/while/try always have spaces, braces and span multiple lines this encourages readability)*



*BAD*
```js
// Examples of really cramped syntax
if(condition) doSomething();

while(condition) iterating++;

for(var i=0;<100;i++) someIterativeFn();
```

*GOOD*
```js
// Use whitespace to promote readability

if ( condition ) {
  // statements
}

while ( condition ) {
  // statements
}

for ( var i = 0; i < 100; i++ ) {
  // statements
}
```

*BETTER*
```js
var i,
    length = 100;

for ( i = 0; i < length; i++ ) {
  // statements
}

// Or...
var i = 0,
    length = 100;

for ( ; i < length; i++ ) {
  // statements
}

var prop;

for ( prop in object ) {
  // statements
}

if ( true ) {
  // statements
} else {
  // statements
}
```

### Assignments, Declarations, Functions ( Named, Expression, Constructor )
#### Variables
```js
var foo = 'bar',
    num = 1,
    undef;
```

#### Literal notations
```js
var array = [],
    object = {};
```


##### **Using only one `var` per scope (function) promotes readability and keeps your declaration list free of clutter (also saves a few keystrokes)**

*BAD*
```js
var foo = '';
var bar = '';
var qux;
```

*GOOD*
```js
var foo = '',
    bar = '',
    qux;
```

*OR*
```js
var // Comment on these
foo = '',
bar = '',
quux;
```


##### **var statements should always be in the beginning of their respective scope (function).**

*BAD*
```js
function foo() {
  // some statements here

  var bar = '',
      qux;
}
```

*GOOD*
```js
function foo() {
  var bar = '',
      qux;

  // all statements after the variables declarations.
}
```


##### **const and let, from ECMAScript 6, should likewise be at the top of their scope (block).**

*BAD*
```js
function foo() {
  let foo,
      bar;

  if ( condition ) {
    bar = '';

    // statements
  }
}
```

*GOOD*
```js
function foo() {
  let foo;

  if ( condition ) {
    let bar = '';

    // statements
  }
}
```

#### Named Function Declaration
```js
function foo( arg1, argN ) {
}

// Usage
foo( arg1, argN );


function square( number ) {
  return number * number;
}

// Usage
square( 10 );
```

#### Really contrived continuation passing style
```js
function square( number, callback ) {
  callback( number * number );
}

// Usage
square( 10, function( square ) {
  // callback statements
});
```

#### Function Expression
```js
var square = function( number ) {
  // Return something valuable and relevant
  return number * number;
};
```

#### Function Expression with Identifier
*This preferred form has the added value of being able to call itself and have an identity in stack traces:*
```js
var factorial = function factorial( number ) {
  if ( number < 2 ) {
    return 1;
  }

  return number * factorial( number - 1 );
};
```

#### Constructor Declaration
```
function FooBar( options ) {
  this.options = options;
}

// Usage
var fooBar = newFooBar({
  a: 'alpha'
});

fooBar.options;
  // {
    a: 'alpha'
  }
```

### Exceptions, Slight Deviations
#### Functions with callbacks
```js
foo(function() {
  // Note there is no extra space between the first paren
  // of the executing function call and the word 'function'
});

// Function accepting an array, no space
foo([
  'alpha',
  'beta'
]);
```

#### Function accepting an object, no space
```js
foo({
  a: 'alpha',
  b: 'beta'
});

foo( 'bar' );

if ( !( 'foo' in obj ) ) {
}
```

#### Consistency Always Wins
The whitespace rules are set forth as a recommendation with a simpler, higher purpose: **consistency**. It's important to note that formatting preferences, such as 'inner whitespace' should be considered optional, but only one style should exist across the entire source of your project.
```js
if ( condition ) {
  // statements
}

while ( condition ) {
  // statements
}

for ( var i = 0; i < 100; i++ ) {
  // statements
}

if ( true ) {
  // statements
} else {
  // statements
}
```

### Quotes
Uuse single quotes unless we are using them in HTML properties.
```js
$('<div class="class"></div>');
```

---

## Type Checking
*(Courtesy jQuery Core Style Guidelines)*

### Actual Types

#### String:
`typeof variable === 'string'`

#### Number:
`typeof variable === 'number'`

#### Boolean:
`typeof variable === 'boolean'`

#### Object:
`typeof variable === 'object'`

#### Array:
`Array.isArray( arrayLikeObject ) (wherever possible)`

#### Node:
`elem.nodeType === 1`

#### null:
`variable === null`

#### null or undefined:
`variable == null`

#### undefined:
##### Global Variables:
`typeof variable === 'undefined'`

##### Local Variables:
`variable === undefined`

##### Properties:
`object.prop === undefinedobject.hasOwnProperty( prop )'prop' in object`

### Coerced Types
*Consider the implications of the following...*

Given this HTML: `<input id="foo-input" type="text" value="1">`


`foo` has been declared with the value `0` and it's type is `number`
```js
var foo = 0;

// typeof foo;
// 'number'
...
```

Somewhere later in your code, you need to update `foo` with a new value derived from an input element
```js
foo = document.getElementById('foo-input').value;
```

If you were to test `typeof foo` now, the result would be `string`

This means that if you had logic that tested `foo` like:
```js
if ( foo === 1 ) {
  importantTask();
}
```

`importantTask()` would never be evaluated, even though `foo` has a value of `'1'`

You can preempt issues by using smart coercion with unary `+` or `-` operators:
```js
foo =+ document.getElementById('foo-input').value;
// ^ unary `+` operator will convert its right side operand to a number

// typeof foo;
// 'number'

if ( foo === 1 ) {
  importantTask();
}
```

`importantTask()` will be called

Here are some common cases along with coercions:
```
var number = 1,
    string = '1',
    bool = false;

    number;
    // 1

    number + '';
    // '1'

    string;
    // '1'

    +string;
    // 1

    +string++;
    // 1

    string;
    // 2

    bool;
    // false

    +bool;
    // 0

    bool + '';
    // 'false'
```

```
var number = 1,
    string = '1',
    bool = true;

string === number;
// false

string === number + '';
// true

+string === number;
// true

bool === number;
// false

+bool === number;
// true

bool === string;
// false

bool === !!string;
// true

var array = [ 'a', 'b', 'c' ];

!!~array.indexOf('a');
// true

!!~array.indexOf('b');
// true

!!~array.indexOf('c');
// true

!!~array.indexOf('d');
// false
```

*Note that the above should be considered 'unnecessarily clever'*
*Prefer the obvious approach of comparing the returned value of indexOf, like:*
```js
if ( array.indexOf( 'a' ) >= 0 ) {
  // ...
}
```

```js
var num = 2.5;

parseInt( num, 10 );

// is the same as...
~~num;
num >> 0;
num >>> 0;

// All result in 2
// Keep in mind however, that negative numbers will be treated differently...
var neg = -2.5;

parseInt( neg, 10 );

// is the same as...
~~neg;
neg >> 0;

// All result in -2
// However...
neg >>> 0;

// Will result in 4294967294
```

---

## Conditional Evaluation
#### When only evaluating that an array has length, instead of this:

*BAD*
```js
if ( array.length > 0 ) ...
```

*GOOD*
```js
if ( array.length ) ...
```

#### When only evaluating that an array is empty, instead of this:


*BAD*
```js
if ( array.length === 0 ) ...
```

*GOOD*
```js
if ( !array.length ) ...
```

#### When only evaluating that a string is not empty, instead of this:

*BAD*
```js
if ( string !== '' ) ...
```

*GOOD*
```js
if ( string ) ...
```

#### When only evaluating that a string _is_ empty, instead of this:

*BAD*
```js
if ( string === '' ) ...
```

*GOOD*
```js
if ( !string ) ...
```

#### When only evaluating that a reference is true, instead of this:

*BAD*
```js
if ( foo === true ) ...
```

*GOOD*
```js
if ( foo ) ...
```

#### When evaluating that a reference is false, instead of this:

*BAD*
```js
if ( foo === false ) ...
```

*GOOD*
```js
if ( !foo ) ...
```

*...Be careful, this will also match: `0, '', null, undefined, NaN`*

#### If you _MUST_ test for a boolean false, then use
```js
if ( foo === false ) ...
```

#### When only evaluating a ref that might be `null` or `undefined`, but NOT `false, '' or 0`, instead of this:

*BAD*
```js
if ( foo === null || foo === undefined ) ...
```

*GOOD*
```js
if ( foo == null ) ...
```

*Remember, using `==` will match a `null` to BOTH `null` and `undefined` but not `false`, `'' or 0 null == undefined`*

*ALWAYS evaluate for the best, most accurate result - the above is a guideline, not a dogma.*

#### Type coercion and evaluation notes

Prefer `===` over `==` (unless the case requires loose type evaluation)

```js
// === does not coerce type, which means that:
'1' === 1;
// false
```

```js
// == does coerce type, which means that:
'1' == 1;
// true
```

#### Booleans, Truthies & Falsies
##### Booleans:
`true, false`

##### Truthy
`'foo', 1`

##### Falsy:
`'', 0, null, undefined, NaN, void, 0`

---

## Practical Style
### A Practical Module
```js
(function( global ) {
  var Module = (function() {
    var data = 'secret';

    return {
      // This is some boolean property
      bool: true,
      // Some string value
      string: 'a string',
      // An array property
      array: [
        1,
        2,
        3,
        4
      ],
      // An object property
      object: {
        lang: 'en-Us'
      },
      getData:function() {
        // get the current value of `data`
        return data;
      },
      setData: function( value ) {
        // set the value of `data` and return it
        return ( data = value );
      }
    };
  })();

  // Other things might happen here
  // expose our module to the global object
  global.Module = Module;
})( this );
```

### A Practical Constructor
```js
(function( global ) {
  function Ctor( foo ) {
    this.foo = foo;
    return this;
  }

  Ctor.prototype.getFoo = function() {
    return this.foo;
  };

  Ctor.prototype.setFoo = function( val ) {
    return ( this.foo = val );
  };

  // To call constructor's without `new`, you might do this:
  var ctor = function( foo ) {
    return newCtor( foo );
  };

  // expose our constructor to the global object
  global.ctor = ctor;
})( this );
```

---

## Naming

### Variable Naming
`const` declarations used as variables are in all caps with `_` between each word
```js
const SOMETHING_THAT_IS_CONSTANT = 'I will never change.';
```

`var` and `let` declarations use camel case with a lowercase first letter:
```js
let somethingNew = 3;
var anotherNewThing = 8;
```

Objects are declared using camel case but with the first letter capitalized:
```js
let MilleniumFalcon = new Object();
```

All modules within the Lexicon will use the [Definitive Module Pattern](https://github.com/tim-montague/Definitive-Module-Pattern). Within these modules, functions are grouped in to `_public` and `_private` objects which are `const` variables that do not follow the above conventions.

The `_private` object stores methods that should not be publicly accessible. The `_public` object stores methods that are publicly accessible.
```js
let myDefinitiveModule = (function () {

  const _private = {
    cacheDom: () => {
      this.$something = $( '.something' );
    },

    bindEvents: () => {
      this.$something.on( 'click', function( e ) {
        alert( 'You clicked on something' );
      });
    }
  };

  const _public = {
    init: () => {
      _private.cacheDom();
      _private.bindEvents();
    }
  };

  return _public;
})();
```

### Human Readable Naming
*You are not a human code compiler/compressor, so don't try to be one.*


The following code is an example of egregious naming:

*BAD*
```js
function q( s ) {
  return document.querySelectorAll(s);
}

var i,a=[],els=q('#foo');for(i=0;i<els.length;i++){a.push(els[i]);}
```

*GOOD*
```js
function query( selector ) {
  return document.querySelectorAll( selector );
}

var idx = 0,
    elements = [],
    matches = query('#foo'),
    length = matches.length;

for ( ; idx < length; idx++ ) {
  elements.push( matches[ idx ] );
}
```

### A few additional naming pointers:
#### Naming strings
`'dog' is a string`

#### Naming arrays
`'dogs' is an array of 'dog' strings`

#### Naming functions, objects, instances, etc
`camelCase; function and var declarations`

#### Naming constructors, prototypes, etc.
`PascalCase; constructor function`

#### Naming regular expressions
`rDesc = //;`

##### From the Google Closure Library Style Guide
```js
functionNamesLikeThis;
variableNamesLikeThis;
ConstructorNamesLikeThis;
EnumNamesLikeThis;
methodNamesLikeThis;
SYMBOLIC_CONSTANTS_LIKE_THIS;
```

### Faces of `this`
  Beyond the generally well known use cases of `call` and `apply`, always prefer `.bind( this )` or a functional equivalent, for creating `BoundFunction` definitions for later invocation. Only resort to aliasing when no preferable option is available.
  ```
  function Device( opts ) {
    this.value = null;

    // open an async stream,
    // this will be called continuously
    stream.read( opts.path, function( data ) {
      // Update this instance's current value
      // with the most recent value from the
      // data stream
      this.value = data;
    }.bind(this) );

    // Throttle the frequency of events emitted from
    // this Device instance
    setInterval(function() {
      // Emit a throttled event
      this.emit('event');
    }.bind(this), opts.freq || 100 );
  }
  // Just pretend we've inherited EventEmitter ;)
  ```

  When unavailable, functional equivalents to `.bind` exist in many modern JavaScript libraries.
  ```
  // eg. lodash/underscore, _.bind()
  function Device( opts ) {
    this.value = null;
    stream.read( opts.path, _.bind(function( data ) {
      this.value = data;
    }, this) );

    setInterval(_.bind(function() {
      this.emit('event');
    }, this), opts.freq || 100 );
  }

  // eg. jQuery.proxy
  function Device( opts ) {
    this.value = null;
    stream.read( opts.path, jQuery.proxy(function( data ) {
      this.value = data;
    }, this) );

    setInterval( jQuery.proxy(function() {
      this.emit('event');
    }, this), opts.freq || 100 );
  }
  ```

  As a last resort, create an alias to this using self as an Identifier. This is extremely bug prone and should be avoided whenever possible.
  ```
  function Device( opts ) {
    var self = this;

    this.value = null;

    stream.read( opts.path, function( data ) {
      self.value = data;
    });

    setInterval(function() {
      self.emit('event');
    }, opts.freq || 100 );
  }
  ```

### Use `thisArg`
Several prototype methods of ES 5.1 built-ins come with a special `thisArg` signature, which should be used whenever possible
```
var obj;
obj = {
  f: 'foo',
  b: 'bar',
  q: 'qux'
};

Object.keys( obj ).forEach(function( key ) {
  // |this| now refers to `obj`
  console.log( this[ key ] );
}, obj ); // <-- the last arg is `thisArg`

// Prints...
// 'foo'
// 'bar'
// 'qux'
```

`thisArg` can be used with `Array.prototype.every`, `Array.prototype.forEach`, `Array.prototype.some`, `Array.prototype.map`, `Array.prototype.filter`

---

## Misc
This section will serve to illustrate ideas and concepts that should not be considered dogma, but instead exists to encourage questioning practices in an attempt to find better ways to do common JavaScript programming tasks.


### Early returns promote code readability with negligible performance difference
*BAD*
```js
function returnLate( foo ) {
  var ret;

  if ( foo ) {
    ret = 'foo';
  } else {
    ret = 'quux';
  }

  return ret;
}
```

*GOOD*
```js
function returnEarly( foo ) {
  if ( foo ) {
    return 'foo';
  }

  return 'quux';
}
```
