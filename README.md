## Yet another Redux Action pattern in Typescript

```typescript
import {defineAction} from 'redux-action-objects';

// define the Application state type
interface AppState {/*...*/}

// create an ActionGroup that works on AppState
const actionGroup = createActionGroup<AppState>();

// add an action to the ActionGroup
const ClickAction = actionGroup.addAction<{
  // define payload here - you can also provide an explicit interface
  x: number
  y: number
}>({
  // We don't actually need to assign a type name - if omitted, the name will be generated randomly.
  // However, it is advisable to use human readable type for logging and debugging.
  type: 'ClickAction',

  // This is the reducer function that should be executed when the action is processed. It takes
  // the AppState and the action payload and should return an Appstate again.
  // Parameters and return types are already typechecked.
  reduce: (state, { x, y }) => {
    return { ...state, clicks: [ ...state.clicks, {x, y} ] }
  }
})

// Add any number of additional actions to actionGroup...

// create the redux store - using actionGroup.reducer as our reducer function. No switch case required!
const state: AppState = {/*...*/}
const store = createStore(actionGroup.reducer, state);

// create an action using the typesafe action creator, then dispatch it to store
const action = ClickAction.create({x: 23, y: 42})
store.dispatch(action);

```
