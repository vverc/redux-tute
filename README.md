# Let's learn some Redux

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app), using the [Redux](https://redux.js.org/) and [Redux Toolkit](https://redux-toolkit.js.org/) template.

## Redux Overview

### Actions 
A plain ol' JavaScript object with a `type` field. It can have other fields too.
An event that describes something happening in the app. 

### Action Creators
Functions that make actions. Easy.

### Reducers
A function that takes current state and an action and updates state if necessary.
Reducer functions **must update state immutably**. They need to create a new copy of the state then modify that copy of the state to make changes. 

### Store
The place where state lives. A store is created by passing in a reducer. 
You can use the store's method **getState()** to get state. Easy.
More interestingly. You can use the store's method **dispatch()** to update the state.

To update the state, you pass in an **action object** into the store's **dispatch method**. The store then runs its reducer function to figure out what to do with the action and updates the state as necessary.

### Selectors 
Functions that can extract specific pieces of the store state. 
These are needed for code reusability and come in handy as the app gets bigger. Easy stuff.

## Redux Data Flow

Redux has a "one-way data flow" structure. The current state describes the app at that current point and the UI renders based on that.
If something happens in the app:
  * The UI dispatches an action to the store.
  * The store runs its reducers and state changes based on the action
  * The store tells the UI the state has changed 
  * The UI rerenders to reflect any changes in state.

## Redux App Structure

# Creating the Redux Store

In `app/store.js` we have: 

```
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});
```

`configureStore` is used to create the Redux store. It requires a reducer as an argument. 
In the above code,


  
 
