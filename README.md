# Explicit closure return pattern for JavaScript

## Authors
* David Rhys White (@davidrhyswhite)

## Overview and motivation

There are many times within a JavaScript application you want to pass a value into a function but not execute the entire function, partially applied functions. The pattern is becoming more popular as JavaScript is taking a more functional approach:

```javascript
function negate(isTrue, x) {
  return !func(x);
}
negate(isTrue, value);
```

We could write it more elegantly and make it a little more reusable with a partially applied function:

```javascript
function negate(func) {
  return function (x) {
    return !func(x);
  };
}
const isNotTrue = negate(isItTrue);

isNotTrue(value);
```

If all your doing is passing literals down the chain without the need to run any additional calculations on those, other than to be available at a later date with you have all the parameters, then I would like to propose a new and possibly unfamiliar syntax to make this more convenient.


## Syntax

My proposal would be to use a `()(){}` pattern within function declarations, for example:

```javascript
function negate(isTrue)(x) {
  return !func(x);
}
const isNotTrue = negate(isItTrue);

isNotTrue(value);
```

This could be extended to allow multiple returns:

```javascript
function positionCheck(destination)(long)(lat) {
  return destination(long, lat);
}
```

The current native implementation would of course look something more along the lines of:

```javascript
function positionCheck(destination) {
  return function (long) {
    return function (lat) {
      return destination(long, lat);
    }
  }
}
```


### Anonymous functions

This could work with anonymous function also:

```javascript
function positionCheck(destination) {
  return function (long)(lat) {
    return destination(long), lat);
  }
}
```
### Classes / constructors

### Arrow functions

### Object literals

```javascript
const locator = {
  distance(latA, longA)(latB, longB) {
    return calculateDistance(latA, longA, latB, longB);
  }
};

const distanceToDestination = locator.distance(3.14, 3.3);

distanceToDestination(3.12, 3.2); // Close to arrival
distanceToDestination(3.14, 3.3); // Arrived
```

### Notes

## External References
