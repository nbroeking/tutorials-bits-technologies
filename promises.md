[Promises](https://developers.google.com/web/fundamentals/getting-started/primers/promises) 
---

A __Promise__ encapsulates an asynchronous operation. A Promise object has 3 states; `pending`, `rejected`, or `resolved`.

Create a __Promise__
``` javascript
function writeFile(file, data, options) {
  return new Promise((resolve, reject) => {
    fs.writeFile(file, data, options, (err) => {
      if (err) {
        reject(err);
      } else {
        resolve();
      }
    });
  });
}

function writeFileFoo() {
  const file = '/tmp/foo.txt';
  return writeFile(file, 'bar');
}
```

Ignore error
``` javascript
function writeFileFooIgnoreError() {
  const file = '/tmp/foo.txt';
  return writeFile(file, 'bar')
  .catch(() => null);
}
```

Ignore specific error
``` javascript
function makeDirectoryFooIgnoreExist() {
  const dirpath = '/tmp/foo';
  return mkdir(dirpath)
  .catch((err) => {
    if (err.code !== 'EEXIST') {
      return Promise.reject(err);
    }
  });
}
```

Turn value into a __Promise__
``` javascript
function getValueFoo() {
  return Promise.resolve('foo');
}
```

Run multiple operations in parallel
``` javascript
function delay(timeout) {
  return new Promise((resolve) => setTimeout(resolve, timeout));
}

function runDelaysParallel() {
  return Promise.all([
    delay(5),
    delay(10)
  ])
  .then(() => {
    console.log('This is printed after ~10ms.');
  });
}
```

Run multiple operations in serial
``` javascript
function runDelaysSerial() {
  return Promise.resolve()
  .then(() => delay(5))
  .then(() => delay(10))
  .then(() => {
    console.log('This is printed after ~15ms.');
  });
}
```

Run multiple operations in parallel from an array
``` javascript
function runDelaysParallel() {
  const timeouts = [5, 10, 20, 15];
  return Promise.all(timeouts.map((timeout) => delay(timeout))
  .then(() => {
    console.log('This is printed after ~20ms.');
  });
}
```

Create chain of promises from an array
``` javascript
function runDelaysSerial() {
  const timeouts = [5, 10, 20, 15];
  return timeouts.reduce((chain, timeout) => {
    return chain.then(() => delay(timeout);
  }), Promise.resolve())
  .then(() => {
    console.log('This is printed after ~50ms.');
  });
}
```