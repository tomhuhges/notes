# React

## table of contents

- [basics]()
  - [create a component (3 ways)]()
  - [render a component]()  
  - [pass props]()

----

## basics

### create a component (3 ways)

##### createClass()

takes an object with a render method as an argument:

```jsx
const Component = React.createClass({
  render: function (){
    return (
      <div>
        <h1>hello world</h1>
      <div />
    )
  }
  })
```

##### class

```jsx
class Component extends React.Component {
  render() {
    return (
      <div>
        <h1>hello world</h1>
      <div />
    )
  }
}
```

##### pure function

```jsx
const Component = () =>
  <div>
    <h1>hello world</h1>
  </div>
```

----

### render a component

```jsx
import React from 'react'
import ReactDOM from 'react-dom'

class Component //...

ReactDOM.render(
  <Component />,
  document.getElementById('app')
  )
```

----

### pass props

there are a couple of ways to pass props, and props can be any type of data.

##### as an attribute

```jsx
// string:
const Component = (props) =>
  <h1>hello, {this.props.name}</h1>

ReactDOM.render(
  <Component name="tom" />,
  document.getElementById('app')
  )
// returns <h1>hello, tom</div>

// array:
```jsx
const App = () => {
  const users = ['user1', 'user2', 'user3']
  return (
    <Component users={users} />
    )
}

const Component = () => {
  return (
    <ul>
      {this.props.users.map(user =>
        <li>{user}</li>
      )}
    </ul>
  )
}

// returns
// <ul>
//   <li>user1</l1>
//   <li>user2</l1>
//   <li>user3</l1>
// </ul>
```

##### through `this.props children`

`this.props.children` can be as simple as a string, or an entire component

```jsx
const Component = () =>
  <Banner>hello world</Banner>

const Banner = () =>
  <h1>{this.props.children}</h1>

// returns <h1>hello world</h1>
```

----
