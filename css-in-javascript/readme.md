# Ultrasound CSS-in-JavaScript Style Guide

*A mostly reasonable approach to CSSin-JavaScript

## Table of Contents

<a name='toc'></a>

1. [Naming](#guide-naming)
1. [Ordering](#guide-ordering)
1. [Nesting](#guide-nesting)
1. [Inline](#guide-inline)
1. [Themes](#guide-themes)

## Naming

<a name='guide-naming'></a>

<a name='guide-naming-1.1'></a>

- [1.1](#guide-naming-1.1) Use camelCase for object keys (i.e. "selectors").

  > Why? We access these keys as properties on the `styles` object in the component, so it is most convenient to use camelCase

  ```js
  // bad
  {
    'bermuda-triangle': {
      display: 'none',
    }
  }

  // good
  {
    bermudaTriangle: {
      display: 'none',
    }
  }
  ```

<a name='guide-naming-1.2'></a>

- [1.2](#guide-naming-1.2) Use an underscore for modifiers to other styles.

  > Why? Similar to BEM, this naming convention makes it clear that the styles are intended to modify the element precended by the underscore. Underscores do not need to be quoted, so they are preferred over other characters, such as dashes.

  ```js
  // bad
  {
    bruceBanner: {
      color: 'pink',
      transition: 'color 10s',
    },

    bruceBannerTheHulk: {
      color: 'green',
    },
  }

  // good
  {
    bruceBanner: {
      color: 'pink',
      transition: 'color 10s',
    },

    bruceBanner_theHulk: {
      color: 'green',
    },
  }
  ```

<a name='guide-naming-1.3'></a>

- [1.3](#guide-naming-1.3) Use `selectorName_fallback` for sets of fallback styles.

  > Why? Similar to modifiers, keeping the naming consistent helps reveal the relationship of these styles to the styles that override them in more adequate browsers.

  ```js
  // bad
  {
    muscles: {
      display: 'flex',
    },

    muscles_sadBears: {
      width: '100%',
    },
  }

  // good
  {
    muscles: {
      display: 'flex',
    },

    muscles_fallback: {
      width: '100%',
    },
  }
  ```

<a name='guide-naming-1.4'></a>

- [1.4](#guide-naming-1.4) Use a separate selector for sets of fallback styles.

  > Why? Keeping fallback styles contained in a separate object clarifies their purpose, which improves readability.

  ```js
  // bad
  {
    muscles: {
      display: 'flex',
    },

    left: {
      flexGrow: 1,
      display: 'inline-block',
    },

    right: {
      display: 'inline-block',
    },
  }

  // good
  {
    muscles: {
      display: 'flex',
    },
    left: {
      flexGrow: 1,
    },

    left_fallback: {
      display: 'inline-block',
    },

    right_fallback: {
      display: 'inline-block',
    },
  }
  ```

<a name='guide-naming-1.5'></a>

- [1.5](#guide-naming-1.5) Use device-agnostic names (e.g. "small", "medium", and "large") to name media query breakpoints.

  > Why? Commonly used names like "phone", "tablet", and "desktop" do not match the characteristics of the devices in the real world. Using these names sets the wrong expectations.

  ```js
  // bad
  const breakpoints = {
    mobile: '@media (max-width: 639px)',
    tablet: '@media (max-width: 1047px)',
    desktop: '@media (min-width: 1048px)',
  };

  // good
  const breakpoints = {
    small: '@media (max-width: 639px)',
    medium: '@media (max-width: 1047px)',
    large: '@media (min-width: 1048px)',
  };
  ```

**[⬆ back to top](#toc)**

## Ordering

<a name='guide-ordering'></a>

<a name='guide-ordering-2.1'></a>

- [2.1](#guide-ordering-2.1) Define styles after the component.

  > Why? We use a higher-order component to theme our styles, which is naturally used after the component definition. Passing the styles object directly to this function reduces indirection.

  ```jsx
  // bad
  const styles = {
    container: {
      display: 'inline-block',
    },
  };

  function MyComponent({ styles }) {
    return (
      <div {...css(styles.container)}>
        Never doubt that a small group of thoughtful, committed citizens can change the world. Indeed, it's the only thing that ever has.
      </div>
    );
  };

  export default withStyles(() => styles)(MyComponent);

  // good
  function MyComponent({ styles }) {
    return (
      <div {...css(styles.container)}>
        Never doubt that a small group of thoughtful, committed citizens can change the world. Indeed, it's the only thing that ever has.
      </div>
    );
  };

  export default withStyles(() => ({
    container: {
      display: 'inline-block',
    },
  }))(MyComponent;
  ```

**[⬆ back to top](#toc)**

## Nesting

<a name='guide-nesting'></a>

<a name='guide-nesting-3.1'></a>

- [3.1](#guide-nesting-3.1) Leave a blank line between adjacent blocks at the same indentation level.

  > Why? The whitespace improves readability and reduces the likelihood of merge conflicts.

  ```js
  // bad
  {
    bigGang: {
      display: 'inline-block',
      '::before': {
        content: "''",
      },
    },
    universe: {
      border: 'none',
    },
  }

  // good
  {
    bigGang: {
      display: 'inline-block',

      '::before': {
        content: "''",
      },
    },

    universe: {
      border: 'none',
    },
  }
  ```

**[⬆ back to top](#toc)**

## Inline

<a name='guide-inline'></a>

<a name='guide-inline-4.1'></a>

- [4.1](#guide-inline-4.1) Use inline styles for styles that have a high cardinality (e.g. uses the value of a prop) and not for styles that have a low cardinality.

  > Why? Generating themed stylesheets can be expensive, so they are best for discrete sets of styles.

  ```jsx
  // bad
  export default function MyComponent({ spacing }) {
    return (
      <div style={{ display: 'table', margin: spacing }} />
    );
  };

  // good
  function MyComponent({ styles, spacing }) {
    return (
      <div {...css(styles.periodic, { margin: spacing })} />
    );
  };
  export default withStyles(() => ({
    periodic: {
      display: 'table',
    },
  }))(MyComponent);
  ```

**[⬆ back to top](#toc)**

## Themes

<a name='guide-themes'></a>

<a name='guide-themes-5.1'></a>

- [5.1](#guide-themes-5.1) Use an abstraction layer such as [react-with-styles](https://github.com/airbnb/react-with-styles) that enables theming. *react-with-styles fives us things like `withStyles()`, `ThemedStyleSheet`, and `css()` which are used in some of the examples in this document.*

  > Why? It is useful to have a set of shared variables for styling your components. Using an abstraction layer makes this more convenient. Additionally, this can help prevent your components from being tightly coupled to any particular underlying implementation, which gives you more freedom.

<a name='guide-themes-5.2'></a>

- [5.2](#guide-themes-5.2) Define colors only in themes.

  ```jsx
  // bad
  export default withStyles(() => ({
    chuckNorris: {
      color: 'bada55',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ color }) => {
    chuckNorris: {
      color: 'bada55',
    },
  })(MyComponent);
  ```

<a name='guide-themes-5.3'></a>

- [5.3](#guide-themes-5.3) Define fonts only in themes.

  ```jsx
  // bad
  export default withStyles(() => ({
    towerOfPisa: {
      fontStyle: 'italic',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ font }) => ({
    towerOfPisa: {
      fontStyle: font.italic,
    },
  }))(MyComponent);
  ```

<a name='guide-themes-5.4'></a>

- [5.4](#guide-themes-5.4) Define fonts as sets of related styles.

  ```jsx
  // bad
  export default withStyles(() => ({
    towerOfPisa: {
      fontFamily: 'Italiana, "Times New Roman", serif',
      fontSize: '2em',
      fontStyle: 'italic',
      lineHeight: 1.5,
    },
  }))(MyComponent);

  // good
  export default withStyles(({ font }) => ({
    towerOfPisa: {
      ...font.italian,
    },
  }))(MyComponent);
  ```

<a name='guide-themes-5.5'></a>

- [5.5](#guide-themes-5.5) Define base grid units in theme (either as a value of a function that takes a multiplier).

  ```jsx
  export default withStyles(() => ({
    rip: {
      bottom: '-6912px', // 6 feet
    },
  }))(MyComponent);

  // good
  export default withStyles(({ units }) => ({
    rip: {
      bottom: units(864), // 6 feet
    },
  }))(MyComponent);

  // good
  export default withStyles(({ unit }) => ({
    rip: {
      bottom: 864 * unit, // 6 feet, assuming our unit is 8px
    }
  }))(MyComponent);
  ```

<a name='guide-themes-5.6'></a>

- [5.6](#guide-themes-5.6) Define media queries only in themes.

  ```jsx
  // bad
  export default withStyles(() => ({
    container: {
      width: '100%',

      '@media (max-width: 1047px)': {
        width: '50%',
      },
    },
  }))(MyComponent);

  // good
  export default withStyles(({ breakpoint }) => ({
    container: {
      width: '100%',

      [breakpoint.medium]: {
        width: '50%',
      },
    },
  }))(MyComponent);
  ```

<a name='guide-themes-5.7'></a>

- [5.7](#guide-themes-5.7) Define tricky fallback properties in themes.

  > Why? Many CSS-in-JavaScript implementations merge style objects together which makes specifying fallbacks for the same property (e.g. `display`) a little tricky. To keep the approach unified, put these fallbacks in the theme.

  ```jsx
  // bad
  export default withStyles(() => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      'display': 'table',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ fallbacks }) => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      [fallbacks.display]: 'table',
    },
  }))(MyComponent);

  // good
  export default withStyles(({ fallback }) => ({
    .muscles {
      display: 'flex',
    },

    .muscles_fallback {
      [fallback('display')]: 'table',
    },
  }))(MyComponent);
  ```

<a name='guide-themes-5.8'></a>

- [5.8](#guide-themes-5.8) Create as few custom themes as possible. Many applications may only have one theme.

<a name='guide-themes-5.9'></a>

- [5.9](#guide-themes-5.9) Namespace custom theme settings under a nested object with a unique and descriptive key.

  ```jsx
  // bad
  ThemedStyleSheet.registerTheme('mySection', {
    mySectionPrimaryColor: 'green',
  });

  // good
  ThemedStyleSheet.registerTheme('mySection', {
    mySection: {
      primaryColor: 'green',
    },
  });
  ```

**[⬆ back to top](#toc)**

------------------------------

CSS puns adapted from [Saijo George](https://saijogeorge.com/css-puns/).