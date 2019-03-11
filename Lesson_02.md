## Lesson 02 - Introduction JSX
### (1) Create our app.

Using what you learned in the previous lesson, create our ``react`` app titled ``lesson-02``.

  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-02
  cd lesson-02
  npm start
  ```

### (2) Introduction
#### What is JSX?

##### Key ideas:

1. It is **syntax**.

2. It is how **events** are handled

3. It is how the **state changes** over time

3. And it is how the **data** is prepared for **display**.

4. **JSX mixes up HTML and JavaScript to define React components**

##### Examples:
###### Example 1:
It is the main component of your application.

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

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

export default App;
```

###### Example 2:
It can be any component you create.

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

###### Example 3:
``JSX`` can be a simple line of code like this.

```js
var element = <h1>Hello Bart!</h1>
```


### (3) App.js

1. Go to your ``src/App.js`` file. Open it and you will see.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

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

    export default App;
    ```

2. The first few lines (see below) are Javascript ES6 imports.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';
    ```

3. The next few lines are the declaration of the **component** and the render. This rendere will return the **element**.

    ```js
    class App extends Component {
        render() {
            return (
                // Some content is returned in here...
            )
        }
    }
    ```

4. Where is this **App** component being **instantiated**? Look in ``src/index.js`` and you will see the following code. Notice the ``<App />`` line? ``React`` renders the **element** to this view.

    ```js
    import React from 'react';
    import ReactDOM from 'react-dom';
    import './index.css';
    import App from './App';
    import * as serviceWorker from './serviceWorker';

    ReactDOM.render(<App />, document.getElementById('root'));

    // If you want your app to work offline and load faster, you can change
    // unregister() to register() below. Note this comes with some pitfalls.
    // Learn more about service workers: http://bit.ly/CRA-PWA
    serviceWorker.unregister();
    ```

5. You are wondering, wait where is the ``src/index.js`` being called from? If you look in the file ``public/index.html``, this is where the main entry into our application is happening.

### (4) Code with JSX

1. Go to your ``src/App.js`` file. Open it make the code look like this.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        return (
          <div className="App">
            <h1>Hellow world!</h1>
          </div>
        );
      }
    }

    export default App;
    ```

2. Save and check out the browser. You will see a hello world message.

3. Now enter this code and what do you see?

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        return (
          <div className="App">
            <h1>Cooking Pot: Dashboard</h1>
            <p>Hello and welcome YOUR_NAME, the last login time is YOUR_TIME.</p>
          </div>
        );
      }
    }

    export default App;
    ```

4. Pretty cool, you are beginning to see an app being created! How do we replace **YOUR_NAME** and **YOUR_TIME** text? We use **JSX** and turns those into variables. The updated code will look like this.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    class App extends Component {
      render() {
        var firstName = "Bart"
        var lastLoginTime = "2019-01-01 12:00pm"
        return (
          <div className="App">
            <h1>Cooking Pot: Dashboard</h1>
            <p>Hello and welcome {firstName}, the last login time is {lastLoginTime}.</p>

          </div>
        );
      }
    }

    export default App;
    ```

### (5) Try it yourself.

In your ``App.js`` file, update the code so it will say the following text:

    ```
    Hello my name is FIRST_NAME and I am from COUNTRY_NAME. I have been programming for NUMBER_OF_YEARS.
    ```

#### (6) Homework

1. Read:

    * https://reactjs.org/docs/introducing-jsx.html

2. Prepare for next tutorial.

    * "The Road to React" pages 13 to 15
