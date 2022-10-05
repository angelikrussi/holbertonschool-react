# `connect()`

##### TIP

`connect`  still works and is supported in React-Redux 8.x. However,  [**we recommend using the hooks API as the default**](https://react-redux.js.org/api/hooks).

## Overview[​](https://react-redux.js.org/api/connect#overview "Direct link to heading")

The  `connect()`  function connects a React component to a Redux store.

It provides its connected component with the pieces of the data it needs from the store, and the functions it can use to dispatch actions to the store.

It does not modify the component class passed to it; instead, it returns a new, connected component class that wraps the component you passed in.

```
function connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)
```


The  `mapStateToProps`  and  `mapDispatchToProps`  deals with your Redux store’s  `state`  and  `dispatch`, respectively.  `state`  and  `dispatch`  will be supplied to your  `mapStateToProps`  or  `mapDispatchToProps`  functions as the first argument.

The returns of  `mapStateToProps`  and  `mapDispatchToProps`  are referred to internally as  `stateProps`  and  `dispatchProps`, respectively. They will be supplied to  `mergeProps`, if defined, as the first and the second argument, where the third argument will be  `ownProps`. The combined result, commonly referred to as  `mergedProps`, will then be supplied to your connected component.

## `connect()`  Parameters[​](https://react-redux.js.org/api/connect#connect-parameters "Direct link to heading")

`connect`  accepts four different parameters, all optional. By convention, they are called:

1.  `mapStateToProps?: Function`
2.  `mapDispatchToProps?: Function | Object`
3.  `mergeProps?: Function`
4.  `options?: Object`

### `mapStateToProps?: (state, ownProps?) => Object`[​](https://react-redux.js.org/api/connect#mapstatetoprops-state-ownprops--object "Direct link to heading")

If a  `mapStateToProps`  function is specified, the new wrapper component will subscribe to Redux store updates. This means that any time the store is updated,  `mapStateToProps`  will be called. The results of  `mapStateToProps`  must be a plain object, which will be merged into the wrapped component’s props. If you don't want to subscribe to store updates, pass  `null`  or  `undefined`  in place of  `mapStateToProps`.

#### Parameters[​](https://react-redux.js.org/api/connect#parameters "Direct link to heading")

1.  `state: Object`
2.  `ownProps?: Object`

A  `mapStateToProps`  function takes a maximum of two parameters. The number of declared function parameters (a.k.a. arity) affects when it will be called. This also determines whether the function will receive ownProps. See notes  [here](https://react-redux.js.org/api/connect#the-arity-of-maptoprops-functions).

##### `state`[​](https://react-redux.js.org/api/connect#state "Direct link to heading")

If your  `mapStateToProps`  function is declared as taking one parameter, it will be called whenever the store state changes, and given the store state as the only parameter.

```
const mapStateToProps = (state) => ({ todos: state.todos })
```


##### `ownProps`[​](https://react-redux.js.org/api/connect#ownprops "Direct link to heading")

If your  `mapStateToProps`  function is declared as taking two parameters, it will be called whenever the store state changes  _or_  when the wrapper component receives new props (based on shallow equality comparisons). It will be given the store state as the first parameter, and the wrapper component's props as the second parameter.

The second parameter is normally referred to as  `ownProps`  by convention.

```
const mapStateToProps = (state, ownProps) => ({  todo: state.todos[ownProps.id],})
```


#### Returns[​](https://react-redux.js.org/api/connect#returns "Direct link to heading")

Your  `mapStateToProps`  functions are expected to return an object. This object, normally referred to as  `stateProps`, will be merged as props to your connected component. If you define  `mergeProps`, it will be supplied as the first parameter to  `mergeProps`.

The return of the  `mapStateToProps`  determine whether the connected component will re-render (details  [here](https://react-redux.js.org/using-react-redux/connect-mapstate#return-values-determine-if-your-component-re-renders)).

For more details on recommended usage of  `mapStateToProps`, please refer to  [our guide on using  `mapStateToProps`](https://react-redux.js.org/using-react-redux/connect-mapstate).

> You may define  `mapStateToProps`  and  `mapDispatchToProps`  as a factory function, i.e., you return a function instead of an object. In this case your returned function will be treated as the real  `mapStateToProps`  or  `mapDispatchToProps`, and be called in subsequent calls. You may see notes on  [Factory Functions](https://react-redux.js.org/api/connect#factory-functions)  or our guide on performance optimizations.

### `mapDispatchToProps?: Object | (dispatch, ownProps?) => Object`[​](https://react-redux.js.org/api/connect#mapdispatchtoprops-object--dispatch-ownprops--object "Direct link to heading")

Conventionally called  `mapDispatchToProps`, this second parameter to  `connect()`  may either be an object, a function, or not supplied.

Your component will receive  `dispatch`  by default, i.e., when you do not supply a second parameter to  `connect()`:

```
// do not pass `mapDispatchToProps`connect()(MyComponent)connect(mapState)(MyComponent)connect(mapState, null, mergeProps, options)(MyComponent)
```


If you define a  `mapDispatchToProps`  as a function, it will be called with a maximum of two parameters.

#### Parameters[​](https://react-redux.js.org/api/connect#parameters-1 "Direct link to heading")

1.  `dispatch: Function`
2.  `ownProps?: Object`

##### `dispatch`[​](https://react-redux.js.org/api/connect#dispatch "Direct link to heading")

If your  `mapDispatchToProps`  is declared as a function taking one parameter, it will be given the  `dispatch`  of your  `store`.

```
const mapDispatchToProps = (dispatch) => {  return {    // dispatching plain actions    increment: () => dispatch({ type: 'INCREMENT' }),    decrement: () => dispatch({ type: 'DECREMENT' }),    reset: () => dispatch({ type: 'RESET' }),  }}
```


##### `ownProps`[​](https://react-redux.js.org/api/connect#ownprops-1 "Direct link to heading")

If your  `mapDispatchToProps`  function is declared as taking two parameters, it will be called with  `dispatch`  as the first parameter and the props passed to the wrapper component as the second parameter, and will be re-invoked whenever the connected component receives new props.

The second parameter is normally referred to as  `ownProps`  by convention.

```
// binds on component re-rendering<button onClick={() => this.props.toggleTodo(this.props.todoId)} />// binds on `props` changeconst mapDispatchToProps = (dispatch, ownProps) => ({  toggleTodo: () => dispatch(toggleTodo(ownProps.todoId)),})
```


The number of declared function parameters of  `mapDispatchToProps`  determines whether they receive ownProps. See notes  [here](https://react-redux.js.org/api/connect#the-arity-of-maptoprops-functions).

#### Returns[​](https://react-redux.js.org/api/connect#returns-1 "Direct link to heading")

Your  `mapDispatchToProps`  functions are expected to return an object. Each fields of the object should be a function, calling which is expected to dispatch an action to the store.

The return of your  `mapDispatchToProps`  functions are regarded as  `dispatchProps`. It will be merged as props to your connected component. If you define  `mergeProps`, it will be supplied as the second parameter to  `mergeProps`.

```
const createMyAction = () => ({ type: 'MY_ACTION' })const mapDispatchToProps = (dispatch, ownProps) => {  const boundActions = bindActionCreators({ createMyAction }, dispatch)  return {    dispatchPlainObject: () => dispatch({ type: 'MY_ACTION' }),    dispatchActionCreatedByActionCreator: () => dispatch(createMyAction()),    ...boundActions,    // you may return dispatch here    dispatch,  }}
```


For more details on recommended usage, please refer to  [our guide on using  `mapDispatchToProps`](https://react-redux.js.org/using-react-redux/connect-mapdispatch).

> You may define  `mapStateToProps`  and  `mapDispatchToProps`  as a factory function, i.e., you return a function instead of an object. In this case your returned function will be treated as the real  `mapStateToProps`  or  `mapDispatchToProps`, and be called in subsequent calls. You may see notes on  [Factory Functions](https://react-redux.js.org/api/connect#factory-functions)  or our guide on performance optimizations.

#### Object Shorthand Form[​](https://react-redux.js.org/api/connect#object-shorthand-form "Direct link to heading")

`mapDispatchToProps`  may be an object where each field is an  [action creator](https://redux.js.org/glossary#action-creator).

```
import { addTodo, deleteTodo, toggleTodo } from './actionCreators'const mapDispatchToProps = {  addTodo,  deleteTodo,  toggleTodo,}export default connect(null, mapDispatchToProps)(TodoApp)
```


In this case, React-Redux binds the  `dispatch`  of your store to each of the action creators using  `bindActionCreators`. The result will be regarded as  `dispatchProps`, which will be either directly merged to your connected components, or supplied to  `mergeProps`  as the second argument.

```
// internally, React-Redux calls bindActionCreators// to bind the action creators to the dispatch of your storebindActionCreators(mapDispatchToProps, dispatch)
```


We also have a section in our  `mapDispatchToProps`  guide on the usage of object shorthand form  [here](https://react-redux.js.org/using-react-redux/connect-mapdispatch#defining-mapdispatchtoprops-as-an-object).

### `mergeProps?: (stateProps, dispatchProps, ownProps) => Object`[​](https://react-redux.js.org/api/connect#mergeprops-stateprops-dispatchprops-ownprops--object "Direct link to heading")

If specified, defines how the final props for your own wrapped component are determined. If you do not provide  `mergeProps`, your wrapped component receives  `{ ...ownProps, ...stateProps, ...dispatchProps }`  by default.

#### Parameters[​](https://react-redux.js.org/api/connect#parameters-2 "Direct link to heading")

`mergeProps`  should be specified with maximum of three parameters. They are the result of  `mapStateToProps()`,  `mapDispatchToProps()`, and the wrapper component's  `props`, respectively:

1.  `stateProps`
2.  `dispatchProps`
3.  `ownProps`

The fields in the plain object you return from it will be used as the props for the wrapped component. You may specify this function to select a slice of the state based on props, or to bind action creators to a particular variable from props.

#### Returns[​](https://react-redux.js.org/api/connect#returns-2 "Direct link to heading")

The return value of  `mergeProps`  is referred to as  `mergedProps`  and the fields will be used as the props for the wrapped component.

> Note: Creating new values in mergeProps will cause re-renders. It is recommended that you memoize fields in order to avoid unnecessary re-renders.

### `options?: Object`[​](https://react-redux.js.org/api/connect#options-object "Direct link to heading")

```
{  context?: Object,  areStatesEqual?: Function,  areOwnPropsEqual?: Function,  areStatePropsEqual?: Function,  areMergedPropsEqual?: Function,  forwardRef?: boolean,}
```


#### `context: Object`[​](https://react-redux.js.org/api/connect#context-object "Direct link to heading")

> Note: This parameter is supported in >= v6.0 only

React-Redux v6 allows you to supply a custom context instance to be used by React-Redux. You need to pass the instance of your context to both  `<Provider />`  and your connected component. You may pass the context to your connected component either by passing it here as a field of option, or as a prop to your connected component in rendering.

```
// const MyContext = React.createContext();connect(mapStateToProps, mapDispatchToProps, null, { context: MyContext })(  MyComponent)
```


#### `areStatesEqual: (next: Object, prev: Object) => boolean`[​](https://react-redux.js.org/api/connect#arestatesequal-next-object-prev-object--boolean "Direct link to heading")

-   default value:  `strictEqual: (next, prev) => prev === next`

Compares incoming store state to its previous value.

```
const areStatesEqual = (next, prev) =>  prev.entities.todos === next.entities.todos
```


You may wish to override  `areStatesEqual`  if your  `mapStateToProps`  function is computationally expensive and is also only concerned with a small slice of your state. The example above will effectively ignore state changes for everything but that slice of state.

This would likely impact the other equality checks as well, depending on your  `mapStateToProps`  function.

#### `areOwnPropsEqual: (next: Object, prev: Object) => boolean`[​](https://react-redux.js.org/api/connect#areownpropsequal-next-object-prev-object--boolean "Direct link to heading")

-   default value:  `shallowEqual: (objA, objB) => boolean`  ( returns  `true`  when each field of the objects is equal )

Compares incoming props to its previous value.

You may wish to override  `areOwnPropsEqual`  as a way to whitelist incoming props. You'd also have to implement  `mapStateToProps`,  `mapDispatchToProps`  and  `mergeProps`  to also whitelist props. (It may be simpler to achieve this other ways, for example by using  [recompose's mapProps](https://github.com/acdlite/recompose/blob/master/docs/API.md#mapprops).)

#### `areStatePropsEqual: (next: Object, prev: Object) => boolean`[​](https://react-redux.js.org/api/connect#arestatepropsequal-next-object-prev-object--boolean "Direct link to heading")

-   type:  `function`
-   default value:  `shallowEqual`

Compares the result of  `mapStateToProps`  to its previous value.

#### `areMergedPropsEqual: (next: Object, prev: Object) => boolean`[​](https://react-redux.js.org/api/connect#aremergedpropsequal-next-object-prev-object--boolean "Direct link to heading")

-   default value:  `shallowEqual`

Compares the result of  `mergeProps`  to its previous value.

You may wish to override  `areStatePropsEqual`  to use  `strictEqual`  if your  `mapStateToProps`  uses a memoized selector that will only return a new object if a relevant prop has changed. This would be a very slight performance improvement, since would avoid extra equality checks on individual props each time  `mapStateToProps`  is called.

You may wish to override  `areMergedPropsEqual`  to implement a  `deepEqual`  if your selectors produce complex props. ex: nested objects, new arrays, etc. (The deep equal check may be faster than just re-rendering.)

#### `forwardRef: boolean`[​](https://react-redux.js.org/api/connect#forwardref-boolean "Direct link to heading")

> Note: This parameter is supported in >= v6.0 only

If  `{forwardRef : true}`  has been passed to  `connect`, adding a ref to the connected wrapper component will actually return the instance of the wrapped component.

## `connect()`  Returns[​](https://react-redux.js.org/api/connect#connect-returns "Direct link to heading")

The return of  `connect()`  is a wrapper function that takes your component and returns a wrapper component with the additional props it injects.

```
import { login, logout } from './actionCreators'const mapState = (state) => state.userconst mapDispatch = { login, logout }// first call: returns a hoc that you can use to wrap any componentconst connectUser = connect(mapState, mapDispatch)// second call: returns the wrapper component with mergedProps// you may use the hoc to enable different components to get the same behaviorconst ConnectedUserLogin = connectUser(Login)const ConnectedUserProfile = connectUser(Profile)
```



In most cases, the wrapper function will be called right away, without being saved in a temporary variable:

```
import { login, logout } from './actionCreators'const mapState = (state) => state.userconst mapDispatch = { login, logout }// call connect to generate the wrapper function, and immediately call// the wrapper function to generate the final wrapper component.export default connect(mapState, mapDispatch)(Login)
```


## Example Usage[​](https://react-redux.js.org/api/connect#example-usage "Direct link to heading")

Because  `connect`  is so flexible, it may help to see some additional examples of how it can be called:

-   Inject just  `dispatch`  and don't listen to store

```
export default connect()(TodoApp)
```


-   Inject all action creators (`addTodo`,  `completeTodo`, ...) without subscribing to the store

```
import * as actionCreators from './actionCreators'export default connect(null, actionCreators)(TodoApp)
```


-   Inject  `dispatch`  and every field in the global state

> Don’t do this! It kills any performance optimizations because  `TodoApp`  will rerender after every state change. It’s better to have more granular  `connect()`  on several components in your view hierarchy that each only listen to a relevant slice of the state.

```
// don't do this!export default connect((state) => state)(TodoApp)
```


-   Inject  `dispatch`  and  `todos`

```
function mapStateToProps(state) {  return { todos: state.todos }}export default connect(mapStateToProps)(TodoApp)
```


-   Inject  `todos`  and all action creators

```
import * as actionCreators from './actionCreators'function mapStateToProps(state) {  return { todos: state.todos }}export default connect(mapStateToProps, actionCreators)(TodoApp)
```


-   Inject  `todos`  and all action creators (`addTodo`,  `completeTodo`, ...) as  `actions`

```
import * as actionCreators from './actionCreators'import { bindActionCreators } from 'redux'function mapStateToProps(state) {  return { todos: state.todos }}function mapDispatchToProps(dispatch) {  return { actions: bindActionCreators(actionCreators, dispatch) }}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)
```

-   Inject  `todos`  and a specific action creator (`addTodo`)

```
import { addTodo } from './actionCreators'import { bindActionCreators } from 'redux'function mapStateToProps(state) {  return { todos: state.todos }}function mapDispatchToProps(dispatch) {  return bindActionCreators({ addTodo }, dispatch)}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)
```


-   Inject  `todos`  and specific action creators (`addTodo`  and  `deleteTodo`) with shorthand syntax

```
import { addTodo, deleteTodo } from './actionCreators'function mapStateToProps(state) {  return { todos: state.todos }}const mapDispatchToProps = {  addTodo,  deleteTodo,}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)


-   Inject  `todos`,  `todoActionCreators`  as  `todoActions`, and  `counterActionCreators`  as  `counterActions`

```
import * as todoActionCreators from './todoActionCreators'import * as counterActionCreators from './counterActionCreators'import { bindActionCreators } from 'redux'function mapStateToProps(state) {  return { todos: state.todos }}function mapDispatchToProps(dispatch) {  return {    todoActions: bindActionCreators(todoActionCreators, dispatch),    counterActions: bindActionCreators(counterActionCreators, dispatch),  }}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)
```


-   Inject  `todos`, and todoActionCreators and counterActionCreators together as  `actions`

```
import * as todoActionCreators from './todoActionCreators'import * as counterActionCreators from './counterActionCreators'import { bindActionCreators } from 'redux'function mapStateToProps(state) {  return { todos: state.todos }}function mapDispatchToProps(dispatch) {  return {    actions: bindActionCreators(      { ...todoActionCreators, ...counterActionCreators },      dispatch    ),  }}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)
```



-   Inject  `todos`, and all  `todoActionCreators`  and  `counterActionCreators`  directly as props

```
import * as todoActionCreators from './todoActionCreators'import * as counterActionCreators from './counterActionCreators'import { bindActionCreators } from 'redux'function mapStateToProps(state) {  return { todos: state.todos }}function mapDispatchToProps(dispatch) {  return bindActionCreators(    { ...todoActionCreators, ...counterActionCreators },    dispatch  )}export default connect(mapStateToProps, mapDispatchToProps)(TodoApp)


-   Inject  `todos`  of a specific user depending on props

```
import * as actionCreators from './actionCreators'function mapStateToProps(state, ownProps) {  return { todos: state.todos[ownProps.userId] }}export default connect(mapStateToProps)(TodoApp)
```


-   Inject  `todos`  of a specific user depending on props, and inject  `props.userId`  into the action

```
import * as actionCreators from './actionCreators'function mapStateToProps(state) {  return { todos: state.todos }}function mergeProps(stateProps, dispatchProps, ownProps) {  return Object.assign({}, ownProps, {    todos: stateProps.todos[ownProps.userId],    addTodo: (text) => dispatchProps.addTodo(ownProps.userId, text),  })}export default connect(mapStateToProps, actionCreators, mergeProps)(TodoApp)
```


## Notes[​](https://react-redux.js.org/api/connect#notes "Direct link to heading")

### The Arity of  `mapToProps`  Functions[​](https://react-redux.js.org/api/connect#the-arity-of-maptoprops-functions "Direct link to heading")

The number of declared function parameters of  `mapStateToProps`  and  `mapDispatchToProps`  determines whether they receive  `ownProps`

> Note:  `ownProps`  is not passed to  `mapStateToProps`  and  `mapDispatchToProps`  if the formal definition of the function contains one mandatory parameter (function has length 1). For example, functions defined like below won't receive  `ownProps`  as the second argument. If the incoming value of  `ownProps`  is  `undefined`, the default argument value will be used.

```
function mapStateToProps(state) {  console.log(state) // state  console.log(arguments[1]) // undefined}const mapStateToProps = (state, ownProps = {}) => {  console.log(state) // state  console.log(ownProps) // {}}
```


Functions with no mandatory parameters or two parameters*will receive  `ownProps`.

```
const mapStateToProps = (state, ownProps) => {  console.log(state) // state  console.log(ownProps) // ownProps}function mapStateToProps() {  console.log(arguments[0]) // state  console.log(arguments[1]) // ownProps}const mapStateToProps = (...args) => {  console.log(args[0]) // state  console.log(args[1]) // ownProps}

### Factory Functions[​](https://react-redux.js.org/api/connect#factory-functions "Direct link to heading")

If your  `mapStateToProps`  or  `mapDispatchToProps`  functions return a function, they will be called once when the component instantiates, and their returns will be used as the actual  `mapStateToProps`,  `mapDispatchToProps`, functions respectively, in their subsequent calls.

The factory functions are commonly used with memoized selectors. This gives you the ability to create component-instance-specific selectors inside the closure:

```
const makeUniqueSelectorInstance = () =>  createSelector([selectItems, selectItemId], (items, itemId) => items[itemId])const makeMapState = (state) => {  const selectItemForThisComponent = makeUniqueSelectorInstance()  return function realMapState(state, ownProps) {    const item = selectItemForThisComponent(state, ownProps.itemId)    return { item }  }}export default connect(makeMapState)(SomeComponent)
```


## Legacy Version Docs[​](https://react-redux.js.org/api/connect#legacy-version-docs "Direct link to heading")

While the  `connect`  API has stayed almost entirely API-compatible between all of our major versions, there have been some small changes in options and behavior from version to version.