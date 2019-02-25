---
title: Note - Getting started with redux
date: 2019-02-21 17:41:03
thumbnail: 'https://cdn-images-1.medium.com/max/1600/1*VeM-5lsAtrrJ4jXH96h5kg.png'
tags: technical
---

I took notes during learning "Getting Started with Redux" by Dan Abramov - who created eact Hot Loader, Redux, React DnD...
The course is offered free on [egghead.io](https://egghead.io/courses/getting-started-with-redux). 

<!-- more -->

### The Single Immutable State Tree

1st principle: the whole state of the application is describe with single JS Object.

### Redux Describing State Changes with Actions

2nd principle: the state is readonly. Can not change the state without using action.

### Redux: Pure and Impure Functions

#### Pure functions: 
1. The return value of the pure functions solely depends on its arguments Hence, if you call the pure functions with the same set of arguments, you will always get the same return values.
2. They do not have any side effects like net­work or data­base calls
3. They do not mod­ify the arguments which are passed to them
  
#### Impure functions:
1. The return value of the impure functions does not solely depend on its arguments Hence, if you call the impure functions with the same set of arguments, you might get the different return values For example, Math.random(), Date.now()
2. They may have any side effects like net­work or data­base calls
3. They may modify the arguments which are passed to them

_Some of function in Redux need to be pure_

### Redux: The Reducer Function

Reducer the pure function takes the previous state, the action and return the next state of whole application.

### Redux: Writing a Counter Reducer with Tests

See video [here](https://egghead.io/lessons/react-redux-writing-a-counter-reducer-with-tests)

### Redux: Store Methods: getState(), dispatch(), and subscribe()

What is the store: 
- It holds the current application's state object.
- It lets you dispatch actions. 
- When you create it, you need to specify the reducer that tells how state is updated with actions.

Method of the store (getState, dispatch, subscribe):
- getState(): get the current state
- dispatch(): it lets you dispatch actions to change the state of your application.
- subscribe(): register a callback that the Redux store will call any time an action has been dispatched. Useful for tracking the current application state.

The basic implementation of the store is in the [video](https://egghead.io/lessons/react-redux-implementing-store-from-scratch).

### Redux: React Counter Example

_Note: smart component and dumb component_ 

Better to read on this [post](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0).
Dumb components: the same with presentational components
Smart components: is equal with container components

### Redux: Avoiding Array Mutations with concat(), slice(), and ...spread

Reducer is required to be not a mutation between input and output value. Using concat(), slice and ...spread operator can be useful.

```
// remove list[index]
const removeItem = (list, index) =>{
    return [
        ...list.slice(0, index),
        ...list.slice(index + 1),
    ];
}

// remove list[index]
const removeItem = (list, index) =>{
    return list
        .slice(0, index)
        .concat(list.slice(index+1));
}
```

### Redux: Avoiding Object Mutations with Object.assign() and ...spread

Object.assign() and ...spread can be used to avoid mutation by returning a new object.

```javascript

todo = {
    id: 0,
    text: 'Learn Redux',
    completed: true
};

const toggleTodo = (todo)=>{
    return Object.assign({}, todo, {completed: !todo.completed});
};

// is the same with the above code

const toggleTodo = (todo)=>{
    // because the last one wins
    return {...todo, completed: !todo.completed};
};

```

### (11 ~ 23) Should watch directly, include how to organize program to dummy & smart component 

### Passing the Store Down (Explicitly via Props, Implicitly via Context)

#### Explicitly way

```javascript

<Page store={store} />
// ... which renders ...
<PageLayout store={store} />
// ... which renders ...
<NavigationBar store={store} />
```

Passing like this can be complicated sometimes, for example when I only need to use the store inside the <NavigationBar> component but still need to pass it through <PageLayout> 

#### Implicitly way
Using Context & Provider to wrap the ```store``` inside a Context, so every child component inside can use the ```store```. Better to check this [link](https://reactjs.org/docs/context.html).


### Redux: Generating Containers with connect() from React Redux

```connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])```
Connect: creates a higher-order component for making container components out of base React components. It will mount both the state and dispatch inside the store to the props of component.

_Note:_ can pass null to mapStateToProps or mapDispatchToProps if you don't have any.

### End

P/S: I am using [React Slingshot](https://github.com/coryhouse/react-slingshot/) as a starter project; I do not take a lots of note in the last past because I already learnt from Cory House when using his starter.