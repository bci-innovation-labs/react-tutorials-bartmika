## Lesson 04 - Components
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-04
  cd lesson-04
  npm start
  ```

### (2) What is a component?

* Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.

* Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called “props”) and return React elements describing what should appear on the screen.

* Source: https://reactjs.org/docs/components-and-props.html

### (3) Function based

1. Look in the ``src/App.js`` file and copy and paste the following:

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        // This is a function-based component. It will return a React "element".
        function Welcome(props) {
          return <h1>Hello, {props.name}</h1>;
        }

        // Below is an example of calling the function.
        return (
          <div className="App">
            <Welcome name="Bart Mika" />
          </div>
        );
      }
    }

    export default App;
    ```

2. Please note that "props" stands for properties.

3. Here is another example which is a little more complex but shows what you can do.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        // This is a function-based component.
        function SuccessMessage(props) {
          return <p>Hello, {props.name} thank you for registering with Cooking Pot! Your activation email has been sent at {props.email} as of {props.time}.</p>;
        }

        // Below is an example of calling the function.
        return (
          <div className="App">
            <h1>Cooking Pot - Registration: Successful</h1>
            <SuccessMessage name="Bart Mika" email="bart@email.com" time="9:30pm" />
          </div>
        );
      }
    }

    export default App;
    ```

### (4) Class based

1. Let us see how a class based component would look like.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    // Here we create a ES6 class to define a component.
    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }

    class App extends Component {
      render() {
        // Below is an example of calling the class based component.
        return (
          <div className="App">
            <h1>Cooking Pot - Dashboard</h1>
            <Welcome name="Bart Mika" />
          </div>
        );
      }
    }

    export default App;
    ```

2. And that's it! You can write function or class based components. The choice is yours!

### (5) Re-cap
1. Lets explain the steps that happen in rendering a component.

  a. The main entry point into the application is through the `public/index.html`file. Inside that file you'll see this line:

      ```html
      <div id="root"></div>
      ```

  b. The ``react`` library populatest our code into there.

  c. The ``react`` library calls the ``src/index.js`` file and sees that we want to load our ``<App /> component via this line:

    ```html
    ReactDOM.render(<App />, document.getElementById('root'));
    ```

  d. If we look in the ``src/App.js`` file, this is where our code lives to generate the component! ``React`` looks for the ``render()`` function and returns the **element**.

  e. Inside the ``render()`` function you see a class-based compnent:

    ```js
    <Welcome name="Bart Mika" />
    ```

  f. The ``react`` library passes the property ``{name: "Bart Mika"}`` into the ``Welcome`` component.

  g. ``React`` looks for the ``render()`` function and returns the **element**. ``React`` sees the following:

    ```js
    return <h1>Hello, {this.props.name}</h1>;
    ```

  h. ``React`` then subs in the **props** into that compnent and that compnent returns an **element** which displays on the page.

  i. And the final product is a updated page. It says:

    ```
    Hello, Bart Mika
    ```

2. When you look in your browser, you are seeing the results of the render.

3. It is important to remember that **components** can be made up of many different **components**. When we combine two or more **components** we call this **composing**.

### (6) Composing Components

1. Components can easily be combined as this example shows.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    // Here we create a ES6 class to define a component.
    class UserView extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }

    class App extends Component {
      render() {
        // Below is an example of calling the class based component.
        return (
          <div className="App">
            <h1>Cooking Pot - Users List</h1>
            <UserView name="Bart Mika" />
            <UserView name="Frank Herbert" />
            <UserView name="Robert A. Heinlein" />
          </div>
        );
      }
    }

    export default App;
    ```

2. It is important to note that every ``React`` app has a single ``App`` component.

    ```js
    class App extends Component {
        // ...
    }
    ```

3. Here is a more advanced example showing how you can compose.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    // Component #1
    function Avatar(props) {
      return (
        <img className="Avatar"
          src={props.user.avatarUrl}
          alt={props.user.name}
        />
      );
    }

    // Component #2
    function EmailText(props) {
        return (
          <a className="EmailText" href="mailto:{props.user.email}">{props.user.email}</a>
        );
      }


    // Component #3
    function UserProfile(props) {
        return (
            <div className="UserProfile">
                <p><strong>Full-name:</strong> {props.user.name}</p>
                <p><strong>Email:</strong><EmailText user={props.user} /></p>
                <Avatar user={props.user} />
            </div>

        );
    }

    // Our application component - it loads all our components.
    class App extends Component {
      render() {
        // The user profile dictionary.
        var user = {
          name: 'Bart Mika',
          email: 'bart@email.com',
          avatarUrl: 'https://avatars3.githubusercontent.com/u/5767602?s=460&v=4'
        }

        // Below is an example of composing different componenets.
        return (
          <div className="App">
            <h1>Cooking Pot - User Profile</h1>
            <UserProfile user={user} />
          </div>
        );
      }
    }

    export default App;
    ```


### (7) Homework

1. Prepare for next tutorial.
    * "The Road to React" pages 16 to 27
    * https://reactjs.org/docs/lists-and-keys.html
