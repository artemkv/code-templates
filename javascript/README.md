## "Hello World"

```js
console.log('Hello, World!');
```

## String interpolation

```js
let city = 'Barcelona';
let cityFormatted = `City: ${city}`; // 'City: Barcelona'
```

## Bind a function to a receiver

```js
let obj = {
    name: 'John',
    greet: function () {
        console.log(`Hi, I am ${ this.name } `);
    }
};

let g = obj.greet.bind(obj);
g();
```

## Make a library accessible in global space

```js
(function (window) {
  mylib = {
    foo: function () {
      console.log('foo');
    },
  };
  window['mylib'] = mylib;
})(window);

window['mylib'].foo();
```

## Immediately invoked function expression (IIFE) + closure

```js
var getNewId = (function () {
  // Last generated id.
  let _lastId = 0;
  return function () {
    _lastId++;
    return _lastId;
  };
})();

console.log('New id: ' + getNewId());
```

## Incapsulation without classes (Classic module pattern)

```js
let counter = (function () {
  let _count = 0;
  let counter = {
    inc: function () {
      _count++;
      return _count;
    },
    dec: function () {
      _count--;
      return _count;
    },
    cur: function () {
      return _count;
    },
  };
  return counter;
})();
```

## Storing methods on prototypes

```js
Person.prototype.greet = function () {
    console.log(`Hi, I am ${this.name}`);
}

function Person(name) {
    // make new-agnostic
    var self = this instanceof Person
        ? this
        : Object.create(Person.prototype);

    self.name = name;
    return self;
}

let p = new Person('John');
p.greet();
```