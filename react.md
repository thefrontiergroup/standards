# Introduction

[React](https://facebook.github.io/react/) is a JavaScript library for building user interfaces. The following guide is to be followed in addition to the [ES2015](es2015.md) style guide.

# Tooling

To lint our React apps we use:

- [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)

To plug into [eslint](http://eslint.org).

# Styleguide

General points:

- React Props and States are camelCased
- Use propTypes where practicable
- Single line JSX to have parentheses
- Multiline props when more than 5 props are in a component
- Multiline props to be in alphabetical order
- Multiline props to be indented 2 spaces
```
<Component
  AProp={this.handleAProp}
  BProp={this.handleBProp}
  CProp={this.handleCProp}
  DProp={this.handleDProp}
  EProp={this.handleEProp}>
</Component>
```

# Folder Structure

```
webpack
  └── App
    └── Actions
    └── Stores
    └── Components
    └── Constants
    └── Sources
    └── Utils
```

# Class Structure

## Basic component

```
export default class NewComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {};
  }
  componentWillMount() {...}
  componentDidMount() {...}
  componentWillReceiveProps(nextProps, nextContext) {...}
  shouldComponentUpdate(nextProps, nextState, nextContext) {...}
  componentWillUpdate(nextProps, nextState, nextContext) {...}
  componentDidUpdate(prevProps, prevState, prevContext) {...}
  componentWillUnmount() {...}
  render() {...}
}

NewComponent.propTypes = {...};

NewComponent.defaultProps = {...}:
```

## Component with custom methods

Components with custom methods are to be placed after lifecycle methods, with the `render()` method to be placed last.

```
export default class NewComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = {...};
  }
  componentWillMount() {...}
  componentDidMount() {...}
  componentWillReceiveProps(nextProps, nextContext) {...}
  shouldComponentUpdate(nextProps, nextState, nextContext) {...}
  componentWillUpdate(nextProps, nextState, nextContext) {...}
  componentDidUpdate(prevProps, prevState, prevContext) {...}
  componentWillUnmount() {...}

  /* custom methods */

  handleXyz() {...}
  doSomething() {...}

  render() {...}
}

NewComponent.propTypes = {...};

NewComponent.defaultProps = {...};
```

This way `render()` is always to be found at the end of the file (the 'final', actual output) and everything that is 'composed' together to create the final output, can be found above the `render()` method.

# Method Names

- Name event handlers like `handleEvent()`
- Name prop handlers like `onEvent()`
- Name helper methods for `render()` like `renderElement()`

# Linting Rules

The following React rules used are:

```
"react/jsx-curly-spacing": 1,
"react/jsx-indent-props": [2, 2],
"react/jsx-no-duplicate-props": 1,
"react/jsx-no-undef": 1,
"react/jsx-sort-prop-types": 1,
"react/jsx-sort-props": 1,
"react/jsx-uses-react": 1,
"react/jsx-uses-vars": 1,
"react/no-did-mount-set-state": 1,
"react/no-did-update-set-state": 1,
"react/no-multi-comp": 1,
"react/no-unknown-property": 1,
"react/react-in-jsx-scope": 1,
"react/require-extension": 1,
"react/self-closing-comp": 1,
"react/sort-comp": 1,
"react/wrap-multilines": 1,
```

# Sample Configuration

- Example [.eslintrc](examples/.eslintrc)
- Example [.eslintignore](examples/.eslintignore)
