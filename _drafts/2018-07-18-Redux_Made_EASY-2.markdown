---
layout: single
title:  "Unit Testing for Beginners with JavaScript and Jest"
date:   2018-07-09 18:33:52 +0900
categories: jekyll update
---

Contents
1. How to update state in redux store with mapDispatchToProps

Now we have the rootReducer and the counter component as below. We haven't made any change in the rootReducer.

Add mapDispatchToProps function receiving dispatch and returning 'incrementCounterInStore' (this is an arbitrary name and you can choose whatever you please )

Now it's time for integrating React and Redux.

Open your console and install react-redux

```
npm install --save react-redux
```

Copy part A from above implementation and paste it to src/index.js file,


```

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

refacotor reducer to another file

```
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

export default rootReducer



```
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

export default rootReducer
```


```
import React, { Component } from 'react';
import {connect} from 'react-redux'

class Counter extends Component {
  handleSubmit = () => {
    console.log('111');
  }
  render() {
    console.log('this.state.counter', this.props.counterFromStore);
    return (
      <div>
          <button onClick={this.props.incrementCounterInStore}> Increment Counter </button>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    counterFromStore: state.counter
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    incrementCounterInStore: () => dispatch({type: 'INCREMENT'})
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);

```

Try clicking button, it will produce incrementing number form 0 to 1, then 2, 3, 4, ...

2. Actions
Now let's say

```
const mapDispatchToProps = (dispatch) => {
  return {
    incrementCounterInStore: () => dispatch({type: 'INCREMENT'}),
    AddValueToCounterInStore: () => dispatch({
      type: 'ADD', value: 5,
    })
  }
}
```
Dont' forget the comma after incrementCounterInStore becuase mapDispatchToProps returns an object. Missing comma will produce an error

3. Refactor reducer with swich
```
const rootReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return {
        ...state,
        counter: state.counter + 1
      }
    case 'ADD':
      return {
        ...state,
        counter: state.counter + action.value
      }
    default:
      return state
  }
}
```

0. add new state property
  history array

```
const initialState = {
  counter: 0,
  history : []
}
const rootReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return {
        ...state,
        counter: state.counter + 1
      }
    case 'ADD':
      return {
        ...state,
        counter: state.counter + action.value
      }
    case 'HISTORY':
      return {
        ...state,
        history: state.history.concat(state.counter)
      }
    default:
      return state
  }
}
```

```
class Counter extends Component {
  handleSubmit = () => {
    console.log('111');
  }
  render() {
    console.log('this.state.counter', this.props.counterFromStore);
    console.log('this.state.historyFromStore', this.props.historyFromStore);
    return (
      <div>
          <button onClick={this.props.incrementCounterInStore}> Increment Counter </button>
          <button onClick={this.props.addValueToCounterInStore}> Add 5 to the counter </button>
          <button onClick={this.props.storeHistory}> storeHistory </button>
      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    counterFromStore: state.counter,
    historyFromStore: state.history
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    incrementCounterInStore: () => dispatch({type: 'INCREMENT'}),
    addValueToCounterInStore: () => dispatch({
      type: 'ADD', value: 5,
    }),
    storeHistory: () => dispatch({
      type: 'HISTORY',
    })
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(Counter);
```

0. how to add and delete history array

counter component
```
class Counter extends Component {
  handleSubmit = () => {
    console.log('111');
  }
  render() {
    console.log('this.state.counter', this.props.counterFromStore);
    console.log('this.state.historyFromStore', this.props.historyFromStore);
    return (
      <div>
          <button onClick={this.props.incrementCounterInStore}> Increment Counter </button>
          <button onClick={this.props.addValueToCounterInStore}> Add 5 to the counter </button>
          <button onClick={this.props.addHistory}> addHistory </button>

          {this.props.historyFromStore.map((h) =>
            <p key={h.id}
              onClick={() => this.props.deleteHistory(h.id)}
              >{h.value}</p>
          )}

      </div>
    );
  }
}

const mapStateToProps = (state) => {
  return {
    counterFromStore: state.counter,
    historyFromStore: state.history
  }
}

const mapDispatchToProps = (dispatch) => {
  return {
    incrementCounterInStore: () => dispatch({type: 'INCREMENT'}),
    addValueToCounterInStore: () => dispatch({
      type: 'ADD', value: 5,
    }),
    addHistory: () => dispatch({
      type: 'ADD_HISTORY'
    }),
    deleteHistory: (id) => dispatch({
      type: 'DELETE_HISTORY', id: id
    })
  }
}
```

reducer
```
case 'ADD_HISTORY':
    return {
      ...state,
      history: state.history.concat({id: id++, value: state.counter})
    }
  case 'DELETE_HISTORY':
    const updatedArray = state.history.filter(h => h.id !== action.id)
    return {
      ...state,
      history: updatedArray
    }
```

3. Seperate actions from dispatch



{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.

function hello(name) {
  console.log(name)
}
{% endhighlight %}


Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
