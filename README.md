<!--

@license Apache-2.0

Copyright (c) 2018 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->

# Sample

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Sample elements from an array-like object.

<section class="intro">

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import sample from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-sample@esm/index.mjs';
```

You can also import the following named exports from the package:

```javascript
import { factory } from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-sample@esm/index.mjs';
```

#### sample( x\[, options] )

Samples elements from an `array`-like object. By default, elements are drawn with replacement from `x` to create an output `array` having the same length as `x`.

```javascript
var out = sample( [ 'a', 'b', 'c' ] );
// e.g., returns [ 'a', 'a', 'b' ]

out = sample( [ 3, 6, 9 ] );
// e.g., returns [ 3, 9, 6 ]

var bool = ( out.length === 3 );
// returns true
```

The function accepts the following `options`:

-   **size**: sample size. Default: `N = x.length`.
-   **probs**: a probability `array`. Default: `[1/N,...,1/N]`.
-   **replace**: `boolean` indicating whether to sample from `x` with replacement. Default: `true`.

By default, the function returns an `array` having the same length as `x`. To generate a sample of a different size, set the `size` option.

```javascript
var out = sample( [ 3, 6, 9 ], {
    'size': 10
});
// e.g., returns [ 6, 3, 9, 9, 9, 6, 9, 6, 9, 3 ]

out = sample( [ 0, 1 ], {
    'size': 20
});
// e.g., returns [ 0, 0, 0, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0 ]
```

To draw a sample _without_ replacement, set the `replace` option to `false`. In this case, the `size` option cannot be an integer larger than the number of elements in `x`.

```javascript
var out = sample( [ 1, 2, 3, 4, 5, 6 ], {
    'replace': false,
    'size': 3
});
// e.g., returns [ 6, 1, 5 ]

out = sample( [ 0, 1 ], {
    'replace': false
});
// e.g., returns [ 0, 1 ]
```

By default, the probability of sampling an element is the same for all elements. To assign elements different probabilities, set the `probs` option.

```javascript
var x = [ 1, 2, 3, 4, 5, 6 ];
var out = sample( x, {
    'probs': [ 0.1, 0.1, 0.1, 0.1, 0.1, 0.5 ]
});
// e.g., returns [ 5, 6, 6, 5, 6, 4 ]

x = [ 1, 2, 3, 4, 5, 6 ];
out = sample( x, {
    'probs': [ 0.1, 0.1, 0.1, 0.1, 0.1, 0.5 ],
    'size': 3,
    'replace': false
});
// e.g., returns [ 6, 4, 1 ]
```

The `probs` option **must** be a numeric array consisting of nonnegative values which sum to one. When sampling _without_ replacement, note that the `probs` option denotes the initial element probabilities which are then updated after each draw.

#### sample.factory( \[pool, ]\[options] )

Returns a `function` to sample elements from an `array`-like object. 

```javascript
var mysample = sample.factory();

var out = mysample( [ 0, 1, 2, 3, 4 ] );
// e.g., returns [ 4, 3, 4, 4 ]
```

If provided an array-like object `pool`, the returned function will always sample from the supplied object.

```javascript
var mysample = sample.factory( [ 1, 2, 3, 4, 5, 6 ] );

var out = mysample();
// e.g., returns [ 2, 4, 1, 6, 5, 1 ]

out = mysample();
// e.g., returns [ 5, 2, 3, 6, 1, 4 ]
```

The function accepts the following `options`:

-   **seed**: pseudorandom number generator seed.
-   **size**: sample size.
-   **mutate**: `boolean` indicating whether to mutate the `pool` when sampling without replacement. Default: `false`.
-   **replace**: `boolean` indicating whether to sample with replacement. Default: `true`.

To seed the pseudorandom number generator, set the `seed` option.

```javascript
var mysample = sample.factory({
    'seed': 430
});

var out = mysample( [ 1, 2, 3, 4, 5, 6 ] );
// e.g., returns [ 1, 1, 1, 5, 4, 4 ]

mysample = sample.factory( [ 1, 2, 3, 4, 5, 6 ], {
    'seed': 430
});

out = mysample();
// e.g., returns [ 1, 1, 1, 5, 4, 4 ]
```

To specify a sample size and/or override the default sample size, set the `size` option.

```javascript
var mysample = sample.factory({
    'size': 4
});

var out = mysample( [ 0, 1 ] );
// e.g., returns [ 0, 0, 0, 1 ]

// Override the size option...
out = mysample( [ 0, 1 ], {
    'size': 1
});
// e.g., returns [ 1 ]
```

By default, the returned function draws samples _with_ replacement. To override the default `replace` strategy, set the `replace` option.

```javascript
var mysample = sample.factory({
    'replace': false
});

var out = mysample( [ 1, 2, 3 ] );
// e.g., returns [ 3, 1, 2 ]
```

If a population from which to sample is provided, the underlying `pool` remains constant for each function invocation. To mutate the `pool` by permanently removing observations when sampling _without_ replacement, set the `mutate` option.

```javascript
var mysample = sample.factory( [ 1, 2, 3, 4, 5, 6 ], {
    'mutate': true,
    'replace': false,
    'size': 3,
    'seed': 342
});

var out = mysample();
// e.g., returns [ 6, 5, 3 ]

// Override the mutate option...
out = mysample({
    'mutate': false
});
// e.g., returns [ 1, 2, 4 ]

out = mysample();
// e.g., returns [ 1, 2, 4 ]
```

The returned function returns `null` after all population units are exhausted.

```javascript
var mysample = sample.factory( [ 1, 2, 3, 4, 5, 6 ], {
    'mutate': true,
    'replace': false
});

var out = mysample();
// e.g., returns [ 3, 2, 1, 6, 5, 4 ]

out = mysample();
// returns null
```

</section>

<!-- /.usage -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="module">

import sample from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-sample@esm/index.mjs';

// By default, sample uniformly with replacement:
var x = [ 'a', 'b', 'c', 'd' ];
var out = sample( x, {
    'size': 10
});
// e.g., returns [ 'd', 'c', 'b', 'b', 'b', 'd', 'c', 'c', 'b', 'd' ]

// Sample with replacement with custom probabilities:
x = [ 'a', 'b', 'c', 'd' ];
out = sample( x, {
    'probs': [ 0.1, 0.1, 0.2, 0.6 ],
    'size': 10
});
// e.g., returns [ 'b', 'a', 'c', 'd', 'd', 'd', 'd', 'c', 'd', 'd' ]

// Sample without replacement:
x = [ 'a', 'b', 'c', 'd' ];
out = sample( x, {
    'size': 3,
    'replace': false
});
// e.g., returns [ 'd', 'c', 'a' ]

// Sample without replacement when (initial) probabilities are nonuniform:
x = [ 1, 2, 3, 4, 5, 6 ];
out = sample( x, {
    'probs': [ 0.1, 0.1, 0.1, 0.1, 0.1, 0.5 ],
    'size': 3,
    'replace': false
});
// e.g., returns [ 2, 3, 6 ]

</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section to include cited references. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="references">

### References

-   Knuth, Donald E. 1997. _The Art of Computer Programming, Volume 2 (3rd Ed.): Seminumerical Algorithms_. Boston, MA, USA: Addison-Wesley Longman Publishing Co., Inc.
-   Vose, Michael D. 1991. "A linear algorithm for generating random numbers with a given distribution." _IEEE Transactions on Software Engineering_ 17 (9): 972–75. doi:[10.1109/32.92917][@vose:1991].

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2022. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-sample.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-sample

[test-image]: https://github.com/stdlib-js/random-sample/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-sample/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-sample/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-sample?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-sample.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-sample/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://gitter.im/stdlib-js/stdlib/

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-sample/tree/deno
[umd-url]: https://github.com/stdlib-js/random-sample/tree/umd
[esm-url]: https://github.com/stdlib-js/random-sample/tree/esm
[branches-url]: https://github.com/stdlib-js/random-sample/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-sample/main/LICENSE

[@vose:1991]: https://doi.org/10.1109/32.92917

</section>

<!-- /.links -->
