## "Hello World"

```js
console.log('Hello, World!');
```

## String interpolation

```js
let city = 'Barcelona';
let cityFormatted = `City: ${city}`; // 'City: Barcelona'
```

## Type checks

```js
let aa = [];
if (Array.isArray(aa)) {
    console.log('is array');
}

function foo() {}
if (typeof foo === 'function') {
    console.log('is function');
}
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

## Promises

```js
const p = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("foo");
    }, 300);
  });

p.then((res) => console.log(res));
```

## Execute at next event cycle

```js
function cb() {
    console.log('executed');
};

// next event cycle (at the end of next tick)
setTimeout(cb, 0);

// NodeJS
// before the beginning of the next tick
// use when you want to make sure that in the next event loop iteration that code is already executed
process.nextTick(cb);

// no longer recommended, being dropped
setImmediate(cb);
```

## Executing long-running operation without blocking the thread

```js
function heavy() {
    let x = 0;
    for (let i = 1; i < 1_000_000_000; i++) {
        x += Math.sqrt(i);
    }
}

function loop(n, m, done) {
    console.log(`Iteration ${n} of ${m}`);
    heavy();
    if (n === m) {
        done();
    } else {
        setTimeout(() => loop(n + 1, m, done), 25);
    }
}

new Promise((resolve, reject) => {
    loop(1, 5, resolve)
}).then(() => { console.log('done'); });
```

## Generator

```js
function* range(n) {
    for (let i = 0; i < n; i++) {
        yield i;
    }
};

for (let x of range(5)) {
    console.log(x);
}
```