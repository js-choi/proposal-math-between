# Number-Interval Predicate for JavaScript
ECMAScript Stage-0 Proposal. J. S. Choi, 2021.

## Description

Checking whether one number is between two others is a frequent task in any
programming language. However, JavaScript’s comparison operators only supports
checking the order between two values at a time, resulting in code that looks
like `x <= y && y < z`. A function that allows checking whether `y` is in
between `x` and `z` would be useful for many developers.

We therefore propose exploring the addition of a number-interval predicate to
the JavaScript language. If this proposal is approved for Stage 1, then we
would explore various directions for the API’s design. We would also assemble
as many real-world use cases as possible and shape our design to fulfill them.

## Description
There are several possible ways to do this, all of which would be static
functions on the Math constructor:

We could have a function that *always* uses an inclusive minimum and exclusive
maximum:
```js
// 0 ≤ 2 < 6:
Math.isBetween(2, 0, 6);
```

…or the same except it would always use an inclusive minimum and inclusive
maximum:
```js
// 0 ≤ 2 ≤ 6:
Math.isBetween(2, 0, 6);
```

We could have a function that also takes two boolean arguments indicating their
respective limits’ inclusivity, like [Google Sheets’ `ISBETWEEN`][]:
```js
// 0 ≤ 2 < 6:
Math.isBetween(2, 0, 6, true, false);
// 0 ≤ 2 ≤ 6:
Math.isBetween(2, 0, 6, true, true);
// 0 < 2 < 6:
Math.isBetween(2, 0, 6, false, false);
// 0 < 2 ≤ 6:
Math.isBetween(2, 0, 6, false, true);
```

(The default values of the boolean arguments might be both true, or false and true,
or something else.

[Google Sheets’ `ISBETWEEN`]: https://support.google.com/docs/answer/10538337?hl=en

Or we could have four functions that correspond to the four possible
configurations of inclusivity, forcing the user to be explicit:
```js
// 0 ≤ 2 < 6:
Math.isBetweenIE(2, 0, 6);
// 0 ≤ 2 ≤ 6:
Math.isBetweenII(2, 0, 6);
// 0 < 2 < 6:
Math.isBetweenEE(2, 0, 6);
// 0 < 2 ≤ 6:
Math.isBetweenEI(2, 0, 6);
```

***

We could have a variadic function checking for ascending monotonicity, with
inclusive limits:
```js
// 0 ≤ 2 ≤ 3 ≤ 6:
Math.isIncreasing(0, 2, 3, 6);
```

…or the same, except with exclusive limits:
```js
// 0 < 2 < 3 < 6:
Math.isIncreasing(0, 2, 3, 6);
```

…or with an extra boolean argument for configuration:
```js
// 0 ≤ 2 ≤ 3 ≤ 6:
Math.isIncreasing([ 0, 2, 3, 6 ], true);
// 0 < 2 < 3 < 6:
Math.isIncreasing([ 0, 2, 3, 6 ], false);
```
