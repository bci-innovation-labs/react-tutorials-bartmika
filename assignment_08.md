## Assignment 08
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app assignment-08
  cd assignment-08
  npm start
  ```

### (2) Task

1. Create a react app which has a email / password field and a login button.

2. Save the email / password when changed

3. When the **login** button is clicked, the email and password should be displayed.



### (3) Answer

```js
import React, { Component } from 'react';
// import { BrowserRouter as Router, Route, Link } from "react-router-dom";
// import logo from './logo.svg';
import './App.css';


class RegisterPage extends Component {
    render() {
        return (
            <div>
            </div>
        );
    }
}

class LoginPage extends Component {
  constructor(props) {
    super(props);

    this.state = {
        email: '',
        password: ''
    };

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
    console.log("Sign in with", this.state.email, "using credentials", this.state.password,".");
  }

  onEmailChange(event) {
    this.setState({ email: event.target.value });
  }

  onPasswordChange(event) {
    this.setState({ password: event.target.value });
  }

  render() {
      const { email, password } = this.props;
      return (
          <form>
            <p>
              Email:
              <input type="text" value={email} onChange={this.onEmailChange} />
            </p>
            <p>
              Password:
              <input type="text" password={password} onChange={this.onPasswordChange} />
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
      <div className="App">
        <LoginPage />
      </div>
    );
  }
}

export default App;

```
