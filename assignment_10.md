## Assignment 10
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app assignment-10
  cd assignment-10
  npm start
  ```

### (2) Task

1. Take the assignment 8 and add fake authentication which routes to a dashboard page.

2. Split the code up using a ES6 module.


### (3) Answer

### src/index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import { BrowserRouter } from 'react-router-dom';
import App from './App';
import * as serviceWorker from './serviceWorker';

ReactDOM.render(
    <BrowserRouter>
        <App />
    </BrowserRouter>,
    document.getElementById('root')
);

// If you want your app to work offline and load faster, you can change
// unregister() to register() below. Note this comes with some pitfalls.
// Learn more about service workers: http://bit.ly/CRA-PWA
serviceWorker.unregister();
```

### src/utils/Authentication.js

```js
import React, { Component } from 'react';
import {
    BrowserRouter as Router, Route, Link, Redirect
} from "react-router-dom";

import {withRouter} from "react-router-dom";


// Authentication middleware.
const fakeAuth = {
  authToken: localStorage.getItem("authToken"),
  isAuthenticated: false,
  authenticate(email, password, successCallback, failureCallback) {

    // For debugging purposes only.
    console.log("Sign in with", email, "using credentials", password,".");

    // Attempt to verify email and password and if it's true then
    // authenticate the user.
    if (email === "bart@mikasoftware.com" && password === "123password") {
        this.isAuthenticated = true
        setTimeout(successCallback, 100) // fake async
    } else {
        setTimeout(failureCallback, 100) // fake async
    }

  },
  signout(cb) {
    this.isAuthenticated = false
    setTimeout(cb, 100) // fake async
  }
}


export { fakeAuth };
```

### src/App.js

```js
import React, { Component } from 'react';
import {
    BrowserRouter as Router, Route, Link, Redirect
} from "react-router-dom";
import {withRouter} from "react-router-dom";

// import logo from './logo.svg';
import './App.css';

import {fakeAuth} from './utils/Authentication.js';


// Private routes
const PrivateRoute = ({ component: Component, ...rest }) => (
  <Route {...rest} render={(props) => (
    fakeAuth.isAuthenticated === true
      ? <Component {...props} />
      : <Redirect to='/login' />
  )} />
)


// Yet another page.
class DashboardPage extends Component {
  render() {
    return (
      <div>
        <h2>Dashboard</h2>
        <Link to="/">Home</Link>
      </div>
    );
  }
}


class LoginPage extends Component {
  constructor(props) {
    super(props);

    // Initialize our state.
    this.state = {
        email: '',
        password: '',
        referrer: null,
        hadFailedAttempt: false
    };

    // Bind the following functions.
    this.onSignIn = this.onSignIn.bind(this);
    this.onEmailChange = this.onEmailChange.bind(this);
    this.onPasswordChange = this.onPasswordChange.bind(this);
  }

  componentDidMount() {
      // Do nothing.
  }

  componentWillUnmount() {
      // Do nothing.
  }

  onSignIn() {
    const {email, password} = this.state; // Deconstruct our state items.
    const okCallback = () => {
        console.log("Attempting to log in...");

        // Assign the URL to redirect to.
        this.setState({referrer: '/dashboard'});
    }
    const failCallback = () => {
        this.setState({
            email: '',
            password: '',
            hadFailedAttempt: true
        });
    }

    fakeAuth.authenticate(email, password, okCallback, failCallback);
  }

  onEmailChange(event) {
    this.setState({ email: event.target.value });
  }

  onPasswordChange(event) {
    this.setState({ password: event.target.value });
  }

  render() {
      const { email, password, referrer, hadFailedAttempt } = this.state;

      // If a `referrer` was set then that means we can redirect
      // to a different page in our application.
      if (referrer) {
          return <Redirect to={referrer} />;
      }

      // If there was a failed attempt then we generate an error message.
      let errorMessage = "";
      if (hadFailedAttempt) {
        errorMessage = <strong>Wrong email or password</strong>
      }

      // Else render the graphics for this login panel.
      return (
          <form>
            {errorMessage}
            <p>
              Email:
              <input type="text" value={email} onChange={this.onEmailChange} />
            </p>
            <p>
              Password:
              <input type="password" value={password} onChange={this.onPasswordChange} />
            </p>
            <button onClick={() => this.onSignIn()} type="button">Sign In
            </button>

          </form>
      );
  }
}


class App extends Component {
  render() {
    return (
        <Router>
         <div>
           <Route path="" exact component={LoginPage} />
           <Route path="/login" exact component={LoginPage} />
           <PrivateRoute path="/dashboard" component={DashboardPage} />
         </div>
       </Router>
    );
  }
}

export default withRouter(App);
```
