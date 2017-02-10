# General information

Basically there are two ways how to implement inheritance in javascript:
* Classical
* Prototypes

ECMAScrip6 added extra functionality to JS and now it is possible to use classes and inheritance like in other OOP languages (aka Python or Java)

General information about all these three types of inheritance with simple examples could be found below.

## Classical inheritance

Classical inheritance (or functional inheritance) is based on functions, this variable and new keyword.
Here is the easiest example of classical inheritance:

```javascript
function Foo(val1) {
  this.foo = val1;
}

Foo.prototype.get_foo = function() {
  return this.foo;
}

function Bar(val1, val2) {
  Foo.call(this, val1);
  this.bar = val2;
}
Bar.prototype = Object.create(Foo.prototype);

Bar.prototype.get_bar = function() {
  return this.bar;
}

var bar = new Bar('foo', 'bar');

console.log(bar.get_foo()); // foo

```

There are a lot of ways how to improve this code and make it more usable. For example add another method to Function prototype which will take care of all actions required for inheritance implementation:

```javascript
Function.prototype.inherit = function(baseClass, superClass, methods ) {
  baseClass.prototype = Object.create(superClass.prototype);

  methods.forEach( function(element) {
    baseClass.prototype[element.name] = element.fn;
  });
}

function Foo(val1) {
  this.foo = val1;
}

Foo.prototype.get_foo = function() {
  return this.foo;
}

function Bar(val1, val2) {
  Foo.call(this, val1);
  this.bar = val2;
}

Bar.inherit(Bar, Foo, [{ name: 'get_bar', fn: function() { return this.bar } }]);

var bar = new Bar('foo', 'bar');

console.log(bar.get_foo()); // foo

console.log(bar);
Bar
  bar: "bar"
  foo: "foo"
  __proto__: Foo
    get_bar: ()
    __proto__: Object
      constructor: Foo(val1)
      get_foo: ()
      __proto__: Object
```

The most interesting part here is how inherited attributes are stored - like object attributes. More details about it will be described later.

## Prototypal inheritance

Prototypal inheritance is based only on objects. Here is the easiest example of prototypal inheritance:
```javascript

var foo = {
  foo: 'some_text',
  get_foo: function() {
    return this.foo;
  }
}

var bar = Object.create(foo);
bar.bar = 'another text';
bar.get_bar = function() {
  return this.bar;
}

console.log(bar.get_foo()) //some_text

console.log(bar);
Object
  bar: "another text"
  get_bar: function()
  __proto__: Object
    foo: "some_text"
    get_foo: ()
    __proto__: Object
```

More details about how inherited attributes are stored and comparing with classical inheritance will be described in paragraph 2.1.

## ES6 inheritance

Inheritance with ES6 is the simplest one - it could be realized in the same way as in other object oriented languages:
```javascript
class Foo {
  constructor(...args) {
    this.foo = 'foo';
  }

  print_foo() {
    console.log(this.foo);
  }
}

class Bar extends Foo {
  constructor(...args) {
    super(...args);
    this.bar = 'bar';
  }

  print_bar() {
    console.log(this.bar);
  }
}

let bar = new Bar();
```

In case of transformation of this code to ES5 will get something like this:
```javascript
var _createClass = function() {
  function defineProperties(target, props) {
    for (var i = 0; i < props.length; i++) {
      var descriptor = props[i];
      descriptor.enumerable = descriptor.enumerable || false;
      descriptor.configurable = true;
      if ("value" in descriptor) descriptor.writable = true;
      Object.defineProperty(target, descriptor.key, descriptor);
    }
  }
  return function(Constructor, protoProps, staticProps) {
    if (protoProps) defineProperties(Constructor.prototype, protoProps);
    if (staticProps) defineProperties(Constructor, staticProps);
    return Constructor;
  };
}();

function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function, not " + typeof superClass);
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: {
      value: subClass,
      enumerable: false,
      writable: true,
      configurable: true
    }
  });
  if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass;
}
var Foo = function() {
  function Foo() {
    _classCallCheck(this, Foo);

    this.foo = 'foo';
  }

  _createClass(Foo, [{
    key: 'print_foo',
    value: function print_foo() {
      console.log(this.foo);
    }
  }]);

  return Foo;
}();

var Bar = function(_Foo) {
  _inherits(Bar, _Foo);

  function Bar() {
    var _ref;

    _classCallCheck(this, Bar);

    for (var _len = arguments.length, args = Array(_len), _key = 0; _key < _len; _key++) {
      args[_key] = arguments[_key];
    }

    var _this = _possibleConstructorReturn(this, (_ref = Bar.__proto__ || Object.getPrototypeOf(Bar)).call.apply(_ref, [this].concat(args)));

    _this.bar = 'bar';
    return _this;
  }

  _createClass(Bar, [{
    key: 'print_bar',
    value: function print_bar() {
      console.log(this.bar);
    }
  }]);

  return Bar;
}(Foo);

var bar = new Bar();
```

If we do this we will get standard classical inheritance with adding some basic functions for improving it.

# Comparing classical and prototypal inheritance

Each method has it's own pros and cons.

Classical inheritance is the easiest solution for everyone who came from static languages and it has standard way of realization.
However it has a lot of disadvantages:
* All methods should be added to the object prototype, if not - they will be copied all the time when child object is created. It will cause an extra memory.
* All parents attributes always copies to the child. In this case child uses extra memory for each parent attributes
* It's static - when parent object changes his attribute it is not reflecting to his children
* Functions for describing objects couldn't be used as functions, only with new keyword for creating new objects, in other cases it will return undefined and could cause to future hidden errors in program.

Prototypal inheritance has much more benefits in comparing with classical one:
* It's flexible - it is easily to manipulate with proto options and selecting required attributes. Also when parent changes his attribute it immediately reflects on all his child, unless they overrode them
* Uses less memory - unless child object uses default value from parent - it does not create a copy of it (just uses variable from proto). If child reassign value to parents variable JS will create separate variable with the same name as child property
* Objects could be used as standard objects, without extra calling of new or any additional methods
* It's possible to implement multi inheritance (through copying all parents objects properties to child proto object)

However prototypal inheritance also has disadvantages:
* Hard to understand for new people
* Require some preparation (writing extra functionality for inheriting and copying all object properties and methods)
* No standard way of inheritance realization and object creation (depends on the person)

Basic disadvantage for all types of inheritance in JavaScript is performance. It is quite critical for situation with long prototype chain - could take a lot of time for going through all child prototypes.
