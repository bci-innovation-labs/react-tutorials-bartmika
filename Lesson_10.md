## Lesson 10 - Routes
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-10
  cd lesson-10
  npm start
  ```

### (2) Install dependency

In your console run the following.

```
npm install react-router-dom
```

### (3) Introductory Example

1. Update the ``src/index.js`` file to look like this.

    ```js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import * as serviceWorker from './serviceWorker';
    import { BrowserRouter } from 'react-router-dom';

    ReactDOM.render(
        <BrowserRouter>
            <App />
        </BrowserRouter>,
        document.getElementById('root')
    );

    // If you want your app to work offline and load faster, you can change
    // unregister() to register() below. Note this comes with some pitfalls.
    // Learn more about service workers: https://bit.ly/CRA-PWA
    serviceWorker.unregister();
    ```

2. Update the ``src/App.js`` file to look like this.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';


    import { BrowserRouter as Router, Route, Link } from "react-router-dom";

    // Main entry point into our application.
    function Index() {
      return <h2>Home</h2>;
    }

    // Another page.
    function About() {
      return <h2>About</h2>;
    }

    // Yet another page.
    function Users() {
      return <h2>Users</h2>;
    }


    class App extends Component {
      render() {
        return (
            <Router>
             <div>
               <nav>
                 <ul>
                   <li>
                     <Link to="/">Home</Link>
                   </li>
                   <li>
                     <Link to="/about/">About</Link>
                   </li>
                   <li>
                     <Link to="/users/">Users</Link>
                   </li>
                 </ul>
               </nav>

               <Route path="/" exact component={Index} />
               <Route path="/about/" component={About} />
               <Route path="/users/" component={Users} />
             </div>
           </Router>
        );
      }
    }

    export default App;
    ```

3. In your console run the app. You should see it work.

    ```bash
    npm start
    ```

### (4) Authentication Example

The following link has a great tutorial on handling routing:

* https://tylermcginnis.com/react-router-protected-routes-authentication/
