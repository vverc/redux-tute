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

### Creating the Redux Store

In `app/store.js` we have: 

```
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: `counterReducer`,
  },
});
```

`configureStore` is used to create the Redux store. It requires a reducer as an argument. 
In the above code, we're creating a store with a **reducer slice** called **counterReducer**. This counterReducer will handle all the stuff related to the counter state.

### Creating Slice Reducers and Actions


  
Slices are created with the `createSlice` function. The function has multiple fields.
  * `name`: the name of the slice.
  * `reducers`: definitions of various reducers used within the slice
  * `initialState`: the default state for the slice's initialisation
  
In all of this code we still haven't mentioned actions. In the UI we had three buttons which dispatched three action types.
  * `{type: "counter/increment"}`
  * `{type: "counter/decrement"}`
  * `{type: "counter/incrementByAmount"}`
  
The cool thing that happens here is that actions are generated based off the `name` and `reducers`. 
So for each of the reducers we define in the slice, an action with `{type: "<name>/<reducer_name>"}` is made for us. 
No need to define actions.

### Reducers and Immutable Updates 

So we said before that we can only make immutable updates within reducers. `createSlice` uses a library called Immer that makes writing immutable updates so much easier. 
However it's important to remember that Immer is only used within the Redux Toolkit's `createSlice` and `createReducer` functions. 

### Writing Async Logic with Thunks

A **thunk** is a Redux function that can contain asynchronous logic. Thunks are made using two functions:
  * An inside thunk function, which takes `dispatch` and `getState` as arguments
  * An outside creator function, which creates and returns the thunk function

```
// the outside "thunk creator" function
const fetchUserById = userId => {
  // the inside "thunk function"
  return async (dispatch, getState) => {
    try {
      // make an async call in the thunk
      const user = await userAPI.fetchById(userId)
      // dispatch an action when we get the response back
      dispatch(userLoaded(user))
    } catch (err) {
      // If something went wrong, handle it here
    }
  }
}
```

### The React Counter Component
Looking at the code we can see that the code works very much the same as a normal React component. 

``` 
  <button
    className={styles.button}
    aria-label="Increment value"
    onClick={() => dispatch(increment())}
  >
    +
  </button>
``` 

When the button is clicked, instead of updating local state, it uses `dispatch` to send an action to the store. In this case, it's the `type: counter/increment' action. The store will adjust the app's state accordingly.


So we can see that the component has some state of its own. 
```
  const count = useSelector(selectCount);
  const dispatch = useDispatch();
  const [incrementAmount, setIncrementAmount] = useState('2');
```

`count` is value kept in the store and is accessed with `useSelector`. 
`useSelector` is a hook that lets the component extract whatever data it needs from the Redux store state. In this case, it's using the `selectCount` selector from our counterSlice file.

```
export const selectCount = (state) => state.counter.value;
```

Anytime an action is dispatched and the Redux store updates state, `useSelector` will re-run the selector function. If the value is different, then `useSelector` prompts a rerender of the component.

### Component State and Forms

It's not necessary to keep an entire components state in the Redux store. Only stuff that the entire app will use should really go in the store.

In the case of forms, it's not necessary to actively update the store as the user edits. The better option is to keep it in component state and when the user is finished, dispatching the complete edit to store.


### Providing the Store

So `useSelector` and `useDispatch` hooks talk to the store, but how?
If we look at the starting point, index.js it's quite straight forward.
```
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import store from './app/store'
import { Provider } from 'react-redux'
import * as serviceWorker from './serviceWorker'

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
)
```
So the store we configured earlier is imported and passed to the `Provider` React component. 
The `Provider` component wraps around the `App` component and any hooks called within this talk to the store passed to the `Provider`.




 

  




  
 
