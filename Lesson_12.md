## Lesson 12 - Redux
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-12
  cd lesson-12
  npm start
  ```

### (2) Theory

* Read https://www.robinwieruch.de/react-redux-tutorial/

### (3) Practice

1. Follow along reading https://medium.freecodecamp.org/how-to-use-redux-in-reactjs-with-real-life-examples-687ab4441b85.

2. Run the following.

    ```js
    create-react-app react-redux-tutorial
    cd react-redux-tutorial
    npm install
    npm start
    ```

3. Stop and run:

    ```js
    npm install --save redux react-redux
    ```

4. Create our files.

    ```bash
    mkdir src/actions
    touch src/actions/startAction.js
    touch src/actions/stopAction.js
    ```

5. Now let’s edit the ``src/actions/startAction.js`` as follows:

    ```js
    export const startAction = {
        type: "rotate",
        payload: true
    };
    ```

6. Now let’s edit the ``src/actions/stopAction.js`` as follows:

    ```js
    export const stopAction = {
        type: "rotate",
        payload: false
    };
    ```

7. Now lets create our reducers.

    ```bash
    mkdir src/reducers
    touch src/reducers/rotateReducer.js
    ```

8. And add the following to it.

    ```js
    export default (state, action) => {
        switch (action.type) {
          case "rotate":
            return {
              rotating: action.payload
            };
          default:
            return state;
        }
    };
    ```

9. Create our store.

    ```bash
    touch src/store.js
    ```

10. And populate that file with.

    ```js
    import { createStore } from "redux";
    import rotateReducer from "reducers/rotateReducer";
    function configureStore(state = { rotating: true }) {
      return createStore(rotateReducer,state);
    }
    export default configureStore;
    ```

11. Run the following.

    ```bash
    echo "NODE_PATH=./src" > .env
    ```

12. Inside the ``src/App.css`` file append the following:

    ```js
    .App-logo-paused {
        animation-play-state: paused;
    }
    ```

13. Inside our ``src/App.js`` file, copy and paste the following. We will add the redux action and integrate our ``App`` with the ``redux`` store.

    ```js
    import React, { Component } from 'react';
    import { connect } from "react-redux"; // STEP 1: Import our redux library stuff.
    import logo from './logo.svg';
    import './App.css';

    // STEP 2: Import our actions.
    import { startAction } from "actions/startAction";
    import { stopAction } from "actions/stopAction";

    class App extends Component {
      render() {
        return (
          <div className="App">
            <header className="App-header">
              <img src={logo} className="App-logo" alt="logo" />
              <p>
                Edit <code>src/App.js</code> and save to reload.
              </p>
              <a
                className="App-link"
                href="https://reactjs.org"
                target="_blank"
                rel="noopener noreferrer"
              >
                Learn React
              </a>
            </header>
          </div>
        );
      }
    }

    // STEP 3: Connect our application with the redux store.
    export default connect()(App);
    ```

14. At the end of the file add.

    ```js
    const mapStateToProps = state => ({
        ...state
    });
    const mapDispatchToProps = dispatch => ({
        startAction: () => dispatch(startAction),
        stopAction: () => dispatch(stopAction)
    });
    ```

15. At the very bottom, replace this line:

    ```js
    export default connect()(App);
    ```

16. With this code:

    ```js
    export default connect(mapStateToProps, mapDispatchToProps)(App);
    ```

17. Replace this code:

    ```html
     <img src={logo} className="App-logo" alt="logo" />
    ```

18. With this code:

    ```html
    <img
    src={logo}
    className={
      "App-logo" + (this.props.rotating ? "":" App-logo-paused")
    }
    alt="logo"
    onClick={
      this.props.rotating ?
        this.props.stopAction : this.props.startAction
    }
    />
    ```

19. Go to the following file ``src/index.js`` and edit the code to look like this:

    ```js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import * as serviceWorker from './serviceWorker';

    // STEP 1: Import our redux.
    import { Provider } from "react-redux";
    import configureStore from "store";

    // STEP 2: Integrate our store.
    ReactDOM.render(
      <Provider store={configureStore()}>
        <App />
      </Provider>,
      document.getElementById('root')
    );

    // If you want your app to work offline and load faster, you can change
    // unregister() to register() below. Note this comes with some pitfalls.
    // Learn more about service workers: https://bit.ly/CRA-PWA
    serviceWorker.unregister();
    ```

20. Also lets integrate our application with default configuration stores. The code will look like this.

    ```js
    import React from 'react';

    ```
