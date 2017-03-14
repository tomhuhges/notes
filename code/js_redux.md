# Redux

## table of contents

- [basics]()
  - [the store]()
  - [actions]()
  - [reducers]()
- [react-redux]()
  - [Provider]()
  - [connect]()
  - [mapStateToProps]()
  - [mapDispatchToProps]()
  - [data flow]()
----

## basics

### the store

redux (usually) has just one store that contains all of the app's data. when a container component wants to change the state, the store is notified, changes its state, and then lets components know what the new state is.

the store should be created at the entry point for the app like this:

```js
import { createStore } from 'redux'
const store = createStore(reducer)
// or
import { createStore, combineReducers } from 'redux'
const store = createStore(combineReducers(reducers))
```

### actions
these are the things that notify the store when a state-changing event happens.

they look like this:

```js
{
  type: ACTION_NAME,
  data: { info: 'some data'}
}
```

### reducers
reducers are the things that change the store's state. they are functions that take the current state of the store and an action, and return the new state accordingly.

they look like this:

```js
function reducer(state = [], action) {
  switch(action.type) {
    case: ACTION_NAME:
      return [
        ...state,
        Object.assign({}, action.data)
      ]
    // ...
  }
}
```

----

## react-redux

### Provider

the `Provider` component provides a way to make the redux store available as a prop to all components. We wrap our app with the Provider because the data will flow down to everything inside it.

```js
<Provider store={this.props.store}>
  <App />
</Provider>
```

----

### connect

each container (stateful) component must be wrapped in react-redux's `connect` function. it takes 2 optional arguments - the [`mapStateToProps`](#mapStateToProps) and [`mapDispatchToProps`](#mapDispatchToProps) callbacks which must be defined beforehand. connect returns a function, and that function then takes the component as an argument.

```js
class Container extends React.Component {
  // ...
}
export default connect(mapStateToProps, mapDispatchToProps)(Container)
```

----

### mapStateToProps

this function determines the state you want to expose to the container component (ie. **the data the component needs to use**). technically you could just return the entire state but in large apps this would kill performance. it's better to restrict to only the parts of the state that the component needs to know about:

```js
function mapStateToProps(state){
  return {
    name: state.name,
    address: state.address    
  }
}
// inside component use this.props.name, this.props.address
```

----

### mapDispatchToProps

this function determines the actions you want to expose to the container component (ie. **the actions the component needs to use**). you can use redux's `bindActionCreators` function to inject dispatch functionality into each action:

```js
import { bindActionCreators } from 'redux'
import * as actions from './actions'
// ...
class Container extends React.Component { ... }
// ...
function mapDispatchToProps(dispatch) {
  return {
    actions: bindActionCreators(actions, dispatch)
  }
}
// inside component use this.props.actions.actionName
```

when using bindActionCreators, you can also use the shorthand of just passing an object of actions, where each action will automatically be run through bindActionCreators with dispatch functionality injected:

```js
import * as actions from './actions'
// ...
export default connect(mapStateToProps, actions)(Container)
// inside component use this.props.actions.actionName
```

----

### data flow

when an event happens (such as user input), the callback for the event will dispatch the relevant action from this.props.actions

the action will then send the action type and new data to the corresponding reducer.

the reducer takes the current state and the new data and updates the store with a new version of the state. the store then notifies all of the connected components that the state has changed.

if they need to re-render because of the state change, they will and the view will be updated.

----

###
