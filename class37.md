# React I

## ES6 Review

[Source: ES6 Syntax and Feature Overview](https://www.taniarascia.com/es6-syntax-and-feature-overview/)

| Keyword | Scope | Hoisting | Can Be Reassigned | Can Be Redeclared |
|-|-|-|-|-|
| `var` | Function scope | `Yes` | `Yes` | `Yes` |
| `let` | Block scope | No | `Yes` | No |
| `const` | Block scope | No | No | No |

> Use const unless working in a loop, then use let

### Arrow functions

```javascript
let func = (a) => {} // parentheses optional with one parameter
let func = (a, b, c) => {} // parentheses required with multiple parameters
```

### Template literals

```javascript
let str = `Release Date: ${date}`


// multiline
let str = `This text
            is on
            multiple lines`
```

### Arrow Functions

```javascript
let func = (a, b, c) => a + b + c // curly brackets must be omitted
```

### Method definition

```javascript
// old
var obj = {
  a: function (c, d) {},
  b: function (e, f) {},
}

// new
let obj = {
  a(c, d) {},
  b(e, f) {},
}
```

> you can remove 'function' when defining methods on an object

### Array Looping

```javascript
for (let i of arr) {
  console.log(i)
}
```

## React

[Source: https://reactjs.org/](https://reactjs.org/)

### Hello World

```javascript
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
```

### JSX

```js
const element = <h1>Hello, world!</h1>;
```

- Similiar to templating language
- Full power of Javascript
- Produces react "elements"
- Can be used in `if` and `for` loops
- Prevents injection attacks

> Rendering logic is tightly coupled with UI logic
> React separates concerns rather than technologies

```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

#### Attributes

- Use quotes for string attributes
- Use curlies for JS expressions

```js
const element = <div tabIndex="0"></div>;
```

#### Children

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

#### JSX Objects

- JSX is essentially creating objects
- Below are identical examples
- `React.createElement()` does validations
- Objects are called "React Elements"

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

> Use "Babel" language for syntax highlighting

### Rendering Elements

```js
const element = <h1>Hello, world</h1>;
```

- Element is what you see on the screen
- React DOM manages updating DOM to match elements
- Elements make up components

#### Rendering

```js
<div id="root"></div>
```

- React renders to a specific DOM node
- Everything inside managed by React DOM
- React can manage the entire page
- OR you can have multiple React DOM nodes

```js
const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

> pass element and specific DOM node to render

#### Updating

- Elements are immutable
- Represents UI at point in time
- Only updates whatever is needed

```js
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

> Example shows updating the element every second to change state
> Not normal practice; state managment in further section

### Components and Props

- Create reusable UI "pieces"
- Accept inputs or `props` (properties)
- Return React elements
- Props should not be manipulated in a component
- Component names should be capticalized

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

// or ES6 class
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

#### Rendering

- Elements can represent user-defined components

```js
const element = <Welcome name="Sara" />;
```

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

#### Composing

- Component can refer to other components in output

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

> New apps often have single App component at top
> If you integrate React into existing, you may start with small component (button) and work your way up

#### Extracting

- Split components into smaller components
- Promoted reusability

```js
// accepts author, text, date as props
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

// extract avatar because comment state irrelevant
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}

// extract UserInfo which renders Avatar
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}


// simplified comment:
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

### State and Lifecycle

- Maintains data about a component
- Keeps element updated without action
- State is fully controlled by component

#### Convert Function to Class

1. Create ES6 class, extend `React.Component`
2. Add single empty method, `render()`
3. Move the body of the function into `render()`
4. Replace `props` with `this.props` in render body
5. Delete remaining empty function declaration

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

> Now, Clock is ready support state management

#### Adding Local State to Class

1. Replace `this.props.date` with `this.state.date` in `render()`
2. Add a class constructor that assigns initial `this.state`
   - props are passed to base constructor
   - class components should always call base constructor with props
3. Remove `date` prop from `<Clock />`

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

#### Adding Lifecycle Methods to Class

- Help with good resource management
- Free up resources when components destroyed
- Rendering -> Mounting
- Removing/clearing -> Unmounting
- `componentDidMount()`
- `componentWillUnmount()`

```js
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

#### Best Practices

- Don't modify state directly (only constructor)
- Must be done in `setState()`
- React will batch calls if needed
- `this.props` and `this.state` set asych
  - do not rely on for calculating state
  - Instead, pass function into `setState()`
  - Allows state to be referrenced

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

// or regular function

}));
```

### Handling Events

> Similiar to HTML

```js
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

```js
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

> Class components have event handlers as methods

```js
// renders button user can toggle between off and on

class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

### Passing args

> If you want to pass an extra param to event handle in a loop

```js
// arrow syntax
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
// bind syntax
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```