# Ultrasound React/JSX Style Guide

*A mostly reasonable approach to React & JSX*

## Table of Contents

<a name='toc'></a>

1. [Basic Rules](#guide-basic)
1. [Class vs `React.createClass` vs stateless](#guide-classReactStateless)
1. [Mixins](#guide-mixins)
1. [Naming](#guide-naming)
1. [Declaration](#guide-declaration)
1. [Alignment](#guide-alignment)
1. [Quotes](#guide-quotes)
1. [Spacing](#guide-spacing)
1. [Props](#guide-props)
1. [Refs](#guide-refs)
1. [Parentheses](#guide-parentheses)
1. [Tags](#guide-tags)
1. [Methods](#guide-methods)
1. [Ordering](#guide-ordering)
1. [`isMounted`](#guide-isMounted)

## Basic Rules

<a name='guide-basic'></a>

<a name='guide-basic-1.1'></a>

- [1.1](#guide-basic-1.1) Only include one React component per file.

  - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file.

    **eslint:** [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless)

  - Always use JSX syntax.
  - Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.

**[⬆ back to top](#toc)**

## Class vs `React.createClass` vs stateless

<a name='guide-classReactStateless'></a>

<a name='guide-classReactStateless-2.1'></a>

- [2.1](#guide-classReactStateless-2.1) If you have internal state and/or refs, prefer `class extends React.Component` over `React.createClass`.

  **eslint:** [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md), [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

  ```jsx
  // bad
  const Listing = React.createClass({
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    };
  });

  // good
  class Listing extends React.Component {
    // ...
    render() {
      return <div>{this.state.hello}</div>;
    };
  };
  ```

  And if you don't have state or refs, prefer normal function (not arrow functions) over classes:

  ```jsx
  // bad
  class Listing extends React.Component {
    render() {
      return <div>{this.props.hello}</div>;
    };
  };

  // bad (replying on function name inference is discouraged)
  const Listing = ({ hello }) => {
    <div>{hello}</div>
  };

  // good
  function Listing({ hello }) {
    return <div>{hello}</div>;
  };
  ```

**[⬆ back to top](#toc)**

## Mixins

<a name='guide-mixins'></a>

<a name='guide-mixins-3.1'></a>

- [3.1](#guide-mixins-3.1) [Do not use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > Why? Mixins introduce implicit dependences, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

**[⬆ back to top](#toc)**

## Naming

<a name='guide-naming'></a>

<a name='guide-naming-4.1'></a>

- [4.1](#guide-naming-4.1) **Extensions:** Use `.jsx` extends for React compenents.

<a name='guide-naming-4.2'></a>

- [4.2](#guide-naming-4.2) **Filename:** Use PascalCase for filenames. E.g., `ReservationCard.jsx`.

<a name='guide-naming-4.3'></a>

- [4.3](#guide-naming-4.3) **Reference Naming:** Use PascalCase for React compnents and camelCase for their instances.

  **eslint:** [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

  ```jsx
  // bad
  import reservationCard from './ReservationCard';

  // good
  import ReservationCard from './ReservationCard';

  // bad
  const ReservationItem = <ReservationCard />;

  // good
  const reservationItem = <ReservationCard />;
  ```

<a name='guide-naming-4.4'></a>

- [4.4](#guide-naming-4.4) **Component Naming:** Use the filename as the component name. For example, `ReservationCard.jsx` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name.

  ```jsx
  // bad
  import Footer from './Footer/Footer';

  // bad
  import Footer from './Footer/index';

  // good
  import Footer from './Footer';
  ```

<a name='guide-naming-4.5'></a>

- [4.5](#guide-naming-4.5) **Higher-order Component Naming:** Use a composite of a higher-order component's name and the passed-in component's name as the `displayName` on the gernated component. For example, the higher-order component `withFoo()`, when passed a component `Bar` should produce a component with a `displayName` of `withFoo(Bar)`.

  > Why? A component's `displayName` may be used by developer tools or in error messages, and have a value that clearly expresses this relationship helps people understand what is happening.

  ```jsx
  // bad
  export default function withFoo(WrappedComponent) {
    return function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    };
  };

  // good
  export default function withFoo(WrappedComponent) {
    function WithFoo(props) {
      return <WrappedComponent {...props} foo />;
    }

    const wrappedComponentName = WrappedComponent.displayName
      || WrappedComponent.name
      || 'Component';

    WithFoo.displayName = `withFoo(${wrappedComponentName})`;
    return WithFoo;
  };
  ```

<a name='guide-naming-4.6'></a>

- [4.6](#guide-naming-4.6) **Props Naming:** Avoid using DOM component prop names for different purposes.

  > Why? People expect props like `style` and `className` to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

  ```jsx
  // bad
  <MyComponent style="fancy" />

  // bad
  <MyComponent className="fancy" />

  // good
  <MyComponent variant="fancy" />
  ```

**[⬆ back to top](#toc)**

## Declaration

<a name='guide-declaration'></a>

<a name='guide-declaration-5.1'></a>

- [5.1](#guide-declaration-5.1) Do not use `displayName` for naming components. Instead, name the component by reference.

  ```jsx
  // bad
  export default React.createClass({
    displayName: 'ReservationCard',
    // stuff goes here
  });

  // good
  export default class ReservationCard extends React.Component {
  };
  ```

**[⬆ back to top](#toc)**

## Alignment

<a name='guide-alignment'></a>

<a name='guide-alignment-6.1'></a>

- [6.1](#guide-alignment-6.1) Follow these alignment styles for JSX syntax.

  **eslint:** [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md), [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

  ```jsx
  // bad
  <Foo superLongParam="bar"
       anotherSuperLongParam="baz" />

  // good
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  />

  // if props fit in one line, then keep it on the same line
  <Foo bar="bar" />

  // children get indented normally
  <Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
  >
    <Quux />
  </Foo>
  ```

**[⬆ back to top](#toc)**

## Quotes

<a name='guide-quotes'></a>

<a name='guide-quotes-7.1'></a>

- [7.1](#guide-quotes-7.1) Always use double quotes (`"`) for JSX attributes, but single quotes(`'`) for all other JS.

  **eslint:** [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

  > Why? Regular HTML attributes also tpically use double quotes instead of single, so JSX attributes mirror this convention.

  ```jsx
  // bad
  <Foo bar='bar' />

  // good
  <Foo bar="bar" />

  // bad
  <Foo style={{ left: "20px" }} />

  // good
  <Foo style={{ left: '20px' }} />
  ```

**[⬆ back to top](#toc)**

## Spacing

<a name='guide-spacing'></a>

<a name='guide-spacing-8.1'></a>

- [8.1](#guide-spacing-8.1) Always include a single space in your self-closing tag.

  **eslint:** [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

  ```jsx
  // bad
  <Foo/>

  // very bad
  <Foo

  // bad
  <Foo
   />

  // good
  <Foo />
  ```

<a name='guide-spacing-8.2'></a>

- [8.2](#guide-spacing-8.2) Do not pad JSX curly braces with spaces.

  **eslint:** [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

  ```jsx
  // bad
  <Foo bar={ baz } />

  // good
  <Foo bar={baz} />
  ```

**[⬆ back to top](#toc)**

## Props

<a name='guide-props'></a>

<a name='guide-props-9.1'></a>

- [9.1](#guide-props-9.1) Always use camelCase for prop names.

  ```jsx
  // bad
  <Foo
    UserName="hello"
    phone_number={12345678}
  />

  // good
  <Foo
    userName="hello"
    phoneNumber={12345678}
  />
  ```

<a name='guide-props-9.2'></a>

- [9.2](#guide-props-9.2) Omit the value of the prop when it is explicitly `true`.

  **eslint:** [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

  ```jsx
  // bad
  <Foo
    hidden={true}
  />

  // good
  <Foo
    hidden
  />

  // good
  <Foo hidden />
  ```

<a name='guide-props-9.3'></a>

- [9.3](#guide-props-9.3) Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`.

  **eslint:** [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

  ```jsx
  // bad
  <img src="hello.jpg" />

  // good
  <img src="hello.jpg" alt="Me waving hello" />

  // good
  <img src="hello.jpg" alt="" />

  // good
  <img src="hello.jpg" role="presentation" />
  ```

<a name='guide-props-9.4'></a>

- [9.4](#guide-props-9.4) Do not use words like "image", "photo", or "picture" in `<img>` `alt` props.

  **eslint:** [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

  > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

  ```jsx
  // bad
  <img src="hello.jpg" alt="Picture of me waving hello" />

  // good
  <img src="hello.jpg" alt="Me waving hello" />
  ```

<a name='guide-props-9.5'></a>

- [9.5](#guide-props-9.5) Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/roles#role_definitions).

  **eslint:** [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

  ```jsx
  // bad - not an ARIA role
  <div role="datepicker" />

  // bad - abstract ARIA role
  <div role="range" />

  // good
  <div role="button" />
  ```

<a name='guide-props-9.6'></a>

- [9.6](#guide-props-9.6) Do not use `accessKey` on elements.

  **eslint:** [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

  > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people using screenreaders and keyboard complicate acecssibility.

  ```jsx
  // bad
  <div accessKey="h" />

  // good
  <div />
  ```

<a name='guide-props-9.7'></a>

- [9.7](#guide-props-9.7) Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

  ```jsx
  // bad
  {todos.map((todo, index) =>
    <Todo
      {...todo}
      key={index}
    />
  )}

  // good
  {todos.map(todo =>
    <Todo
      {...todo}
      key={todo.id}
    />
  )}
  ```

<a name='guide-props-9.8'></a>

- [9.8](#guide-props-9.8) Always define explicit defaultProps for all non-required props.

  > Why? propTypes are a form of documentation, and procuding defaultProps means the reader of your code doesn't have to assume as much. In addition, it can mean that your code can omit certain type checks.

  ```jsx
  // bad
  function SFC({ oo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  };
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };

  // good
  function SFC({ foo, bar, children }) {
    return <div>{foo}{bar}{children}</div>;
  };
  SFC.propTypes = {
    foo: PropTypes.number.isRequired,
    bar: PropTypes.string,
    children: PropTypes.node,
  };
  SFC.defaultProps = {
    bar: '',
    children: null,
  };
  ```

<a name='guide-props-9.9'></a>

- [9.9](#guide-props-9.9) Use spread props sparingly

  > Why? Otherwise, you're more likely to pass unnecessary props down to components. And for React c15.6.1 and older, you could [pass invalid HTML attrivutes to the DOM](https://reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

**Exceptions:**

<a name='guide-props-9.10'></a>

- [9.10](#guide-props-9.10) HOCs that proxy down props and hoist propTypes

  ```jsx
  function HOC(WrappedComponent) {
    return class Proxy extends React.Component {
      Proxy.propTypes = {
        text: PropTypes.string,
        isLoading: PropTypes.bool,
      };

      render() {
        return <WrappedComponent {...this.props} />
      };
    };
  };
  ```

<a name='guide-props-9.11'></a>

- [9.11](#guide-props-9.11) Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha's beforeEach construct.

  ```jsx
  export default function Foo {
    const props = {
      text: '',
      isPublished: false,
    };

    return (<div {...props} />);
  };
  ```

**[⬆ back to top](#toc)**

## Refs

<a name='guide-refs'></a>

<a name='guide-refs-10.1'></a>

- [10.1](#guide-refs-10.1) Always use ref callbacks.

  **eslint:** [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

  ```jsx
  // bad
  <Foo
    ref="myRef"
  />

  // good
  <Foo
    ref={ref => { this.myRef = ref; }}
  />
  ```

**[⬆ back to top](#toc)**

## Parentheses

<a name='guide-parentheses'></a>

<a name='guide-parentheses-11.1'></a>

- [11.1](#guide-parentheses-11.1) Wrap JSX tags in parentheses when they span more than one line.

  **eslint:** [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

  ```jsx
  // bad
  render() {
    return <MyComponent variant="long body" foo="bar">
             <MyChild />
           </MyComponent>;
  };

  // good
  render() {
    return (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    );
  };

  // good, when single line
  render() {
    const body = <div>hello</div>;
    return <MyComponent>{body}</MyComponent>;
  };
  ```

**[⬆ back to top](#toc)**

## Tags

<a name='guide-tags'></a>

<a name='guide-tags-12.1'></a>

- [12.1](#guide-tags-12.1) Always self-close tags that have no children.

  **eslint:** [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

  ```jsx
  // bad
  <Foo variant="stuff"></Foo>

  // good
  <Foo variant="stuff" />
  ```

<a name='guide-tags-12.2'></a>

- [12.2](#guide-tags-12.2) If your component has multi-line properties, close its tag on a new line.

  **eslint:** [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

  ```jsx
  // bad
  <Foo
    bar="bar"
    baz="baz" />

  // good
  <Foo
    bar="bar"
    baz="baz"
  />
  ```

**[⬆ back to top](#toc)**

## Methods

<a name='guide-methods'></a>

<a name='guide-methods-13.1'></a>

- [13.1](#guide-methods-13.1) Use arrow functions to close over local variables.

  ```jsx
  function ItemList(props) {
    return (
      <ul>
        {props.items.map((item, index) => (
          <Item
            key={item.key}
            onClick={() => doSomethingWith(item.name, index)}
          />
        ))}
      </ul>
    );
  };
  ```

<a name='guide-methods-13.2'></a>

- [13.2](#guide-methods-13.2) Bind event handlers for the render method in the constructor.

  **eslint:** [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > Why? A bind call in the render path creates a brand new function on every single render.

  ```jsx
  // bad
   class extends React.Component {
     onClickDiv() {
       // do stuff
     };

     render() {
       return <div onClick={this.onClickDiv.bind(this)} />;
     };
   };

   // good
   class extends React.Component {
     constructor(props) {
       super(props);

       this.onClickDiv = this.onClickDiv.bind(this);
     };

     onClickDiv() {
       // do stuff
     };

     render() {
       return <div onClick={this.onClickDiv} />;
     };
   };
  ```

<a name='guide-methods-13.3'></a>

- [13.3](#guide-methods-13.3) Do not use underscore prefix for internal methods of a React component.

  > Why? Underscore prefixes are cometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) fora  more in-depth discussion.

  ```jsx
  // bad
  React.createClass({
    _onClickSubmit() {
      // do stuff
    };

    // other stuff
  });

  // good
  class extends React.Component {
    onClickSubmit() {
      // do stuff
    };

    // other stuff
  };
  ```

<a name='guide-methods-13.4'></a>

- [13.4](#guide-methods-13.4) Be sure to return a value in your `render` methods.

  **eslint:** [`react/require-render-return`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/require-render-return.md)

  ```jsx
  // bad
  render() {
    (<div />);
  };

  // good
  render() {
    return (<div />);
  };
  ```

**[⬆ back to top](#toc)**

## Ordering

<a name='guide-ordering'></a>

<a name='guide-ordering-14.1'></a>

- [14.1](#guide-ordering-14.1) Ordering for `class extends React.Component`:

1. optional `static` methods
1. `constructor`
1. `getChildContext`
1. `componentWillMount`
1. `componentDidMount`
1. `componentWillReceiveProps`
1. `shouldComponentUpdate`
1. `componentWillUpdate`
1. `componentDidUpdate`
1. `componentWillUnmount`
1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
1. `render`

<a name='guide-ordering-14.2'></a>

- [14.2](#guide-ordering-14.2) How to define `propTypes`, `defaultProps`, `contextTypes`, etc....

  ```jsx
  import React from 'react';
  import PropTypes from 'prop-types';

  const propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
  };

  const defaultProps = {
    text: 'Hello World',
  };

  class Link extends React.Compnent {
    static methodsAreOkay() {
      return true;
    };

    render() {
      return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>;
    };
  };

  Link.propTypes = propTypes;
  Link.defaultProps = defaultProps;

  export default Link;
  ```

<a name='guide-ordering-14.3'></a>

- [14.3](#guide-ordering-14.3) Ordering for `React.createClass`:

  **eslint:** [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

1. `displayName`
1. `propTypes`
1. `contextTypes`
1. `childContextTypes`
1. `mixins`
1. `statics`
1. `defaultProps`
1. `getDefaultProps`
1. `getInitialState`
1. `getChildContext`
1. `componentWillMount`
1. `componentDidMount`
1. `componentWillReceiveProps`
1. `shouldComponentUpdate`
1. `componentWillUpdate`
1. `componentDidUpdate`
1. `componentWillUnmount`
1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
1. *optional render methods* like `renderNavigation()` or `renderProfilePicture()`
1. `render`

**[⬆ back to top](#toc)**

## `isMounted`

<a name='guide-isMounted'></a>

<a name='guide-isMounted-15.1'></a>

- [15.1](#guide-isMounted-15.1) Do not use `isMounted`.

  **eslint:** [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern](https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html), is not available when using ES6 classes, and is on its way to being officially deprecated.

**[⬆ back to top](#toc)**