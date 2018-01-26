# Ultrasound JavaScript Style Guide

<a name='guide'></a>

*A mostly reasonable approach to JavaScript*

> **Note:** this guide assumes you are using [Babel](https://babeljs.io/), and requires that you use [babel-preset-airbnb](https://npmjs.com/babel-preset-airbnb) or the equivalent. It also assumes you are installing shims/polyfills in your app, with [airbnb-browser-shims](https://npmjs.com/airbnb-browser-shims) or the equivalent.

## Table of Contents

<a name='toc'></a>

1. [Types](#guide-types)
1. [References](#guide-references)
1. [Objects](#guide-objects)
1. [Arrays](#guide-arrays)
1. [Destructuring](#guide-destructuring)
1. [Strings](#guide-string)
1. [Functions](#guide-functions)
1. [Arrow Functions](#guide-arrowFunctions)
1. [Classes & Constructors](#guide-classesConstructors)
1. [Modules](#guide-modules)
1. [Iterators & Generators](#guide-iteratorsGenerators)
1. [Properties](#guide-properties)
1. [Variables](#guide-variables)
1. [Hoisting](#guide-hoisting)
1. [Comparison Operators & Equality](#guide-comparisonOperatorsEquality)
1. [Blocks](#guide-blocks)
1. [Control Statements](#guide-controlStatements)
1. [Comments](#guide-comments)
1. [Whitespace](#guide-whitespace)
1. [Commas](#guide-commas)
1. [Semicolons](#guide-semicolons)
1. [Type Casting & Coercion](#guide-typeCastingCoercion)
1. [Naming Conventions](#guide-namingConventions)
1. [Accessors](#guide-accessors)
1. [Events](#guide-events)
1. [jQuery](#guide-jquery)
1. [ECMAScript 5 Compatibility](#guide-ecma5Compatibility)
1. [ECMAScript 6+ (ES 2015+) Styles](#guide-ecma6ES2015Styles)
1. [Standard Library](#guide-standardLibrary)
1. [Testing](#guide-testing)
1. [Performance](#guide-performance)
1. [Resources](#guide-resources)
1. [The Javascript Style Guide Guide](#guide-jsStyleGuideGuide)
1. [Contributors](#guide-contributors)
1. [License](#guide-license)
1. [Amendments](#guide-amendments)

## Types

<a name='guide-types-1.1'></a>

- [1.1](#guide-types-1.1) **Primitives:** When you access a primitive type, you work directly on its value.

  - `string`
  - `number`
  - `boolean`
  - `null`
  - `undefined`
  - `symbol`

  ```js
  const foo = 1;
  let bar = foo;

  bar = 9;

  console.log(foo, bar); // => 1, 9
  ```

  - Symbols cannot be faithfully polyfilled, so they should be used when targeting browsers/environments that don't support them natively.

- [1.2](#guide-types-1.2) **Complex:** When you access a complex type, you work on a reference to its value.

  - `object`
  - `array`
  - `function`

  ```js
  const foo = [1, 2];
  const bar = foo;

  bar[0] = 9;

  console.log(foo[0], bar[0]); // => 9, 9
  ```

**[⬆ back to top](#toc)**

## References

<a name='guide-references-2.1'></a>

- [2.1](#guide-references-2.1) Use `const` for all of your references; avoid using `var`.

**eslint:** [`prefer_const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

  > Why? This ensures that you can't reassign your references, which can lead to bugs and difficult to comprehend code.

  ```js
  // bad
  var a = 1;
  var b = 2;

  // good
  const a = 1;
  const b = 2;
  ```

<a name='guide-references-2.2'></a>

- [2.2](#guide-references-2.2) If you must reassign references, use `let` instead of `var`.

  - **eslint:** [`no-var`](https://eslint.org/docs/rules/no-var.html)

  **- jsc**s: [`disallowVar`](http://jscs.info/rule/disallowVar)

  > Why? `let` is block-scoped rather than function-scoped like `var`.

  ```js
  // bad
  var count = 1;
  if (true) {
    count += 1;
  };

  // good, use the let.
  let count = 1;
  if (true) {
    count += 1;
  };
  ```

<a name='guide-references-2.3'></a>

- [2.3](#guide-references-2.3) Note that both `let` and `const` are block-scoped.

  ```js
  // const and let only exist in the blocks they are defined in.
  {
    let a = 1;
    const b = 1;
  }
  console.log(a); // ReferenceError
  console.log(b); // ReferenceError
  ```

**[⬆ back to top](#toc)**

## Objects

<a name='guide-objects-3.1'></a>

- [3.1](#guide-objects-3.1) Use the literal syntax for object creation.

  **eslint:** [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

  ```js
  // bad
  const item = new Object();

  // good
  const item = {};
  ```

<a name='guide-objects-3.2'></a>

- [3.2](#guide-objects-3.2) Use computed property names when creating objects with dynamic property names.

  > Why? They allow you to define all the properties of an object in one place.

  ```js
  function getKey(k) {
    return `a key named ${k}`;
  }

  // bad
  const obj = {
    id: 5,
    name: 'San Francisco',
  };
  obj[getKey('enabled')] = true;

  // good
  const obj = {
    id: 5,
    name: 'San Francisco',
    [getKey('enabled')]: true,
  };
  ```

<a name='guide-objects-3.3'></a>

- [3.3](#guide-objects-3.3) Use object method shorthand.

  **eslint:** [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  **jscs:** [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

  ```js
  // bad
  const atom = {
    value: 1,

    addValue: function(value) {
      return atom.value + value;
    },
  };

  // good
  const atom = {
    value: 1,

    addValue(value) {
      return atom.value + value;
    },
  };
  ```

<a name='guide-objects-3.4'></a>

- [3.4](#guide-objects-3.4) Use property value shorthand.

  **eslint:** [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

  **jscs:** [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

  > Why? It is shorter to write and descriptive.

  ```js
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    lukeSkywalker: lukeSkywalker,
  };

  // good
  const obj = {
    lukeSkywalker,
  };
  ```

<a name='guide-objects-3.5'></a>

- [3.5](#guide-objects-3.5) Group your shorthand properties at the beginning of your object declaration.

  > Why? It's easier to tell which properties are using the shorthand.

  ```js
  const anakinSkywalker = 'Anakin Skywalker';
  const lukeSkywalker = 'Luke Skywalker';

  // bad
  const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
  };

  // good
  const obj = {
    anakinSkywalker,
    lukeSkywalker,
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
  };
  ```

<a name='guide-objects-3.6'></a>

- [3.6](#guide-objects-3.6) Only quote properties that are invalid identifiers.

  **eslint:** [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

  **jscs:** [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

  > Why? In general, we consider it subjectively easier to read. It improves syntax highlighting, and is also more easily optimized by many JS engines.

  ```js
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5,
  };
  ```

<a name='guide-objects-3.7'></a>

- [3.7](#guide-objects-3.7) Do not call `Object.prototype` methods directly, such as `hasOwnProperty`, `propertyIsEnumerable`, and `isPrototypeOf`.

  > Why? These methods may be shadowed by properties on the object in question - consider `{ hasOwnProperty: false }` - or, the object may be a null object (`Object.create(null)`).

  ```js
  // bad
  console.log(object.hasOwnProperty(key));

  // good
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // best
  const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
  /* or */
  import has from 'has'; // https://www.npmjs.com/package/has
  // ...
  console.log(has.call(object, key));
  ```

<a name='guide-objects-3.8'></a>

- [3.8](#guide-objects-3.8) Prefer the object spread operator over `Object.assign` to shallow-copy objects. Use the object rest operator to get a new object with certain properties omitted.

  ```js
  // very bad
  const original = { a: 1, b: 2};
  const copy = Object.assign(original, { c: 3 }); // this mutates `original` ಠ_ಠ
  delete copy.a; // so does this

  // bad
  const original = { a: 1, b: 2 };
  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // good
  const original = { a: 1, b: 2 };
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }

  const { a, ...noA} = copy; // noA => { b: 2, c: 3 };
  ```

**[⬆ back to top](#toc)**

## Objects

<a name='guide-arrays-4.1'></a>

- [4.1](@guide-arrays-4.1) Use the literal syntax for array creation.

  **eslint:** [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

  ```js
  // bad
  const items = new Array();

  // good
  const items = [];
  ```

<a name='guide-arrays-4.2'></a>

- [4.2](#guide-arrays-4.2) Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) instead of direct assignment to add items to an array.

  ```js
  const someStack = [];

  // bad
  someStack[someStack.length] = 'abracadabra';

  // good
  someStack.push('abracadabra');
  ```

<a name='guide-arrays-4.3'></a>

- [4.3](#guide-arrays-4.3) Use array spreads `...` to copy arrays.

  ```js
  // bad
  const len = items.length;
  const itemsCopy = [];
  let i;

  for (let i = 0; i < len; i++) {
    itemsCopy[i] = items[i];
  };

  // good
  const itemsCopy = [...items];
  ```

<a name='guide-arrays-4.4'></a>

- [4.4](#guide-arrays-4.4) To convert an array-like object to an array, use spreads `...` instead of [Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

  ```js
  const foo = document.querySelectorAll('.foo');

  // good
  const nodes = Array.from(foo);

  // best
  const nodes = [...foo];
  ```

<a name='guide-arrays-4.5'></a>

- [4.5](#guide-arrays-4.5) Use [Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) instead of spread `...` for mapping over iterables, because it avoids creating an intermediate array.

  ```js
  // bad
  const baz = [...foo].map(bar);

  // good
  const baz = Array.from(foo, bar);
  ```

<a name='guide-arrays-4.6'></a>

- [4.6](#guide-arrays-4.6) Use return statements in array method callbacks. It's okay to omit the return if the function body consists of a single statement returning an expression without side effects, following [8.2](#guide-arrowFunctions-8.2).

  **eslint:** [array-callback-return](https://eslint.org/docs/rules/array-callback-return)

  ```js
  // good
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map(x => x + 1);

  // bad - no returned value means 'memo' becomes undefined after the first iteration
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    memo[index] = flatten;
  });

  // good
  [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
    const flatten = memo.concat(item);
    memo[index] = flatten;
    return flatten;
  });

  // bad
  inbox.filter(msg => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee';
    } else {
      return false;
    };
  });

  // good
  inbox.filter(msg => {
    const { subject, author } = msg;
    if (subject === 'Mockingbird') {
      return author === 'Harper Lee';
    };
    return false;
  });
  ```

<a name='guide-arrays-4.7'></a>

- [4.7](#guide-arrays-4.7) Use line breaks after open and close array brackets if an array has multiple lines

  ```js
  // bad
  const arr = [
    [0, 1], [2, 3], [4, 5],
  ];
  const objectInArray = [{
    id: 1,
  }, {
    id: 2,
  }];
  const numberInArray = [
    1, 2,
  ];

  // good
  const arr = [[0, 1], [2, 3], [4, 5]];
  const objectInArray = {
    {
      id: 1,
    },
    {
      id: 2,
    },
  };
  const numberInArray = [
    1,
    2,
  ];
  ```

**[⬆ back to top](#toc)**

## Destructuring

<a name='guide-destructuring-5.1'></a>

- [5.1](#guide-destructuring-5.1) Use object destructuring when accessing and using multiple properties of an object.

  **eslint:** [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  **jscs:** [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

  > Why? Destructuring saves you from creating temporary references for those properties.

  ```js
  // bad
  function getFullName(user) {
    const firstName = user.firstName;
    const lastName = user.lastName;
    return `${firstName} ${lastName}`;
  };

  // good
  function getFullName(user) {
    const { firstName, lastName } = user;
    return `${firstName} ${lastName}`;
  };

  // best
  function getFullName({ firstName, lastName }) {
    return `${firstName} ${lastName}`;
  };
  ```

<a name='guide-destructuring-5.2'></a>

- [5.2](#guide-destructuring-5.2) Use array destructuring.

  **eslint:** [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

  **jscs:** [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)

  ```js
  const arr = [1, 2, 3, 4];

  // bad
  const first = arr[0];
  const second = arr[1];

  // good
  const [first, second] = arr;
  ```

<a name='guide-destructuring-5.3'></a>

- [5.3](#guide-destructuring-5.3) Use object destructuring for multiple return values, not array destructuring.

  **jscs:** [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)

  > Why? You can add new properties over time or change the order of things without breaking call sites.

  ```js
  // bad
  function processInput(input) {
    // then a miracle occurs
    return [left, right, top, bottom];
  };

  // the caller needs to think about the order of return data
  const [left, __, top] = processInput(input);

  // good
  function processInput(input) {
    // then a miracle occurs
    return { left, right, top, bottom };
  };

  // the caller selects only the data they need
  const { left, top } = processInput(input);
  ```

**[⬆ back to top](#toc)**

## Strings

<a name='guide-strings-6.1'></a>

- [6.1](#guide-strings-6.1) Use single quotes `''` for strings.

  **eslint:** [`quotes`](https://eslint.org/docs/rules/quotes.html)

  **jscs:** [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

  ```js
  // bad
  const name = "Capt. Janeway";

  // bad - template literals should contain interpolation or newlines
  const name = `Capt. Janeway`;

  // good
  const name = 'Capt. Janeway';
  ```

<a name='guide-strings-6.2'></a>

- [6.2](#guide-strings-6.2) Strings that cause the line to go over 100 characters should not be written across multiple lines using string concatenation;

  > Why? Broken strings are painful to work with and make code less searchable.

  ```js
  // bad
  const errorMessage = 'This is a super long error that was thrown because \
  ofBatman. When you stop to think about how Batman had anything to do \
  with this, you would get nowhere \
  fast.';

  // bad
  const errorMessage = 'This is a super long error that was thrown because ' +
    'of Batman. When you stop to think about how Batman had anything to do ' +
    'with this, you would get nowhere fast.';

  // good
  const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
  ```

<a name='guide-strings-6.3'></a>

- [6.3](#guide-strings-6.3) When programmatically building up strings, use template strings instead of concatenation.

  **eslint:** [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html), [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

  **jscs:** [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

  > Why? Template strings give you a readable, concise syntax with proper newlines and string interpolation features.

  ```js
  // bad
  function sayHi(name) {
    return 'How are you, ' + name + '?';
  };

  // bad
  function sayHi(name) {
    return ['How are you', name, '?'].join();
  };

  // bad
  function sayHi(name) {
    return `How are you, ${ name }?`;
  };

  // good
  function sayHi(name) {
    return `How are you, ${name}?`;
  };
  ```

<a name='guide-strings-6.4'></a>

- [6.4](#guide-strings-6.4) Never use `eval()` on a string, it opens too many vulnerabilities.

  **eslint:** [`no-eval`](https://eslint.org/docs/rules/no-eval)

<a name='guide-strings-6.5'></a>

- [6.5](#guide-strings-6.5) Do not unnecessarily escape characters in strings.

  **eslint:** [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

  > Why? Backslashes harm readability, this they should only be present when necessary.

  ```js
  // bad
  const foo = '\'this\' \i\s \"quoted\"';

  // good
  const foo = '\'this\' is "quoted"';
  const foo = `my name is '${name}'`;
  ```

**[⬆ back to top](#toc)**

## Functions

<a name='guide-functions-7.1'></a>

- [7.1](#guide-functions-7.1) Use named function expressions instead of function declarations.

  **eslint:** [`func-style`](https://eslint.org/docs/rules/func-style)

  **jscs:** [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)

  > Why? Function declarations are hoisted, which means that it's easy - too easy - to reference the function before it is defined in the file. This harms readability and maintainability. If you fin that a function's definition is large or complex enough that it is interfering with understanding the rest of the file, then perhaps it's time to extract it to its own module! Don't forget to explicitly name the expression, regardless of whether or not the name is inferred from the containing variable (which is often the case in modern browsers or when using compilers such as Babel). This eliminates any assumptions made about the Error's call stack. ([Discussion](https://github.com/airbnb/javascript/issues/794))

  ```js
  // bad
  function foo() {
    // ...
  };

  // bad
  const foo = function () {
    // ...
  };

  // good
  // lexical name distinguished from the variable-referenced invocation(s)
  const short = function longUniqueMoreDescriptiveLexicalFoo() {
    // ...
  };
  ```

<a name='guide-functions-7.2'></a>

- [7.2](#guide-functions-7.2) Wrap immediately invoked function expressions in parantheses.

  **eslint:** [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

  **jscs:** [`requireParanthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

  > Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE.

  ```js
  // immediately-invoked function expression (IIFE)
  (function () {
    console.log('Welcome to the Internet. Please follow me.');
  }());
  ```

<a name='guide-functions-7.3'></a>

- [7.3](#guide-functions-7.3) Never declare a function in a non-function block (`if`, `while`, etc.). Assign the function to a variable instead. Browsers will allow you to do it, but they all interpret it differently, which is bad news bears.

  **eslint:** [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

<a name='guide-functions-7.4'></a>

- [7.4](#guide-functions-7.4) **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement.

  ```js
  // bad
  if (currentUser) {
    function test() {
      console.log('Nope.');
    };
  };

  // good
  let test;
  if (currentUser) {
    test = () => {
      console.log('Yup.');
    };
  };
  ```

<a name='guide-functions-7.5'></a>

- [7.5](#guide-functions-7.5) Never name a parameter `arguments`. This will take precedence over the `arguments` object that is given to every function scope.

  ```js
  // bad
  function foo(name, options, arguments) {
    // ...
  };

  // good
  function foo(name, options, args) {
    // ...
  };
  ```

<a name='guide-functions-7.6'></a>

- [7.6](#guide-functions-7.6) Never use `arguments`, opt to use rest syntax `...` instead.

  **eslint:** [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

  > Why? `...` is explicit about which arguments you want pulled. Plus, rest arguments are a real Array, and not merely Array-like like `arguments`.

  ```js
  // bad
  function concatenateAll() {
    const args = Array.prototype.slice.call(arguments);
    return args.join();
  };

  // good
  function concatenateAll(...args) {
    return args.join();
  };
  ```

<a name='guide-functions-7.7'></a>

- [7.7](#guide-functions-7.7) Use default parameter syntax rather than mutating function arguments.

  ```js
  // really bad
  function handleThings(opts) {
    // No! We shouldn't mutate function arguments.
    // Double load: if opts is falsy, it'll be set to an object which may
    // be what you want but it can introduce subtle bugs.
    opts = opts || {};
    // ...
  };

  // still bad
  function handleThings(opts) {
    if (opts === void 0) {
      opts = {};
    };
    // ...
  };

  // good
  function handleThings(opts = {}) {
    // ...
  };
  ```

<a name='guide-functions-7.8'></a>

- [7.8](#guide-functions-7.8) Avoid side effects with default parameters.

  > Why? They are confusing to reason about.

  ```js
  var b = 1;
  // bad
  function count(a = b++) {
    console.log(a);
  };
  count();  // 1
  count();  // 2
  count(3); // 3
  count();  // 3
  ```

<a name='guide-functions-7.9'></a>

- [7.9](#guide-functions-7.9) Always put default parameters last.

  ```js
  // bad
  function handleThings(opts = {}, name) {
    // ...
  };

  // good
  function handleThings(name, opts = {}) {
    // ...
  };
  ```

<a name='guide-functions-7.10'></a>

- [7.10](#guide-functions-7.10) Never use the Function constructor to create a new function.

  **eslint:** [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

  > Why? Creating a function in this way evaluates a string similarly to eval(), which opens vulnerabilities.

  ```js
  // bad
  var add = new Function('a', 'b', 'return a + b');

  // still bad
  var subtract = Function('a', 'b', 'return a - b');
  ```

<a name='guide-functions-7.11'></a>

- [7.11](#guide-functions-7.11) Spacing is a function signature.

  **eslint:** [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren),  [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

  > Why? Consistency is good, and you shoudn't have to add or remove a space when adding or removing a name.

  ```js
  // bad
  const f = function(){};
  const g = function (){};
  const h = function() {};

  // good
  const x = function () {};
  const y = function a() {};
  ```

<a name='guide-functions-7.12'></a>

- [7.12](#guide-functions-7.12) Never mutate parameters.

  **eslint:** [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  > Why? Manipulating objects passed in as parameters can cause unwanted variable side effects in the original caller.

  ```js
  // bad
  function f1(obj) {
    obj.key = 1;
  };

  // good
  function f2(obj) {
    const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
  };
  ```

<a name='guide-functions-7.13'></a>

- [7.13](#guide-functions-7.13) Never reassign parameters.

  **eslint:** [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

  > Why? Reassigning parameters can lead to unexpected behavior, especially when accessing the `arguments` object. It can also cause optimization issues, especially in V8.

  ```js
  // bad
  function f1(a) {
    a = 1;
    // ...
  };
  function f2(a) {
    if (!a) { a = 1; };
    // ...
  };

  // good
  function f3(a) {
    const b = a || 1;
    // ...
  };
  function f4(a = 1) {
    // ...
  };
  ```

<a name='guide-functions-7.14'></a>

- [7.14](#guide-functions-7.14) Prefer the use of the spread operator `...` to call variadic functions.

  **eslint:** [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

  > Why? It's cleaner, you don't need to supply a context, and you can not easily compose `new` with `apply`.

  ```js
  // bad
  const x = [1, 2, 3, 4, 5];
  console.log.apply(console, x);

  // good
  const x = [1, 2, 3, 4, 5];
  console.log(...x);

  // bad
  new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]));

  // good
  new Date(...2016, 8, 5);
  ```

<a name='guide-functions-7.15'></a>

- [7.15](@guide-functions-7.15) Functions with multiline signatures, or invocations, should be indented just like every other multiline list in this guide: with each item ona  line by itself, with a trailing comma on the last item.

  ```js
  // bad
  function foo(bar,
               baz,
               quux) {
    // ...
  };

  // good
  function foo(
    bar,
    baz,
    quux,
  ) {
    // ...
  };

  // bad
  console.log(foo,
    bar,
    baz);

  // good
  console.log(
    foo,
    bar,
    baz,
  );
  ```

**[⬆ back to top](#toc)**

## Arrow Functions

<a name='guide-arrowFunctions-8.1'></a>

- [8.1](#guide-arrowFunctions-8.1) When you must us an anonymous function (as when passing an inline callback), use arrow function notation.

  > Why? it creates a version of the function that executes in teh context of `this`, which is usually what you want, and is a more concise syntax.
  >
  > Why not? If you have a fairly complicated function, you might move that logic out into its own named function expression.

  ```js
  // bad
  [1, 2, 3].map(function (x) {
    const y = x + 1;
    return x * y;
  });

  // good
  [1, 2, 3].map(x => {
    const y = x + 1;
    return x * y;
  });
  ```

<a name='guide-arrowFunctions-8.2'></a>

- [8.2](#guide-arrowFunctions-8.2) If the function body consists of a single statement returning an [expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) without side effects, omit the braces and use the implicit return. If there is only one parameter, omit the braces as well. Otherwise, keep the braces and use a `return` statement.

  **eslint:** [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

  **jscs:** [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

  > Why? Syntactic sugar. It reads well when multiple functions are chained together.

  ```js
  // bad
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map(number => `A string containing the ${number}.`);

  // good
  [1, 2, 3].map(number => {
    const nextNumber = number + 1;
    return `A string containing the ${nextNumber}.`;
  });

  // good
  [1, 2, 3].map((number, index) => ({
    [index]: number,
  }));

  // No implicit return with side effects
  function foo(callback) {
    const val = callback();
    if (val === true) {
      // Do something if callback returns true
    };
  };

  let bool = false;

  // bad
  foo(() => bool = true);

  // good
  foo(() => {
    bool = true;
  });
  ```

<a name='guide-arrowFunctions-8.3'></a>

- [8.3](#guide-arrowFunctions-8.3) In case the expression spans over multiple lines, wrap it in parentehese for better readability.

  > Why? It shows clearly where the function starts and ends.

  ```js
  // bad
  ['get', 'post', 'put'].map(httpMethod => Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod,
    )
  );

  // good
  ['get', 'post', 'pput'].map(httpMethod => (
    Object.prototype.hasOwnProperty.call(
      httpMagicObjectWithAVeryLongName,
      httpMethod,
    )
  ));
  ```

<a name='guide-arrowFunctions-8.4'></a>

- [8.4](#guide-arrowFunctions-8.4) If your function takes a single argument, omit the parentheses. Otherwise, always include parentheses around arguments for clarity and consistency. Note: it is also acceptable to always use parentheses, in which case, use the ["always" option](https://eslint.org/docs/rules/arrow-parens#always) for eslint or do not include [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam) for jscs:

  **eslint:** [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html)

  **jscs:** [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

  > Why? Less visual clutter.

  ```js
  // bad
  [1, 2, 3].map((x) => x * x);

  // good
  [1, 2, 3].map(x => x * x);

  // good
  [1, 2, 3].map(number => (
    `A long string with the ${number}. It's so long that we don't want it to take up space on the .map line!`
  ));

  // bad
  [1, 2, 3].map((x) => {
    const y = x + 1;
    return x * y;
  });
  ```

<a name='guide-arrowFunctions-8.5'></a>

- [8.5](#guide-arrowFunctions-8.5) Aboid confusing arrow function synatax (`=>`) with comparison operators (`<=`, `>=`).

  **eslint:** [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

  ```js
  // bad
  const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

  // bad
  const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

  // good
  const itemHeight = item => (
    item.height > 256 ? item.largeSize : item.smallSize
  );

  // good
  const itemHeight = item => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
  };
  ```

**[⬆ back to top](#toc)**

## Classes & Constructors

<a name='module+jsStyleGuide-classesConstructors-9.1'></a>

- [9.1](#guide-classesConstructors-9.1) Always use `class`. Avoid manipulating `prototype` directly.

  > Why? `class` syntax is more concise and easier to reason about.

  ```js
  // bad
  function Queue(contents = []) {
    this.queue = [...contents];
  };
  Queue.prototype.pop = function () {
    const value = this.queue[0];
    this.queue.splice(0, 1);
    return value;
  };

  // good
  class Queue {
    constructor(contents = []) {
      this.queue = [...contents];
    };
    pop() {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };
  };
  ```

<a name='guide-classesConstructors-9.2'></a>

- [9.2](#guide-classesConstructors-9.2) Use `extends` for inheritance.

  > Why? It is a built-in way to inherit prototype functionality without breaking `instanceOf`.

  ```js
  // bad
  const inherits = require('inherits');
  function PeekableQueue(contents) {
    Queue.apply(this, contents);
  };
  inherits(PeekableQueue, Queue);
  PeekableQueue.prototype.peek = function () {
    return this.queue[0];
  };

  // good
  class PeekableQueue extends Queue {
    peek() {
      return this.queue[0];
    };
  };
  ```

<a name='guide-classesConstructors-9.3'></a>

- [9.3](#guide-classesConstructors-9.3) Methods can return `this` to help with method chaining.

  ```js
  // bad
  Jedi.prototype.jump = function () {
    this.jumping = true;
    return true;
  };

  Jedi.prototype.setHeight = function (height) {
    this.height = height;
  };

  const luke = new Jedi();
  luke.jump();
  luke.setHeight(20); // => undefined

  // good
  class Jedi {
    jump() {
      this.jumping = true;
      return this;
    };
    setHeight(height) {
      this.height = height;
      return this;
    };
  };

  const luke = new Jedi();

  luke.jump()
    .setHeight(20);
  ```

<a name='guide-classesConstructors-9.4'></a>

- [9.4](#guide-classesConstructors-9.4) It's okay to write a custom `toString()` method, just make sure it works successfully and causes no side effects.

  ```js
  class Jedi {
    constructor(options = {}) {
      this.name = options.name || 'no name';
    };
    getName() {
      return this.name;
    };
    toString() {
      return `Jedi - ${this.getName}`;
    };
  };
  ```

<a name='guide-classesConstructors-9.5'></a>

- [9.5](#guide-classesConstructors-9.5) Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary.

  **eslint:** [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

  ```js
  // bad
  class Jedi {
    constructor() {};
    getName() {
      return this.name;
    };
  };

  // bad
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
    };
  };

  // good
  class Rey extends Jedi {
    constructor(...args) {
      super(...args);
      this.name = 'Rey';
    };
  };
  ```

<a name='guide-classesConstructors-9.6'></a>

- [9.6](#guide-classesConstructors-9.6) Avoid duplicate class members.

  **eslint:** [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

  > Why? Duplicate class member declarations will silently prefer the last one = having duplicates is almost certainly a bug.

  ```js
  // bad
  class Foo {
    bar() { return 1; };
    bar() { return 2; };
  };

  // good
  class Foo {
    bar() { return 1; };
  };

  // good
  class Foo {
    bar() { return 2; };
  };
  ```

**[⬆ back to top](#toc)**

## Modules

<a name='guide-modules-10.1'></a>

- [10.1](#guide-modules-10.1) Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

  > Why? Modules are the future, let's start using the future now.

  ```js
  // bad
  const UltrasoundStyleGuide = require('./UltrasoundStyleGuide');
  module.exports = UltrasoundStyleGuide;

  // okay
  import UltrasoundStyleGuide from './UltrasoundStyleGuide';
  export default UltrasoundStyleGuide.es6;

  // best
  import { es6 } from './UltrasoundStyleGuide';
  export default es6;
  ```

<a name='guide-modules-10.2'></a>

- [10.2](#guide-modules-10.2) Do not use wildcard imports.

  > Why? This makes sure you have a single default export.

  ```js
  // bad
  import * as UltrasoundStyleGuide from './UltrasoundStyleGuide';

  // good
  import UltrasoundStyleGuide from './UltrasoundStyleGuide';
  ```

<a name='guide-modules-10.3'></a>

- [10.3](#guide-modules-10.3) And do not export directly from an import.

  > Why? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

  ```js
  // bad
  // filename es6.js
  export { es6 as default } from './UltrasoundStyleGuide';

  // good
  // filename es6.js
  import { es6 } from './UltrasoundStyleGuide';
  export default es6;
  ```

<a name='guide-modules-10.4'></a>

- [10.4](#guide-modules-10.4) Only import from a path in one place.

  **eslint:** [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)

  > Why? Having multiple lines that import from the same path can make code harder to maintain.

  ```js
  // bad
  import foo from './foo';
  // ... some other imports ... ///
  import { named1, named2 } from 'foo';

  // good
  import foo, { named1, named2 } from 'foo;

  // good
  import foo, {
    named1,
    named2,
  } from 'foo';
  ```

<a name='guide-modules-10.5'></a>

- [10.5](#guide-modules-10.5) Do not export mutable bindings.

  **eslint:** [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)

  > Why? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported.

  ```js
  // bad
  let foo = 3;
  export { foo };

  // good
  const foo = 3;
  export { foo };
  ```

<a name='guide-modules-10.6'></a>

- [10.6](#guide-modules-10.6) In modules witha  single export, prefer default export over named export.

  **eslint:** [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

  > Why? To encourage more files that only ever export one thing, which is better for readability and maintainability.

  ```js
  // bad
  export function foo() {};

  // good
  export default function foo() {};
  ```

<a name='guide-modules-10.7'></a>

- [10.7](#guide-modules-10.7) Put all `import`s above non-import statements.

  **eslint:** [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)

  > Why? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

  ```js
  // bad
  import foo from 'foo';
  foo.init();

  import bar from 'bar';

  // good
  import foo from 'foo';
  import bar from 'bar';

  foo.init();
  ```

<a name='guide-modules-10.8'></a>

- [10.8](#guide-modules-10.8) Multiline imports should be indented just like multiline array and object literals.

  > Why? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas.

  ```js
  // bad
  import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path';

  // good
  import {
    longNameA,
    longNameB,
    longNameC,
    longNameD,
    longNameE,
  } from 'path';
  ```

<a name='guide-modules-10.9'></a>

- [10.9](#guide-modules-10.9) Disallow Webpack loader syntax in module import statements.

  **eslint:** [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)

  > Why? Since using Webpack syntax in the imports couples the code to a module bundler. Prefer using the loader syntax in `webpack.config.js`.

  ```js
  // bad
  import fooSass from 'css!sass!foo.scss';
  import barCss from 'style!css!bar.css;

  // good
  import fooSass from 'foo.scss';
  import barCss from 'bar.css';
  ```

**[⬆ back to top](#toc)**

## Iterators & Generators

<a name='guide-iteratorsGenerators-11.1'></a>

- [11.1](#guide-iteratorsGenerators-11.1) Don't use iterators. Prefer JavaScript's higher-order functions instead of loops like `for-in` or `for-of`.

  **eslint:** [`no-iterators`](https://eslint.org/docs/rules/no-iterator.html), [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

  > Why? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects.
  >
  > Use `map()` | `every()` | `filter()` | `find()` | `findIndex()` | `reduce()` | `some()` | ... to iterate over arrays, and `Object.keys()` | `Object.values()` | `Object.entries()` to produce arrays so you can iterate over objects.

  ```js
  const numbers = [1, 2, 3, 4, 5];

  // bad
  let sum = 0;
  for (let num of numbers) {
    sum += num;
  };
  sum === 15;

  // good
  let sum = 0;
  numbers.forEach(num => sum += num);
  sum === 15;

  // best (use the functional force)
  const sum = reduce((total, num) => total + num, 0);
  sum === 15;

  // bad
  const increasedByOne = [];
  for (let i = 0; i < numbers.length; i++) {
    increasedByOne.push(num + 1);
  };

  // best (keeping it functional)
  const increasedByOne = numbers.map(num => num + 1;)
  ```

<a name='guide-iteratorsGenerators-11.2'></a>

- [11.2](#guide-iteratorsGenerators-11.2) Don't use generators for now.

  > Why? They don't transpile well to ES5.

<a name='guide-iteratorsGenerators-11.3'></a>

- [11.3](#guide-iteratorsGenerators-11.3) If you must use generators, or if you disregard [our advice](#guide-iteratorsGenerators-11.2), make sure their function signature is spaced properly.

  **eslint:** [`generator-star-spacing`](https://eslint.org/docs/rules/generator-star-spacing)

  > Why? `function` and `*` are part of the same conceptual keyword - `*` is not a modifier for `function`, `function*` is a unique construct, different from `function`.

  ```js
  // bad
  function * foo() {
    // ...
  };

  // bad
  const bar = function * () {
    // ...
  };

  // bad
  const baz = function *() {
    // ...
  };

  // bad
  const quux = function*() {
    // ...
  };

  // bad
  function*foo() {
    // ...
  };

  // bad
  function *foo() {
    // ...
  };

  // very bad
  function
  *
  foo() {
    // ...
  };

  // very bad
  const wat = function
  *
  () {
    // ...
  };

  // good
  function* foo() {
    // ...
  };

  // good
  const foo = function* () {
    // ...
  };
  ```

**[⬆ back to top](#toc)**

## Properties

<a name='guide-properties-12.1'></a>

- [12.1](#guide-properties-12.1) Use dot notation when accessing properties.

  **eslint:** [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

  **jscs:** [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

  ```js
  const luke = {
    jedi: true,
    age: 28,
  };

  // bad
  const isJedi = luke['jedi'];

  // good
  const isJedi = luke.jedi;
  ```

<a name='guide-properties-12.2'></a>

- [12.2](#guide-properties-12.2) Use bracker notation `[]` when accessing properties with a variable.

  ```js
  const luke = {
    jedi: true,
    age: 28,
  };

  function getProp(prop) {
    return luke[prop];
  };

  const isJedi = getProp('jedi');
  ```

<a name='guide-properties-12.3'></a>

- [12.3](#guide-properties-12.3) Use exponentiation operator `**` when calculating exponentiations.

  **eslint:** [`no-restricted-properties`](https://eslint.org/docs/rules/no-restricted-properties)

  ```js
  // bad
  const binary = Math.pow(2, 10);

  // good
  const binary = 2 ** 10;
  ```

**[⬆ back to top](#toc)**

## Variables

<a name='guide-variables-13.1'></a>

- [13.1](#guide-variables-13.1) Always use `const` and `let` to declare vraiables. Not doing so will result in global cariables. We want to avoid polluting the gloval namespace. Captain Planet warned is of that.

  esline: [`no-undef`](https://eslint.org/docs/rules/no-undef), [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

  ```js
  // bad
  superPower = new SuperPower();

  // good
  const superPower = new SuperPower();
  ```

<a name='guide-variables-13.2'></a>

- [13.2](#guide-variables-13.2) Use one `const` and `let` declaration per variable.

  **eslint:** [`one-var`](https://eslint.org/docs/rules/one-var.html)

  **jscs:** [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

  > Why? It's easier to add new variable declarations this way, and you never have to worry about swapping out a `;` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once.

  ```js
  // bad
  const items = getItems(),
    goSportsTeam = true,
    drafonball = 'z';

  // bad
  // (compare to above, and try to spot the mistake)
  const items = getItems(),
    goSportsTeam = true;
    dragonball - 'z';

  // good
  const items = getItems();
  const goSportsTeam = true;
  const dragonball = 'z';
  ```

<a name='guide-variables-13.3'></a>

- [13.3](#guide-variables-13.3) Group all your `const`s and then group all your `let`s.

  > Why? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

  ```js
  // bad
  let i, len, dragonball,
    items = getItems(),
    goSportsTeam = true;

  // bad
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // good
  const goSports = true;
  const items = getItems();
  let dragonball;
  let i;
  let length;
  ```

<a name='guide-variables-13.4'></a>

- [13.4](#guide-variables-13.4) Assign variables when where you need them, but place them in a reasonable place.

  > Why? `let` and `const` are block scoped and not function scoped.

  ```js
  // bad - unnecessary function call
  function checkName(hasName) {
    const name = getName();

    if (hasName === 'test') {
      return false;
    };
    if (name === 'test') {
      this.setName('');
      return false;
    };
    return name;
  };

  // good
  function checkName(hasName) {
    if (hasName === 'test') {
      return false;
    };

    const name = getName();

    if (name === 'test') {
      this.setName('');
      return false;
    };
    return name;
  };
  ```

<a name-'guide-variables-13.5'></a>

- [13.5](#guide-variables-13.5) Don't chain variable assignments.

  **eslint:** [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

  > Why? Chaining variable assignments creates implicit global variables.

  ```js
  // bad
  (function example() {
    // JavaScript interprets this as
    // let a = ( b = (c = 1 ) );
    // The let keyword only applies to variable a; variables b and c become
    // global variables.
    let a = b = c = 1;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // 1
  console.log(c); // 1

  // good
  (function example() {
    let a = 1;
    let b = a;
    let c = a;
  }());

  console.log(a); // throws ReferenceError
  console.log(b); // throws ReferenceError
  console.log(c); // throws ReferenceError
  ```

  > **Note:** The same applies for `const`.

<a name='guide-variables-13.6'></a>

- [13.6](#guide-variables-13.6) Avoid using unary increments and decrements (++, --).

  **eslint:** [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

  > Why? Per the eslint documentation, unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

  ```js
  // bad
  const array = [1, 2, 3];
  let num = 1;
  num++;
  --num;

  let sum = 0;
  let truthyCount = 0;
  for (let i = 0; i < array.length; i++) {
    let value = array[i];
    sum += value;
    if (value) {
      truthyCount++;
    };
  };

  // good
  const array = [1, 2, 3];
  let num = 1;
  num += 1;
  num -= 1;

  const sum = array.reduce((a, b) => a + b, 0);
  const truthyCount = array.filter(Boolean).length;
  ```

**[⬆ back to top](#toc)**

## Hoisting

<a name='guide-hoisting-14.1'></a>

- [14.1](#guide-hoisting-14.1) `var` declarations get hoisted to the top of their closest enclosing function scope, their assignments does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_Dead_Zone_and_errors_with_let). It's important to know why [typeof is not longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15).

  ```js
  // we know this wouldn't work (assuming there is no notDefined global
  // variables)
  function example() {
    console.log(notDefined); // => throws ReferenceError
  };

  // creating a vraiables declaration after you reference the variable
  // will work due to variable hoisting. Note: the assignment value of
  // `true` is not hoisted.
  function example() {
    console.log(declaredButNotAssigned); // => undefined
    var declaredButNotAssigned = true;
  };

  // the interpreter is hoisting the variable declaration to the top of
  // the scope, which means our example could be rewritten as:
  function example() {
    let declaredButNotAssigned;
    console.log(declaredButNotAssigned); // => undefined
    delcaredButNotAssigned = true;
  };

  // using const and let
  function example() {
    console.log(declaredButNotAssigned); // => throws ReferenceError
    console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
    const declaredButNotAssigned = true;
  };
  ```

<a name='guide-variables-14.2'></a>

- [14.2](#guide-variables-14.2) Anonymous function expressions hoist their variable name, but not the function assignment.

  ```js
  function example() {
    console.log(anonymous); // => undefined

    anonymous(); // => TypeError anonymous is not a function

    var anonymous = function () {
      console.log('anonymous function expression');
    };
  };
  ```

<a name='guide-variables-14.3'></a>

- [14.3](#guide-variables-14.3) Named function expressions hoist the variable name, not the function name or the function body.

  ```js
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    superPower(); // => ReferenceError superPower is not defined

    var named = function superPower() {
      console.log('Flying');
    };
  };

  // the same is true when the function name is the same as the variable name
  function example() {
    console.log(named); // => undefined

    named(); // => TypeError named is not a function

    var named = function named() {
      console.log('named');
    };
  };
  ```

<a name='guide-variables-14.4'></a>

- [14.4.](#guide-variables-14.4) Function declarations hoist their name and the function body.

  ```js
  function example() {
    superPower(); // => Flying

    function superPower() {
      console.log('Flying');
    };
  };
  ```

- For more information, refer to [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#toc)**

## Comparison Operators & Equality

<a name='guide-comparisonOperatorsEquality-15.1'></a>

- [15.1](#guide-comparisonOperatorsEquality-15.1) Use `===` and `!==` over `==` and `!=`.

  **eslint:** [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

<a name='guide-comparisonOperatorsEquality-15.2'></a>

- [15.2](#guide-comparisonOperatorsEquality-15.2) Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

  - **Objects** evaluate to **true**
  - **Undefined** evalues to **false**
  - **Null** evalues to **false**
  - **Booleans** evalues to **the value of the boolean**
  - **Numbers** evalue to **false** if **+0**, **-0**, or **Nan**, otherwise **true**
  - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

  ```js
  if ([0] && []) {
    // true
    // an array (even an empty one) is an object, objects will
    // evaluate to true
  };
  ```

<a name='guide-comparisonOperatorsEquality-15.3'></a>

- [15.3](#guide-comparisonOperatorsEquality-15.3) Use shortcuts for booleans, but explicit comparisons for strings and numbers.

  ```js
  // bad
  if (isValid === true) {
    // ...
  };

  // good
  if (isValid) {
    // ...
  };

  // bad
  if (name) {
    // ...
  };

  // good
  if (name !== '') {
    // ...
  };

  // bad
  if (collection.length) {
    // ...
  };

  // good
  if (collection.length > 0) {
    // ...
  };
  ```

<a name='guide-comparisonOperatorsEquality-15.4'></a>

- [15.4](#guide-comparisonOperatorsEquality-15.4) For more information, see [Truth Equality and JavaScript](https://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

<a name='guide-comparisonOperatorsEquality-15.5'></a>

- [15.5](#guide-comparisonOperatorsEquality-15.5) Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`).

  **eslint:** [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

  > Why? Lexical declarations are visible in the entire `switch` block, but only get initialized whena ssigned, whoch only happens when its `case` is reached. This causes problms when multiple `case` clauses attempt to define the same thing.

  ```js
  // bad
  switch (foo) {
    case 1:
      let x = 1;
      break;
    case 2:
      const y = 2;
      break;
    case 3:
      function f() {
        // ...
      };
      break;
    default:
      class C {};
  };

  // good
  switch (foo) {
    case 1: {
      let x = 1;
      break;
    }
    case 2: {
      const y = 2;
      break;
    }
    case 3: {
      function f() {
        // ...
      };
      break;
    }
    case 4: {
      bar();
      break;
    }
    default: {
      class C {};
    }
  };
  ```

<a name='guide-comparisonOperatorsEquality-15.6'></a>

- [15.6](#guide-comparisonOperatorsEquality-15.6) Ternaries should not be nested and generally be single line expressions.

  **eslint:** [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

  ```js
  // bad
  const foo = maybe1 > maybe2
    ? 'bar'
    : value1 > value2 ? 'baz' : null;

  // split into 2 separate ternary expressions
  const maybeNull = value1 > value2 ? 'baz' : null;

  // better
  const foo = maybe1 > maybe2
    ? 'bar'
    : maybeNull;

  // best
  const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
  ```

<a name='guide-comparisonOperatorsEquality-15.7'></a>

- [15.7](#guide-comparisonOperatorsEquality-15.7) Avoid unneeded ternary statements.

  **eslint:** [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

  ```js
  // bad
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;

  // good
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
  ```

<a name='guide-comparisonOperatorsEquality-15.8'></a>

- [15.8](#guide-comparisonOperatorsEquality-15.8) When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators (`+`, `-`, `*`, `/`) since their precendence is broadly understood.

  **eslint:** [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

  > Why? This improves readability and clarifies the developer's intention.

  ```js
  // bad
  const foo = a && b < 0 || c > 0 || d + 1 === 0;

  // bad
  const bar = a ** b - 5 % d;

  // bad
  // ony may be confused into thinking (a || b) && c
  if (a || b && c) {
    return d;
  };

  // good
  const foo = (a && b < 0) || c > 0 || (d + 1 === 0);

  // good
  const bar = (a ** b) - (5 % d);

  // good
  if (a || (b && c)) {
    return d;
  };

  // good
  const bar = a + b / c * d;
  ```

**[⬆ back to top](#toc)**

## Blocks

<a name='guide-blocks-16.1'></a>

- [16.1](#guide-blocks-16.1) Use braces with all multi-line blocks.

  **eslint:** [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

  ```js
  // bad
  if (test) {
    return false;
  };

  // good
  if (test) return false;

  // good
  if (test) {
    return false;
  };

  // bad
  function foo() { return false; };

  // good
  function bar() {
    return false;
  };
  ```

<a name='guide-blocks-16.2'></a>

- [16.2](#guide-blocks-16.2) If you're using multi-line blocks with `if` and `else`, put `else` on the same line as your `if` block's closing brace.

  **eslint:** [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

  **jscs:** [`disallowNewlinesBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

  ```js
  // bad
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  };

  // good
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  };
  ```

<a name='guide-blocks-16.3'></a>

- [16.3](#guide-blocks-16.3) If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks.

  **eslint:** [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

  ```js
  // bad
  function foo() {
    if (x) {
      return x;
    } else {
      return y;
    };
  };

  // good
  function foo() {
    if (x) {
      return x;
    };
    return y;
  };

  // bad
  function cats() {
    if (x) {
      return x;
    } else if (y) {
      return y;
    };
  };

  // good
  function cats() {
    if (x) {
      return x;
    };
    if (y) {
      return y;
    };
  };

  // bad
  function dogs() {
    if (x) {
      return x;
    } else {
      if (y) {
        return y;
      };
    };
  };

  // good
  function dogs() {
    if (x) {
      if (z) {
        return y;
      };
    };
    return z;
  };
  ```

**[⬆ back to top](#toc)**

## Control Statements

<a name='guide-controlStatements-17.1'></a>

- [17.1](#guide-controlStatements-17.1) In case your control statement (`if`, `while`, etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.

  > Why? Requiring operators at the beginning of the line keeps the operators aligned and dollows a pattern similar to method chaining. This also improvaes reaability by making it easier to visually follow complex logic.

  ```js
  // bad
  if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
    thing1();
  };

  // bad
  if (foo === 123
    && bar === 'abc') {
    thing1();
  };

  // bad
  if (
    foo == 123 &&
    bar === 'abc'
  ) {
    thing1();
  };

  // good
  if (
    foo == 123
    && bar === 'abc'
  ) {
    thing1();
  };

  // good
  if (
    (foo === 123 || bar === 'abc')
    && doesItLookGoodWhenItBecomesThatLong()
    && isThisReallyHappening()
  ) {
    thing1();
  };

  // good
  if (foo === 123 && bar === 'abc') {
    thing1();
  };
  ```

**[⬆ back to top](#toc)**

## Comments

<a name='guide-comments-18.1'></a>

- [18.1](#guide-comments-18.1) Use `/** ... */` for multi-line comments.

  ```js
  // bad
  // make() returns a new element
  // based on the passed in tag name
  //
  // @param {String} tag
  // @return {Element} element
  function make(tag) {
    // ...
    return element;
  };

  // good
  /**
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {
    // ...
    return element;
  };
  ```

<a name='guide-comments-18.2'></a>

- [18.2](#guide-comments-18.2) Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it's on the first line of a block.

  ```js
  // bad
  const acive = true; // is current tab

  // good
  // is current tab
  const active = true;

  // bad
  function getType() {
    console.log('fetching type...');
    // set the default type to 'no type'
    const type = this.type || 'notype';
    return type;
  };

  // good
  function getType() {
    console.log('fetching type...');

    // set the default type to 'no type'
    const type = this.type || 'no type';
    return type;
  };

  // also good
  function getType() {
    // set the default tyoe to 'no type'
    const type = this.type || 'no type';
    return type;
  };
  ```

<a name='guide-comments-18.3'></a>

- [18.3](#guide-comments-18.3) Start all comments with a space to make it easier to read.

  **eslint:** [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

  ```js
  // bad
  //is current tab
  const active = true;

  // good
  // is current tab
  const active = true;

  // bad
  /**
   *
   *make() returns a new element
   *based on the passed-in tag name
   */
  function make(tag) {
    //...
    return element;
  };

  // good
  /**
   *
   * make() returns a new element
   * based on the passed-in tag name
   */
  function make(tag) {
    // ...
    return element;
  };
  ```

<a name='guide-comments-18.4'></a>

- [18.4](#guide-comments-18.4) Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you're pointing our a problem that needs to be revisited, or if you're suggesting a solution to the problem that needs to be implemented. These are different than regular commends because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

<a name='guide-comments-18.5'></a>

- [18.5](#guide-comments-18.5) Use `// FIXME:` to annotate problems.

  ```js
  class Calculator extends Abacus {
    constructor() {
      super();

      // FIXME: shouldn't use a global here
      total = 0;
    };
  };
  ```

<a name='guide-comments-18.6'></a>

- [18.6](#guide-comments-18.6) Use `// TODO:` to annotate solutions to problems.

  ```js
  class Calculator extends Abacus {
    constructor() {
      super();

      // TODO: total should be configurable by an options param
      this.total = 0;
    };
  };
  ```

**[⬆ back to top](#toc)**

## Whitespace

<a name='guide-whitespace-19.1'></a>

- [19.1](#guide-whitespace-19.1) Use soft tabs (space character) set to 2 spaces.

  **eslint:** [`indent`](https://eslint.org/docs/rules/indent.html)

  **jscs:** [`validateIndentation`](http://jscs.info/rule/validateIndentation)

  ```js
  // bad
  function foo() {
  ••••let name;
  };

  // bad
  function bar() {
  •let name;
  };

  // good
  function baz() {
  ••let name;
  };
  ```

<a name='guide-whitespace-19.2'></a>

- [19.2](#guide-whitespace-19.2) Place 1 space before the leading brace.

  **eslint:** [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

  **jscs:** [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

  ```js
  // bad
  function test(){
    console.log('test');
  };

  // good
  function test() {
    console.log('test');
  };

  // bad
  dog.set('attr',{
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });

  // good
  dog.set('attr', {
    age: '1 year',
    breed: 'Bernese Mountain Dog',
  });
  ```

<a name='guide-whitespace-19.3'></a>

- [19.3](#guide-whitespace-19.3)  Place 1 space before the opening parentheses in control statements (`if`, `while`, etc.). Place no space between the argument list and the function name in function calls and declarations.

  **eslint:** [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

  **jscs:** [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

  ```js
  // bad
  if(isJedi) {
    fight ();
  };

  // good
  if (isJedi) {
    fight();
  };

  // bad
  function fight () {
    // ...
  };

  // good
  function fight() {
    // ...
  };
  ```

<a name='guide-whitespace-19.4'></a>

- [19.4](#guide-whitespace-19.4) Set off operators with spaces.

  **eslint:** [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

  **jscs:** [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

  ```js
  // bad
  const x=y+5;

  // good
  const x = y + 5;
  ```

<a name='guide-whitespace-19.5'></a>

- [19.5](#guide-whitespace-19.5) End files with a single newline characters.

  **eslint:** [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

  ```js
  // bad
  import { es6 } from './UltrasoundStyleGuide';
  // ...
  export default es6;

  // bad
  import { es6 } from './UltrasoundStyleGuide';
  // ...
  export default es6;↵
  ↵

  // good
  import { es6 } from './UltrasoundStyleGuide';
  // ...
  export default es6;↵
  ```

<a name='guide-whitespace-19.6'></a>

- [19.6](#guide-whitespace-19.6) Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which emphasizes that the line is a method call, not a new statement.

  **eslint:** [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call), [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

  ```js
  // bad
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // bad
  $('#items').
    find('.selected').
      highlight().
    end().
  find('.open').
    updateCount();

  // good
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // bad
  const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
    .attr('width', (radius + margin) * 2).append('svg:g')
    .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
    .call(tron.led);

  // good
  const leds = stage.selectAll('.led')
      .data(data)
    .enter().append('svg:svg')
      .classed('led', true)
      .attr('width', (radius + margin) * 2)
    .append('svg:g')
      .attr('transform', `translate(${radius + margin}, ${radius + margin})`)
      .call(tron.led);

  // good
  const leds = stage.selectAll('.led').data(data);
  ```

<a name='guide-whitespace-19.7'></a>

- [19.7](#guide-whitespace-19.7) Leave a blank line after blocks and before the next statement.

  **jscs:** [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

  ```js
  // bad
  if (foo) {
    return bar;
  };
  return baz;

  // good
  if (foo) {
    return bar;
  };

  return baz;

  // bad
  const obj = {
    foo() {
    },
    bar() {
    },
  };
  return obj;

  // good
  const obj = {
    foo() {
    },

    bar() {
    },
  };

  return obj;

  // bad
  const arr = [
    function foo() {
    },
    function bar() {
    },
  ];
  return arr;

  // good
  const arr = [
    function foo() {
    },

    function bar() {
    },
  ];

  return arr;
  ```

<a name='guide-whitespace-19.8'></a>

- [19.8](#guide-whitespace-19.8) Do not pad your blocks with blank lines.

  **eslint:** [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

  **jscs:** [`disallowPaddingNewlinesInblocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

  ```js
  // bad
  function bar() {

    console.log(foo);

  };

  // bad
  if (baz) {

    console.log(qux);
  } else {
    console.log(foo);

  };

  // bad
  class Foo {

    constructor(bar) {
      this.bar = bar;
    };
  };

  // good
  function bar() {
    console.log(foo);
  };

  // good
  if (baz) {
    console.log(qux);
  } else {
    console.log(foo);
  };
  ```

<a name='guide-whitespace-19.9'></a>

- [19.9](#guide-whitespace-19.9) Do not add spaces inside parentheses.

  **eslint:** [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

  **jscs:** [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

  ```js
  // bad
  function bar( foo ) {
    return foo;
  };

  // good
  function bar(foo) {
    return foo;
  };

  // bad
  if ( foo ) {
    console.log(foo);
  };

  // good
  if (foo) {
    console.log(foo);
  };
  ```

<a name='guide-whitespace-19.10'></a>

- [19.10](#guide-whitespace-19.10) Do not add spaces inside brackets.

  **eslint:** [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

  **jscs:** [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

  ```js
  // bad
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // good
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

<a name='guide-whitespace-19.11'></a>

- [19.11](#guide-whitespace-19.11) Add spaces inside curly braces.

  **eslint:** [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

  **jscs:** [`requireSpacesInsideObjectBrakcets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

  ```js
  // bad
  const foo = {clark: 'kent'};

  // good
  const foo = { clark: 'kent' };
  ```

<a name='guide-whitespace-19.12'></a>

- [19.12](#guide-whitespace-19.12) Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#guide-strings-6.2), long strings are exempt from this rule, and should not be broken up.

  **eslint:** [`max-len`](https://eslint.org/docs/rules/max-len.html)

  **jscs:** [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

  > Why? This ensures readability and maintainability.

  ```js
  // bad
  const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

  // good
  const foo = jsonData
    && jsonData.foo
    && jsonData.foo.bar
    && jsonData.foo.bar.baz
    && jsonData.foo.bar.baz.quux
    && jsonData.foo.bar.baz.quux.xyzzy;

  // bad
  $.ajax({ method: 'POST', url: 'https://ultrasound.tunerinc.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

  // good
  $.ajax({
    method: 'POST',
    url: 'https://ultrasound.tunerinc.com/',
    data: { name: 'John' }
  })
    .done(() => console.log('Congratulations!'))
    .fail(() => console.log('You have failed this city.'));
  ```

**[⬆ back to top](#toc)**

## Commas

<a name='guide-commas-20.1'></a>

- [20.1](#guide-commas-20.1) Leading commas: **Nope.**

  **eslint:** [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

  **jscs:** [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

  ```js
  // bad
  const story = [
      once
    , upon
    , aTime
  ];

  // good
  const story = [
    once,
    upon,
    aTime,
  ];

  // bad
  const hero = {
      firstName: 'Ada'
    , lastName: 'Lovelace'
    , birthYear: 1815,
    , superPower: 'computers'
  };

  // good
  const hero = {
    firstName: 'Ada',
    lastName: 'Lovelace',
    birthYear: 1815,
    superPower: 'computers',
  };
  ```

<a name='guide-commas-20.2'></a>

- [20.2](#guide-commas-20.2) Additional trailing comma: **Yup.**

  **eslint:** [`comma-dangle`](https://eslint.org/docs/rules/comma-dangle.html)

  **jscs:** [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)

  > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas) in legacy browsers.

  ```diff
  // bad - git diff without trailing comma
  const hero = {
       firstName: 'Florence',
  -    lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing']
  };

  // good
  git diff with trailing comma
  const hero = {
       firstName: 'Florence',
       lastName: 'Nightingale',
  +    inventorOf: ['coxcomb chart', 'modern nursing'],
  };
  ```

  ```js
  // bad
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // good
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];

  // bad
  function createHero(
    firstName,
    lastName,
    inventorOf
  ) {
    // does nothing
  };

  // good
  function createHero(
    firstName,
    lastName,
    inventorOf,
  ) {
    // does nothing
  };

  // good (note that a comma must not appear after a "rest" element)
  function createHero(
    firstName,
    lastName,
    inventorOf,
    ...heroArgs
  ) {
    // does nothing
  };

  // bad
  createHero(
    firstName,
    lastName,
    inventorOf
  );

  // good
  createHero(
    firstName,
    lastName,
    inventorOf,
  );

  // good (note that a comma must not appear after a "rest" element)
  createHero(
    firstName,
    lastName,
    inventorOf,
    ...heroArgs,
  );
  ```

**[⬆ back to top](#toc)**

## Semicolons

<a name='guide-semicolons-21.1'></a>

- [21.1](#guide-semicolons-21.1) **Yup.**

  **eslint:** [`semi`](https://eslint.org/docs/rules/semi.html)

  **jscs:** [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)

  > Why? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether or not it should regard that line break as the end of a statement, and (as the name implies) place a semicolon into your code before the line break if it thinks so. ASI contains a few eccentric behaviors, though, and your code will break if JavaScript misinterprets your line break. These rules will become more complicated as new features become a part of JavaScript. Explicitly terminating your statements and configuring your linter to catch missing semicolons will help prevent you from encountering issues.

  ```js
  // bad - raises exception
  const luke = {}
  const leia = {}
  [luke, leia].forEach(jedi => jedi.father = 'vader')

  // good
  const luke = {};
  const leia = {};
  [luke, leia].forEach(jedi => {
    jedi.father = 'vader';
  });

  // bad - raises exception
  const reaction = 'No! That is impossible!'
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2`, `c3po`
    // ...
  }())

  // good
  const reaction = 'No! That is impossible!';
  (async function meanwhileOnTheFalcon() {
    // handle `leia`, `lando`, `chewie`, `r2  , `c3po`
    // ...
  }());

  // bad - return `undefined` instead of the value on the next line - always happens whne `return` is on a line by itself  because of ASI!
  function foo() {
    return
      'search your feelings, you know it to be foo'
  };

  // good
  function foo() {
    return 'search your feelings, you know it to be foo';
  };
  ```

  [Read more](https://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214#7365214)

**[⬆ back to top](#toc)**

## Type Casting & Coercion

<a name='guide-typeCastingCoercion-22.1'></a>

- [22.1](#guide-typeCastingCoercion-22.1) Perform type coercion at the beginning of the statement.

<a name='guide-typeCastingCoercion-22.2'></a>

- [22.2](#guide-typeCastingCoercion-22.2) Strings:

  **eslint:** [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```js
  // => this.reviewScore = 9;

  // bad
  const totalScore = new String(this.reviewScore); // typeof totalScore is "object" not "string"

  // bad
  const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

  // bad
  const totalScore = this.reviewScore.toString(); // isn't guaranteed to return a string

  // good
  const totalScore = String(this.reviewScore);
  ```

<a name='guide-typeCastingCoercion-22.3'></a>

- [22.3](#guide-typeCastingCoercion-22.3) Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing string.

  **eslint:** [`radix`](https://eslint.org/docs/rules/radix), [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```js
  const inputValue = '4';

  // bad
  const val = new Number(inputValue);

  // bad
  const val = +inputValue;

  // bad
  const val = inputValue >> 0;

  // bad
  const val = parseInt(inputValue);

  // good
  const val = Number(inputValue);

  // good
  const val = parseInt(inputValue, 10);
  ```

<a name='guide-typeCastingCoercion-22.4'></a>

- [22.4](#guide-typeCastingCoercion-22.4) If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](https://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

  ```js
  // good
  /**
   * parseInt was the reason my code was slow.
   * Bitshifting the String to coerce it to a
   * Number made it a lot faster.
   */
   const val = inputValue >> 0;
  ```

<a name='guide-typeCastingCoercion-22.5'></a>

- [22.5](#guide-typeCastingCoercion-22.5) **Note:** Be careful when using bitshift operators. Numbers are represented as [64-bit values](https://es5.github.io/#x4.3.19), but bitshift operators always return a 32-bit integer ([source](https://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Larfest signed 32-bit Int is 2,147,483,647:

  ```js
  2147483647 >> 0; // => 2147483647
  2147483648 >> 0; // => -2147483648
  2147483649 >> 0; // => -2147483647
  ```

<a name='guide-typeCastingCoercion-22.6'></a>

- [22.6](#guide-typeCastingCoercion-22.6) Booleans:

  **eslint:** [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

  ```js
  const age = 0;

  // bad
  const hasAge = new Boolean(age);

  // good
  const hasAge = Booelean(age);

  // best
  const hasAge = !!age;
  ```

**[⬆ back to top](#toc)**

## Naming Conventions

<a name='guide-namingConventions-23.1'></a>

- [23.1](#guide-namingConventions-23.1) Avoid single letter names. Be descriptive with your naming.

  **eslint:** [`id-length`](https://eslint.org/docs/rules/id-length)

  ```js
  // bad
  function q() {
    // ...
  };

  // good
  function query() {
    // ...
  };
  ```

<a name='guide-namingConventions-23.2'></a>

- [23.2](#guide-namingConventions-23.2) Use camelCase when naming objects, functions, and instances.

  **eslint:** [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

  **jscs:** [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

  ```js
  // bad
  const OBJEcttsssss = {};
  const this_is_my_object = {};
  function c() {};

  // good
  const thisIsMyObject = {};
  function thisIsMyFunction() {};
  ```

<a name='guide-namingConventions-23.3'></a>

- [23.3](#guide-namingConventions-23.3) Use PascalCase only when naming constructors or classes.

  **eslint:** [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

  **jscs:** [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

  ```js
  // bad
  function user(options) {
    this.name = options.name;
  };

  // good
  class User {
    constructor(options) {
      this.name = options.name;
    };
  };

  // bad
  const bad = new user({
    name: 'nope',
  });

  // good
  const good = new User({
    name: 'yup',
  });
  ```

<a name='guide-namingConventions-23.4'></a>

- [23.4](#guide-namingConventions-23.4) Do not use trailing or leading underscores.

  **eslint:** [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

  **jscs:** [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)

  > Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean 'private,' in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won't count as breaking, or that tests aren't needed. **tl;dr:** if you want something to be 'private,' it must not be oberservably present.

  ```js
  // bad
  this.__firstName__ = 'Panda';
  this.firstName_ = 'Panda';
  this._firstName = 'Panda';

  // good
  this.firstName = 'Panda';
  ```

<a name='guide-namingConventions-23.5'></a>

- [23.5](#guide-namingConventions-23.5) Don't save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

  **jscs:** [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

  ```js
  // bad
  function foo() {
    const self = this;
    return function () {
      console.log(self);
    };
  };

  // bad
  function foo() {
    const that = this;
    return function () {
      console.log(that);
    };
  };

  // good
  function foo() {
    return () => {
      console.log(this);
    };
  };
  ```

<a name='guide-namingConventions-23.6'></a>

- [23.6](#guide-namingConventions-23.6) A base filename should exactly match the name of its default export.

  ```js
  // file 1 contents
  class CheckBox {
    // ...
  };
  export default CheckBox;

  // file 2 contents
  export default function fortyTwo() { return 42; };

  // file 3 contents
  export default function insideDirectory() {};

  // in some other file
  // bad
  import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
  import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
  import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

  // bad
  import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
  import forty_two from './forty_two'; // snake_case import/filename, camelCase export
  import inside_directory from './inside_directory'; // snake_case import, camelCase export
  import index from './inside_directory/index'; // requiring the index file explicitly
  import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

  // good
  import CheckBox from './CheckBox'; // PascaleCase export/import/filename
  import fortyTwo from './fortyTwo'; // camelCase export/import/filename
  import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
  // ^ supports both isnideDirectory.js and insideDirectory/index.js
  ```

<a name='guide-namingConventions-23.7'></a>

- [23.7](#guide-namingConventions-23.7) Use camelCase when you export-default a function. Your filename should be identical to your function's name.

  ```js
  function makeStyleGuide() {
    // ...
  };

  export default makeStyleGuide;
  ```

<a name='guide-namingConventions-23.8'></a>

- [23.8](#guide-namingConventions-23.8) Use PascalCase when you export a constructor / class / singleton / function library / bare object.

  ```js
  const UltrasoundStyleGuide = {
    es6: {
    },
  };

  export default UltrasoundStyleGuide;
  ```

<a name='guide-namingConventions-23.9'></a>

- [23.9](#guide-namingConventions-23.9) Acronyms and initialisms should always be all capitalized, or all lowercased.

  > Why? Names are for readability, not to appease a computer algorithm.

  ```js
  // bad
  import SmsContainer from './containers/SmsContainer';

  // bad
  const HttpRequests = [
    // ...
  ];

  // good
  import SMSContainers from './containers/SMSContainer';

  // good
  const HTTPRequests = [
    // ...
  ]l

  // also good
  const httpRequests = [
    // ...
  ];

  // best
  import TextMessageContainer from './containers/TextMessageContainer';

  // best
  const requests = [
    // ...
  ];
  ```

**[⬆ back to top](#toc)**

## Accessors

<a name='guide-accessors-24.1'></a>

- [24.1](#guide-accessors-24.1) Accessor functions for properties are not required.

<a name='guide-accessors-24.2'></a>

- [24.2](#guide-accessors-24.2) Do not use JavaScript getters/setters as they cause unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, use `getVal()` and `setVal('hello')`.

  ```js
  // bad
  class Dragon {
    get age() {
      // ...
    };

    set age(value) {
      // ...
    };
  };

  // good
  class Dragon {
    getAge() {
      // ...
    };

    setAge(value) {
      // ...
    };
  };
  ```

<a name='guide-accessors-24.3'></a>

- [24.3](#guide-accessors-24.3) If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

  ```js
  // bad
  if (!dragon.age()) {
    return false;
  };

  // good
  if (!dragon.hasAge()) {
    return false;
  };
  ```

<a name='guide-accessors-24.4'></a>

- [24.4](#guide-accessors-24.4) It's okay to create `get()` and `set()` functions, but be consistent.

  ```js
  class Jedi {
    constructor(opts = {}) {
      const lightsaber = opts.lightsaber || 'blue';
      this.set('lightsaber', lightsaber);
    };

    set(key, val) {
      this[key] = val;
    };

    get(key) {
      return this[key];
    };
  };
  ```

**[⬆ back to top](#toc)**

## Events

<a name='guide-events-25.1'></a>

- [25.1](#guide-events-25.1) When attaching data payloads to events (whehter DOM events or something more proprietary like Backbone events), pass an object literal (also known as a "hash") instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

  ```js
  // bad
  $(this).trigger('listingUpdated', listing.id);

  // ...

  $(this).on('listingUpdated', (e, listingID) => {
    // do something with listingID
  });
  ```

  prefer:

  ```js
  // good
  $(this).trigger('listingUpdated', { listingID: listing.id });

  // ...

  $(this).on(listingUpdated, (e, data) => {
    // do something with data.listingID
  });
  ```

**[⬆ back to top](#toc)**

## jQuery

<a name='guide-jquery-26.1'></a>

- [26.1](#guide-jquery-26.1) Prefix jQuery object variables with `$`.

  **jscs:** [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)

  ```js
  // bad
  const sidebar = $('.sidebar');

  // good
  const $sidebar = $('.sidebar');

  // good
  const $sidebarBtn = $('.sidebar-btn');
  ```

<a name='guide-jquery-26.2'></a>

- [26.2](#guide-jquery-26.2) Cache jQuery lookups.

  ```js
  // bad
  function setSidebar() {
    $('.sidebar').hide();

    // ...

    $('.sidebar').css({
      'background-color': 'pink',
    });
  };

  // good
  function setSidebar() {
    const $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...

    $sidebar.css({
      'background-color': 'pink',
    });
  };
  ```

<a name='guide-jquery-26.3'></a>

- [26.3](#guide-jquery-26.3) For DOM queries, use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

<a name='guide-jquery-26.4'></a>

- [26.4](#guide-jquery-26.4) Use `find` with scoped jQuery object queries.

  ```js
  // bad
  $('ul', '.sidebar').hide();

  // bad
  $('.sidebar').find('ul').hide();

  // good
  $('.sidebar ul').hide();

  // good
  $('.sidebar > ul').hide();

  // good
  $sidebar.find('ul').hide();
  ```

**[⬆ back to top](#toc)**

## ECMAScript 5 Compatibility

<a name='guide-ecma5Compatibility-27.1'></a>

- [27.1](#guide-ecma5Compatibility-27.1) Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](https://kangax.github.io/es5-compat-table/)

**[⬆ back to top](#toc)**

## ECMAScript 6+ (ES2015+) Styles

<a name='guide-ecma6ES2015Styles-28.1'></a>

- [28.1](#guide-ecma6ES2015Styles-28.1) This is a collection of links to the various ES6+ features.

  1. [Arrow Functions](#guide-arrowFunctions-8.1)
  1. [Classes](#guide-classes-9.1)
  1. [Object Shorthand](#guide-objects-3.3)
  1. [Object Concise](#guide-objects-3.4)
  1. [Object Computed Properties](#guide-objects-3.2)
  1. [Tempalte Strings](#guide-string-6.3)
  1. [Destructuring](#guide-destructuring-5.1)
  1. [Default Parameters](#guide-functions-7.7)
  1. [Rest](#guide-functions-7.6)
  1. [Array Spreads](#guide-arrays-4.3)
  1. [Let & Const](#guide-references-2.1)
  1. [Exponentiation Operator](#guide-properties-12.3)
  1. [Iterators & Generators](#guide-iteratorsGenerators-11.1)
  1. [Modules](#guide-modules-10.1)

<a name='guide-ecma6ES2015Styles-28.2'></a>

- [28.2](#guide-ecma6ES2015Styles-28.2) Do not use [TC39 proposals](https://github.com/tc39/proposals) that have not reached stage 3.

  > Why? [They are not finalized](https://tc39.github.io/process-document/), and they are subject to change or to be withdrawn entirely. We want to use JavaScript, and proposals are not JavaScript yet.

**[⬆ back to top](#toc)**

## Standard Library

The [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects) contains utilities that are functionally broken but remain for legacy reasons.

<a name='guide-standardLibrary-29.1'></a>

- [29.1](#guide-standardLibrary-29.1) Use `Number.isNaN` instead of global `isNaN`.

  **eslint:** [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > Why? The global `isNaN` coerces non-numbers to numbers, returning `true` for anything that coerces to `NaN`. If this behavior is desired, make it explicit.

  ```js
  // bad
  isNaN('1.2'); // false
  isNaN('1.2.3'); // true

  // good
  Number.isNaN('1.2.3'); // false
  Number.isNaN(Number('1.2.3')); // true
  ```

<a name='guide-standardLibrary-29.2'></a>

- [29.2](#guide-standardLibrary-29.2) Use `Number.isFinite` instead of global `isFinite`.

  **eslint:** [`no-restricted-globals`](https://eslint.org/docs/rules/no-restricted-globals)

  > Why? The global `isFinite` coerces non-numbers to numbers, returning `true` for anything that coerces toa  finite number. If this behavior is desired, make it explicit.

  ```js
  // bad
  isFinite('2e3'); // false

  // good
  Number.isFinite('2e3'); // false
  Number.isFinite(parseInt('2e3', 10)); // true
  ```

**[⬆ back to top](#toc)**

## Testing

<a name='guide-testing-30.1'></a>

- [30.1](#guide-testing-30.1) **Yup.**

  ```js
  function foo() {
    return true;
  };
  ```

<a name='guide-testing-30.2'></a>

- [30.2](#guide-testing-30.2) **No, but seriously:**

  - Whichever testing framework you use, you should be writing tests!
  - Strive to write many small pure functions, and minimize where mutations occur.
  - Be cautious about stubs and mocks - they can make your tests more brittle.
  - We primarily use [`mocha`](https://www.npmjs.com/package/mocha) at Ultrasound. [`tape`](https://www.npmjs.com/package/tape) is also used occasionally for small, separate modules.
  - 100% test coverage is a good goal to strive for, even it it's not always practical to reach it.
  - Whenever you fix a bug, *write a regresssion test*. A bug fixed without a regression test is almost certainly going to break again in the future.

**[⬆ back to top](#toc)**

## Performance

<a name='guide-performance-31'></a>

- [On Layout & Web Performance](https://www.kellegous.com/j/2013/01/26/layout-performance/)
- [String vs Array Concat](https://jsperf.com/string-vs-array-concat/2)
- [Try/Catch Cost In a Loop](https://jsperf.com/try-catch-in-loop-cost)
- [Bang Function](https://jsperf.com/bang-function)
- [jQuery Find vs Context, Selector](https://jsperf.com/jquery-find-vs-context-sel/13)
- [innerHTML vs textContext for script text](https://jsperf.com/innerhtml-vs-textcontent-for-script-text)
- [Long String Concatenation](https://jsperf.com/ya-string-concat)
- [Are Javascript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
- Loading...

**[⬆ back to top](#toc)**

## Resources

<a name='guide-resources-32'></a>

#### Learning ES6+

- [Latest ECMA spec](https://tc39.github.io/ecma262/)
- [ExploringJS](http://exploringjs.com/)
- [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
- [Comprehensive Overview of ES6 Features](http://es6-features.org/)

#### Read This

- [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

#### Tools

- Code Style Linters
  - [ESLint](https://eslint.org/) - [Airbnb Style `.eslintrc`](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
  - [JSHint](http://jshint.com/) - [Airbnb Style `.jshintrc`](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
  - [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json) (Deprecated, please use [ESLint](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base))
- Neutrino preset - [neutrino-preset-airbnb-base](https://neutrino.js.org/presets/neutrino-preset-airbnb-base/)

#### Other Style Guides

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript/blob/master/README.md)
- [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
- [jQuery Core Style Guidelines](https://contribute.jquery.org/style-guide/js/)
- [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

#### Other Styles

- [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
- [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
- [Popular JavaScript Coding Conventions on GitHub](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
- [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

#### Further Reading

- [Understanding JavaScript Closures](https://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
- [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
- [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
- [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
- [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

#### Books

- [JavaScript: The Good Parts](https://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
- [JavaScript Patterns](https://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
- [Pro JavaScript Design Patterns](https://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X) - Ross Jarmes & Dustin Diaz
- [High Performance Web Sites: Essential Knowledge for Front-End Engineers](https://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
- [Maintainable JavaScript](https://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
- [JavaScript Web Applications](https://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
- [Pro JavaScript Techniques](https://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
- [Smashing Node.js: JavaScript Everywhere](https://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
- [Secrets of the JavaScript Ninja](https://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig & Bear Bibeault
- [Human JavaScript](http://humanjavascript.com/) - Jenrik Joreteg
- [Superhero.js](http://superherojs.com/) - Jim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
- [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
- [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar & Anton Kovalyov
- [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
- [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
- [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

#### Blogs

- [JavaScript Weekly](http://javascriptweekly.com/)
- [JavaScript, JavaScript...](https://javascriptweblog.wordpress.com/)
- [Bocoup Weblog](https://bocoup.com/weblog)
- [Adequately Good](http://www.adequatelygood.com/)
- [NCZOnline](https://www.nczonline.net/)
- [Perfection Kills](http://perfectionkills.com/)
- [Ben Alman](http://benalman.com/)
- [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
- [nettuts](http://code.tutsplus.com/?s=javascript)

#### Podcasts

- [JavaScript Air](https://javascriptair.com/)
- [JavaScript Jabber](https://devchat.tv/js-jabber/)

**[⬆ back to top](#toc)**

## The JavaScript Style Guide Guide

<a name='guide-jsStyleGuideGuide-33'></a>

  > We would like to thank the folks over at [Airbnb](https://github.com/airbnb) for writing such an amazing [style guide](https://github.com/airbnb/javascript/blob/master/README.md). We've modified small things here and there with which we respectfully disagree with.
  >
  > We're releasing it under the MIT license, so please feel free to form and use at your will. We don't expect everyone to agree with the way we do things either, but we do hope this can help kick start your own style guide as a template or map of some sort just as Airbnb did for us. Hope it helps!

## Contributors

<a name='guide-contributors-34'></a>

- [View Contributors](https://github.com/tunerinc/ultrasound/graphs/contributors)

## License

<a name='guide-license-35'></a>

```text
MIT License

Copyright (c) 2018 Tuner, Inc

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```