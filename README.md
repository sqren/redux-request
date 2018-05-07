### Install

```
npm install @sqren/redux-request
```

### Getting started

```js
import { ReduxRequest, reduxRequestReducer } from '@sqren/redux-request';

// Add reduxRequestReducer to your store
const store = createStore((state = {}, action) => {
  return {
    ...state,
    reduxRequest: reduxRequestReducer(state.reduxRequest, action)
  };
});

// this method must return a promise
function getData() {
  return fetch(`https://jsonplaceholder.typicode.com/posts/1`).then(response =>
    response.json()
  );
}

// Render ReduxRequest
return (
  <ReduxRequest
    fn={getData}
    id="my-request-id"
    render={({ status, data, error }) => <div>{status}</div>}
  />
);
```

### Run examples locally
```
npm run storybook
```
Open [localhost:6006](http://localhost:6006) in your browser

All examples are located in [https://github.com/sqren/redux-request/tree/master/stories](stories/)


### API

#### args: array
A list of arguments that will be applied to `fn`
Example: `['a', 'b']`

#### fn: func
A function to fetch data. This must return a promise that resolves with the response. Arguments can be supplied with `args` props.

#### id: string
The identifier used to store the data in redux. If id is `selected-user`, the data will be stored in `store.reduxRequest['selected-user']`.
This is useful if you have data you want to display different places in your application. By relying on redux as a cahce, `reduxRequest` will only fetch the data the first time.

#### render: func
A so-called [render-prop](https://reactjs.org/docs/render-props.html) that will be called with the resolved or rejected value of `fn`. The render function is called with an object with these keys `{status, data, error}`

#### selector: func
This takes a selector (eg. [re-select](https://github.com/reduxjs/reselect)) that will be called with `state, { id }`, where `state` is the entire store state, and `id` is the identifier supplied to `reduxRequest`.
