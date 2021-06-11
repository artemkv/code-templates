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

## Immediately invoked function expression (IIFE)

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

## Incapsulation without classes

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
