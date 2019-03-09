## Lesson 12 - Redux
### (1) Theory

* Read:

  * https://www.robinwieruch.de/react-redux-tutorial/

  * https://medium.freecodecamp.org/how-to-use-redux-in-reactjs-with-real-life-examples-687ab4441b85

### (1) Practice

1. Setup our repo.

    ```
    cd ~/javascript/bcinnovationlabs/
    create-react-app lesson-12a
    cd lesson-12a
    npm install --save redux react-redux
    npm start
    ```

2. Read the following link https://blog.tylerbuchea.com/super-simple-react-redux-application-example/ as the following section is copied from there.

3. Create the ``src/redux.js`` file and populate the following code:

    ```js
    import {
      combineReducers,
      createStore,
    } from 'redux';

    // actions.js
    export const activateGeod = geod => ({
      type: 'ACTIVATE_GEOD',
      geod,
    });

    export const closeGeod = () => ({
      type: 'CLOSE_GEOD',
    });

    // reducers.js
    export const geod = (state = {}, action) => {
      switch (action.type) {
        case 'ACTIVATE_GEOD':
          return action.geod;
        case 'CLOSE_GEOD':
          return {};
        default:
          return state;
      }
    };

    export const reducers = combineReducers({
      geod,
    });

    // store.js
    export function configureStore(initialState = {}) {
      const store = createStore(reducers, initialState);
      return store;
    };

    export const store = configureStore();
    ```

4. Update our ``src/App.js`` file with the following code:

    ```js
    import React from 'react';

    import { connect } from 'react-redux';

    import { activateGeod, closeGeod } from './redux';

    // App.js
    export class App extends React.Component {
      render() {
        return (
          <div>
            <h1>{this.props.geod.title || 'Hello World!'}</h1>

            {this.props.geod.title ? (
              <button onClick={this.props.closeGeod}>Exit Geod</button>
            ) : (
              <button
                onClick={() =>
                  this.props.activateGeod({ title: 'I am a geo dude!' })
                }
              >
                Click Me!
              </button>
            )}
          </div>
        );
      }
    }

    // AppContainer.js
    const mapStateToProps = state => ({
      geod: state.geod,
    });

    const mapDispatchToProps = {
      activateGeod,
      closeGeod,
    };

    const AppContainer = connect(
      mapStateToProps,
      mapDispatchToProps
    )(App);

    export default AppContainer;
    ```

5. Update the ``src/index.js`` with the follwing code:

    ```js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import App from './App';
    import './index.css';

    // Add these imports - Step 1
    import { Provider } from 'react-redux';
    import { store } from './redux';

    // Wrap existing app in Provider - Step 2
    ReactDOM.render(
      <Provider store={store}>
        <App />
      </Provider>,
      document.getElementById('root')
    );
    ```


### (2) Practice - Continued

1. Setup our repo.

    ```
    cd ~/javascript/bcinnovationlabs/
    create-react-app lesson-12b
    cd lesson-12b
    npm install --save redux react-redux redux-thunk
    npm start
    ```

2. We will be creating a simple app by following the instructions from https://blog.tylerbuchea.com/super-simple-react-redux-application-example/.

3. Create a file in ``src/redux.js`` and populate it with the following code:

    ```js
    import { applyMiddleware, combineReducers, createStore } from 'redux';

    import thunk from 'redux-thunk';

    // actions.js
    export const addRepos = repos => ({
      type: 'ADD_REPOS',
      repos,
    });

    export const clearRepos = () => ({ type: 'CLEAR_REPOS' });

    export const getRepos = username => async dispatch => {
      try {
        const url = `https://api.github.com/users/${username}/repos?sort=updated`;
        const response = await fetch(url);
        const responseBody = await response.json();
        dispatch(addRepos(responseBody));
      } catch (error) {
        console.error(error);
        dispatch(clearRepos());
      }
    };

    // reducers.js
    export const repos = (state = [], action) => {
      switch (action.type) {
        case 'ADD_REPOS':
          return action.repos;
        case 'CLEAR_REPOS':
          return [];
        default:
          return state;
      }
    };

    export const reducers = combineReducers({ repos });

    // store.js
    export function configureStore(initialState = {}) {
      const store = createStore(reducers, initialState, applyMiddleware(thunk));
      return store;
    }

    export const store = configureStore();
    ```

4. Copy and paste this code into your ``src/App.js`` file:

    ```js
    import React, { Component } from 'react';

    import { connect } from 'react-redux';

    import { getRepos } from './redux';

    // App.js
    export class App extends Component {
      state = { username: 'tylerbuchea' };

      componentDidMount() {
        this.updateRepoList(this.state.username);
      }

      updateRepoList = username => this.props.getRepos(username);

      render() {
        return (
          <div>
            <h1>I AM AN ASYNC APP!!!</h1>
            <strong>Github username: </strong>
            <input
              type="text"
              value={this.state.username}
              onChange={ev => this.setState({ username: ev.target.value })}
              placeholder="Github username..."
            />
            <button onClick={() => this.updateRepoList(this.state.username)}>
              Get Lastest Repos
            </button>
            <ul>
              {this.props.repos.map((repo, index) => (
                <li key={index}>
                  <a href={repo.html_url} target="_blank">
                    {repo.name}
                  </a>
                </li>
              ))}
            </ul>
          </div>
        );
      }
    }

    // AppContainer.js
    const mapStateToProps = (state, ownProps) => ({ repos: state.repos });
    const mapDispatchToProps = { getRepos };
    const AppContainer = connect(mapStateToProps, mapDispatchToProps)(App);

    export default AppContainer;
    ```

5. Update your ``src/index.css`` file to be like this:

    ```js
    body {
      margin: 0;
      padding: 0;
      font-family: sans-serif;
    }
    ```

6. Finally update your ``src/index.js`` file to be:

    ```js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import AppContainer from './App';
    import './index.css';

    // Add these imports - Step 1
    import { Provider } from 'react-redux';
    import { store } from './redux';

    // Wrap existing app in Provider - Step 2
    ReactDOM.render(
      <Provider store={store}>
        <AppContainer />
      </Provider>,
      document.getElementById('root')
    );
    ```
