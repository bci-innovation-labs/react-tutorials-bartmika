## Lesson 03 - JSX Continued
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-03
  cd lesson-03
  npm start
  ```

### (2) JSX: Embedding Expressions

1. We are able to **embed** variables within variables. Look in the ``src/App.js`` file and write:

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        var firstName = "Bart";
        var lastName = "Mika";
        var element = <p><strong>Full-name:</strong> {firstName} {lastName}</p>
        return (
          <div className="App">
            <h1>Cooking Pot: Profile</h1>
            {element}
          </div>
        );
      }
    }

    export default App;
    ```

2. ``JSX`` allows this sort of embedding behaviour. Also you can use arrays and dictionary objects too. Here is an example.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        var user = {
            firstName: 'Bartlomiej',
            lastName: 'Mika'
        }
        var element = <p><strong>Full-name:</strong> {user.firstName} {user.lastName}</p>
        return (
          <div className="App">
            <h1>Cooking Pot: Profile</h1>
            {element}
          </div>
        );
      }
    }

    export default App;
    ```

3. This is also acceptable **embedding**.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        var user = {
            firstName: 'Frank',
            lastName: 'Herbert',
            email: 'frank@dune.com'
        }
        return (
          <div className="App">
            <h1>Cooking Pot: Profile</h1>
            <p><strong>Full-name:</strong> {user.firstName} {user.lastName}</p>
            <p><strong>Email:</strong> {user.email}</p>
          </div>
        );
      }
    }

    export default App;
    ```

### (3) JSX: Function Expressions

1. You can create functions as well! Here is an example of a function to print the user's full name.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        function getFullName(user) {
            return user.firstName+" "+user.lastName;
        }

        var user = {
            firstName: 'Frank',
            lastName: 'Herbert'
        }
        return (
          <div className="App">
            <h1>Cooking Pot: Profile</h1>
            <p><strong>Full-name:</strong> {getFullName(user)}</p>

          </div>
        );
      }
    }

    export default App;
    ```

2. You can add conditional logic as well. Here is an example.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        function getFullName(user) {
            return user.firstName+" "+user.lastName;
        }

        function getGreeting(user) {
            if (user) {
                return <h2>Hello, {getFullName(user)}!</h2>;
            }
            return <h2>Hello, Stranger.</h2>;
        }

        var user = {
            firstName: 'Frank',
            lastName: 'Herbert'
        }
        return (
          <div className="App">
            <h1>Cooking Pot: Login Page</h1>
            {getGreeting(user)}
          </div>
        );
      }
    }

    export default App;
    ```

3. Try removing the "user" from the ``{getGreeting(user)}`` line and see what happens.

### (4) Updating HTML Attributes

1. In the previous sections we updated the **HTML Element** but now we will update the **HTML attribute**. Update your file to look like this:

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        var user = {
            firstName: 'Bart',
            lastName: 'Mika',
            email: 'bart@email.com',
            avatarUrl: 'https://avatars3.githubusercontent.com/u/5767602?s=460&v=4'
        }
        return (
          <div className="App">
            <h1>Cooking Pot: User Profile</h1>
            <p><strong>Full-name:</strong> {user.firstName} {user.lastName}</p>
            <p><strong>Email:</strong> {user.email}</p>
            <p>
                <strong>Profile:</strong><img src={user.avatarUrl}></img>
            </p>
          </div>
        );
      }
    }

    export default App;
    ```

### (5) Grandparent / Parent / Child Relationship

1. Take a look at the following code.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        const element = (
            <div>
                <h1>Hello!</h1>
                <h2>Good to see you here.</h2>
            </div>
        );
        return (
          <div className="App">
            {element}
          </div>
        );
      }
    }

    export default App;
    ```

2. We see the parent is:

    ```
    <div>
       // ...
    </div>
    ```

3. And children 1 is:

    ```
       <h1>Hello!</h1>
    ```

3. And children 2 is:

    ```
        <h2>Good to see you here.</h2>
    ```

### (6) Homework

1. Prepare for next tutorial.

    * https://reactjs.org/docs/components-and-props.html
