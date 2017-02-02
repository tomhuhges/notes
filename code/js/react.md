# React

## table of contents

- [basics]()
  - [create a component (3 ways)]()
  - [render a component]()  

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

###
