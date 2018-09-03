---
layout: single
title:  "Unit Testing for Beginners with JavaScript and Jest"
date:   2018-07-09 18:33:52 +0900
categories: jekyll update
---

Contents
1. Seperate Actions from components  
diagram

2. We added objects to each dispatch function  

```
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

create actions directory and index.js file and reducers import it

```
export const INCREMENT = 'INCREMENT'
export const ADD = 'ADD'
export const ADD_HISTORY = 'ADD_HISTORY'
export const DELETE_HISTORY = 'DELETE_HISTORY'
```

move id variable from reducer/index.js to actions/index.js

```
let id = 0

export const inc = () => {
  return {
    type: INCREMENT
  }
}

export const add = () => {
  return {
    type: ADD,
    value: 5
  }
}

export const addHistory = () => {
  return {
    type: ADD_HISTORY,
    id: id++
  }
}

export const deleteHistory = (id) => {
  return {
    type: DELETE_HISTORY,
    id: id
  }
}
```

3.
