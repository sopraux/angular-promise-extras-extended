# angular-promise-extras-extended

This is a fork of [angular-promise-extras](https://github.com/ohjames/angular-promise-extras) in order to improve semantic with isFulfilled and isRejected

[![build status](https://travis-ci.org/sopraux/angular-promise-extras-extended.svg?branch=master)](https://travis-ci.org/sopraux/angular-promise-extras-extended.svg?branch=masters)

## Installation

### WebPack

1. Install the package
    ```
    npm install --save angular-promise-extras-extended
    ```

2. `require`/`import` `angular-promise-extras`.
3. Add the angular module `ngPromiseExtras` as a dependency of your application module.

## Usage

```javascript
var deferreds = [ $q.defer(), $q.defer() ]
var asyncVals = deferreds.map(function(deferred) {
  return deferred.promise
})
asyncVals.push(3)

$q.allSettled(asyncVals).then(function(values) {
  expect(values).toEqual([
    { state: 'fulfilled', value: 1, isFulfilled: true, isRejected: false },
    { state: 'rejected', reason: 2, isFulfilled: false, isRejected: true },
    { state: 'fulfilled', value: 3, isFulfilled: true, isRejected: false },
  ])
})

deferreds[0].resolve(1)
deferreds[1].reject(2)
```

Also works with objects:

```javascript
var deferreds = [ $q.defer(), $q.defer() ]
var promisesArray = deferreds.map(function(deferred) {
  return deferred.promise
})
var promises = { a: promisesArray[0], b: promisesArray[1], c: 3  }

$q.allSettled(promises).then(function(values) {
  expect(values).toEqual({
    a: { state: 'fulfilled', value: 1, isFulfilled: true, isRejected: false },
    b: { state: 'rejected', reason: 2, isFulfilled: false, isRejected: true },
    c: { state: 'fulfilled', value: 3, isFulfilled: true, isRejected: false },
  })
})

deferreds[0].resolve(1)
deferreds[1].reject(2)
```

Also provides
  * `$q.map`: works like `Bluebird.map` or `Bluebird.props` depending on whether an array or an object is passed.
  * `$q.mapSettled`: Works like `$q.map` but with the settled semantics.
  * `$q.resolve`: Works like `Bluebird.resolve`.
