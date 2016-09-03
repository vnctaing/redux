# Usage with React Router

So you want to do routing with your Redux app. You can use it with the library react-router. Redux will be the source of truth for your data and react-router will be the source of truth for your URL. In most of the cases, **it is fine** to have them separate unless if you need to time travel and rewind actions that triggers the URL.

## Redux-router, react-router-redux, react-router
Before starting, let's clarify the different routing libraries.

### Redux-router
Redux-router is an **experimental** library, it lets you keep entirely the state of your url inside your redux store. It has the same API with React Router API, has a smaller community support than react-router.

### React-router-redux
react-router-redux creates binding between your redux app and react-router and it keeps them in sync. Without this binding, you will not be able to rewind the actions with Time Travel. Unless you need this, React-router and Redux can operates completely apart.


## Installing react-router
react-router is available on npm :

`npm install --save react-router`


## Connecting the router with Redux App

We will be using the [Todos](https://github.com/reactjs/redux/tree/master/examples/todos) example.

The <Router /> component has to be a children of <Provider/> (the higher-order component provided by react-redux that lets you bind Redux to React, see [Usage with React](../basics/UsageWithReact.md)) so the `<Router />` has access to the global `store`.

The `<Route>` component lets you define a component to be loaded whenever an url match with the property `path`. We added the optional `(:filter)` parameter so it will renders the <App> component if the url match '/'.

Passing the `browserHistory` is necessary if you want to remove the hash from URL the `#/?_k=4sbb0i`. Unless you are targeting old browsers like IE9, you can always use `browserHistory`.

``` js
import React, { PropTypes } from 'react';
import { Provider } from 'react-redux';
import { Router, Route, browserHistory } from 'react-router';
import App from './App';

const Root = ({ store }) => (
  <Provider store={store}>
    <Router history={browserHistory}>
      <Route path="/(:filter)" component={App} />
    </Router>
  </Provider>
);

Root.propTypes = {
  store: PropTypes.object.isRequired,
};

export default Root;
```

## Navigating with react-router

react-router comes with a [<Link/>](https://github.com/reactjs/react-router/blob/master/docs/API.md#link) component that let you navigate around your application. We can use it in our example and change our container `<FilterLink />` component so we can change the URL using `<FilterLink />. The `activeStyle={}` property lets you apply a style on the active state.


#### `containers/FilterLink.js`
```js
import React from 'react';
import { Link } from 'react-router';

const FilterLink = ({ filter, children }) => (
  <Link
    to={filter === 'all' ? '' : filter}
    activeStyle={{
      textDecoration: 'none',
      color: 'black'
    }}
  >
    {children}
  </Link>
);

export default FilterLink;
```

#### `containers/Footer`
```js
import React from 'react'
import FilterLink from '../containers/FilterLink'

const Footer = () => (
  <p>
    Show:
    {" "}
    <FilterLink filter="all">
      All
    </FilterLink>
    {", "}
    <FilterLink filter="active">
      Active
    </FilterLink>
    {", "}
    <FilterLink filter="completed">
      Completed
    </FilterLink>
  </p>
)

export default Footer
```

Now if you click on `<FilterLink/>` you will see that your URL will change from '/complete', '/active', '/'. Even if you are going back with your browser, it will use your browser's history.
