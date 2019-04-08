# Introduction

![](../logo/logo.png)

Redux Preboiled is a collection of boilerplate-reducing helper functions
for [Redux][redux] applications. It is built with the following goals in
mind:

- **_À la carte_ design.** Preboiled's helpers are designed to be largely
  independent from each other, which allows you to pick and choose
  the ones you need without being burdened by the rest.

- **Minimal magic.** While Preboiled should reduce the code needed for
  common Redux patterns, it shouldn't do so at the expense of readability.
  Clever metaprogramming techniques and other magic are avoided in favor of
  straight-forward functions with easy-to-understand implementations.

- **Typing friendliness.** Preboiled is written in [TypeScript][ts]
  and maximizes its benefits by avoiding patterns that are hard to type
  or sacrifice type information (e.g., dynamically generated property
  names). It also minimizes typing effort by allowing for type inference
  wherever possible.

## A First Taste

Consider the following vanilla Redux code for a simple counter:

```js
// Action Types

const INCREMENT = 'increment'
const MULTIPLY = 'multiply'

// Action Creators

function increment() {
  return {
    type: INCREMENT
  }
}

function multiply(amount) {
  return {
    type: MULTIPLY,
    payload: amount
  }
}

// Reducer

function counterReducer(state = 0, action) {
  switch (action.type) {
    case INCREMENT:
      return state + 1
    case MULTIPLY:
      return state * action.payload
    default:
      return state
  }
}
```

With Redux Preboiled, this can be reduced to:

```js
import {
  chainReducers,
  createAction,
  onAction,
  withInitialState
} from 'redux-preboiled'

// Actions

const increment = createAction('increment')
const multiply = createAction('multiply').withPayload()

// Reducer

const counterReducer = chainReducers(
  withInitialState(0),
  onAction(increment, state => state + 1),
  onAction(multiply, (state, { payload }) => state * payload)
)
```

* Using the `createAction` helper, we can define the action creators with just
  one line each. Note that we don't lose the [benefits of separate action type
  constants][redux-action-types] as Preboiled makes the action types available
  as `increment.type` and `multiply.type`, respectively. See the
  [Actions](./guides/actions.md) guide.

* We combine `onAction`, `withInitialState` and `chainReducers` for a slightly
  less noisy alternative to the `switch` reducer pattern. In TypeScript, this
  variant is also more type-safe: based on the type of the action creator passed
  to `onAction`, the compiler can automatically infer the type of the handler
  function's `action` parameter. See the [Reducers](./docs/reducers.md) guide.

## Next Steps

The [Getting Started](./guides/getting-started.md) guide shows you how to install and use Redux Preboiled. From there you can take a tour through the offered helpers by following the links to the other guides.

For reference documentation, see to the [API docs](./api/README.md).

[redux]: https://redux.js.org/
[redux-action-types]: https://redux.js.org/faq/actions#why-should-type-be-a-string-or-at-least-serializable-why-should-my-action-types-be-constants
[ts]: https://www.typescriptlang.org/