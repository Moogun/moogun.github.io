---
layout: single
title:  "React-Redux Made Easy 1"
date:   2018-07-09 18:33:52 +0900
categories: jekyll update
---

![Fish-Bone]({{ "/assets/fish-bone.jpg" | absolute_url }})

My junior found it difficult to grasp how redux works with react and I decided to write an article about redux and react-redux with easiest and simplest examples.   

This article is one of the two parts. The first part will explain the basics of redux and how React side reads state from redux(store). The 2nd part will explain how React manages and updates state with Redux with simple math and log functions.

It would be more useful to read this article with official React-Redux instruction https://redux.js.org/introduction. But please note that this article explains the building blocks of Redux in (kind of) an opposite direction of the official guide from Store -> Reducers -> Actions.

Create-React-App Package, Redux, React-Redux libraries will be used to see how they works.
```
Contents

1. How plain redux works
2. How to read state from redux store with mapStateToProps
```

1. Redux Structure

The four items below are building blocks of Redux and do things as each of the following statement explains.

1. Store - Manage all state
2. Reducer - Reducer will be passed to store and calculate state changes
3. Dispatch - Dispatch will send data to the store so that appropriate reducer in the store can calculate state changes
4. Subscription - Subscription will be called to read state changes

Clear? Not Clear?

Let's test those items.


2. Install a React app with Create React App package, Redux library, and then create a file 'redux-test.js' under src directory

Visit https://github.com/facebook/create-react-app if you don't know how to use Create React App package.


3. Implement Store and Reducer

The store will be the one and only source of state in an app and will brings the other building blocks together. The reducer will be passed to the store and wait for any state changes to calculate it, and dispatch functions will send data (which is called actions) to the store.  


The store will have the following responsibilities:

- Allows access to state via getState();
- Allows state to be updated via dispatch(action);
- Registers listeners via subscribe(listener);

Let's add below in redux-test.js and run the file in the console with 'node src/redux-test.js' Keep in mind it has nothing to do with the react app yet.

```
const store = createStore()
const rootReducer = (state, action) => {}
console.log(store.getState());
```

It will print an error because any reducer has not been passed to the store. Now add the rootReducer to the store and change the order of store and rootReducer as below.

```
const rootReducer = (state, action) => {
  return state
}
const store = createStore(rootReducer)
console.log(store.getState());
```
This will print undefined because state has not been initialized.

Let's add initial state and assign it to state in rootReducer as below.

```
const initialState = {
  counter: 0
}
const rootReducer = (state = initialState, action) => {
  return state
}
...
```
Run the file again. Now consol.log(store.getState()) will print {counter: 0}.

4. dispatch action

Now it's time for dispatch. Like I said, dispatch send data to store so the reducer inside the store can calculate and update the state. The dispatch receives a javascript object with a type property and additional properties

Let's add below after 'store '
```
const store = createStore(rootReducer)
....

store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'ADD', value: 10})
console.log(store.getState)
```

Save the file and run it. It will produce {counter: 0} {counter: 0}  because no logic has not been added to the rootReducer to deal with 'Increment' yet.

5. Let's add a logic to the rootReducer to deal with the 'INCREMENT' or 'ADD' type object passed by dispatch to the store.

```
const rootReducer = (state, action) => {
  if (action.type === 'INCREMENT') {
    return {
      counter: state.counter + 1
    }
  }
  return state
}
```

Now run the file again and see what message you get.
It will print { counter: 1 } because the rootReducer dealt with the 'INCREMENT' and updated the counter state.

One thing to remember is the below will mutate the state and it is not recommended way to update the state.

```
return {
  counter: state.counter + 1
}
```

Let's do this instead. Copy the state with ES6 rest operator and update the value. This way, the reducer won't mutate the original state and will merge the copied state and updated counter for us

```
return {
  ...state,
  counter: state.counter + 1
}
```

Let's add additional logic into the reducer to deal with 'ADD' type object.

```
const rootReducer = (state, action) => {
...
  if (action.type === 'ADD') {
    return {
      ...state,
      counter: state.counter + action.value
    }
  }
  return state
}
```

And now run the file, it will produce {counter: 0 } {counter: 11} in the console log.


6. subscription

We have covered Store, Reducer and Dispatch so far out of the Redux structure mentioned above. Now it's time for Subscription

Let's add below at the end of the file, save it and run the file.

```
store.subscribe( () => {
  console.log('[subscription]', store.getState())
  })
```

What do you see in the console ? The subscribe is listening to the state changes. If you don't see nothing, put the subscribe in between store and two dispatches, and run the file.

You will see logs in the order as below. Because subscribe will be called every time the state change occurs.

1. [dispatch] before { counter: 0 }
2. [subscribe] { counter: 1 }
3. [subscribe] { counter: 11 }
4. [dispatch] after { counter: 11 }

```
const redux = require('redux')
const createStore = redux.createStore

const initialState = {
  counter: 0
}

const rootReducer = (state = initialState, action) => {
  if (action.type === 'INCREMENT') {
    return {
      ...state,
      counter: state.counter + 1
    }
  }
  if (action.type === 'ADD') {
    return {
      ...state,
      counter: state.counter + action.value
    }
  }
  return state
}
const store = createStore(rootReducer)
console.log('[dispatch] before', store.getState())
store.subscribe( () => {
  console.log('[subscribe]', store.getState());
})

store.dispatch({type: 'INCREMENT'})
store.dispatch({type: 'ADD', value: 10})
console.log('[dispatch] after', store.getState())
```

Now it's time for integrating React and Redux.

Open your console and install react-redux

```
npm install --save react-redux
```

Now in your src/index.js file, import createStore and Provider from redux and react-redux and then copy reducer in the above test file and paste it into the index.js and pass the rootReducer to the store and then again pass the store to the provider and wrap the app compoennt

```
import {createStore} from 'redux'
import {Provider} from 'react-redux'

const initialState = {
  counter: 0
}

const rootReducer = (state = initialState, action) => {
  if (action.type === 'INCREMENT') {
    return {
      ...state,
      counter: state.counter + 1
    }
  }
  if (action.type === 'ADD') {
    return {
      ...state,
      counter: state.counter + action.value
    }
  }
  return state
}

const store = createStore(rootReducer)

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>
  , document.getElementById('root'));
registerServiceWorker();

```

Now create Counter component and import it in App.js file and try calling this.state.counter in the counter component.

You will see 'undefined' and now you need to implement 'mapStateToProps' function in the counter function as below.

```
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
import Counter from './containers/Counter'
class App extends Component {
  render() {
    return (
      <div>
        <h1>Redux Test</h1>
        <Counter />
      </div>
    );
  }
}

export default App;
```


```
import React, { Component } from 'react';
import {connect} from 'react-redux'

class Counter extends Component {
  render() {
    console.log('this.state.counter', this.props.counterFromStore);
    return (
      <div>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    counterFromStore: state.counter
  }
}

export default connect(mapStateToProps)(Counter);
```

mapStateToProps receives state and return an object. With the property name you have given (counterFromStore in here) will let you access the state in the store.

try to log 'this.props.counterFromStore' and you will see {counter : 0}

Successful ? That's great.

Now refactor the file structure little bit as below and keep digging on redux in the next article.
