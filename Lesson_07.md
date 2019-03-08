## Lesson 07 - Forms, Events, & Higher-Order Functions, Destructuring, Controlled Component
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-07
  cd lesson-07
  npm start
  ```

### (2) Forms, Events & Higher Order Functions
Lets add a search field in our previous code.

1. Here is our previous code.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    const list = [
        {
          title: 'React',
          url: 'https://facebook.github.io/react/',
          author: 'Jordan Walke',
          num_comments: 3,
          points: 4,
          objectID: 0,
        }, {
            title: 'Redux',
            url: 'https://github.com/reactjs/redux',
            author: 'Dan Abramov, Andrew Clark',
            num_comments: 2,
            points: 5,
            objectID: 1,
        },
    ];

    class ListView extends React.Component {

        constructor(props) {
          super(props);

          this.state = {
              list: list,
          };

          this.onDismiss = this.onDismiss.bind(this);
        }

        componentDidMount() {
            // Do nothing.
        }

        componentWillUnmount() {
            // Do nothing.
        }

        onDismiss(id) {
            const isNotId = item => item.objectID !== id;
            const updatedList = this.state.list.filter(isNotId);
            this.setState({ list: updatedList });
        }

        render() {
          return (
            <div className="ListView">
            { this.state.list.map(item =>
                <div key={item.objectID}>
                    <span>
                      <a href={item.url}>{item.title}</a>
                    </span>
                    <span>{item.author}</span>
                    <span>{item.num_comments}</span>
                    <span>{item.points}</span>
                    <span>
                  <button
                    onClick={() => this.onDismiss(item.objectID)}
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }

    }


    class App extends Component {
      render() {
        return (
          <div className="App">
              <h1>Learn React</h1>
              <ListView />
          </div>
        );
      }
    }

    export default App;    
    ```

2. We want to have a search field and have our code to automatically start filtering as the user types in the search field. To do this we need to create a synthetic event, here is the code.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    const list = [
        {
          title: 'React',
          url: 'https://facebook.github.io/react/',
          author: 'Jordan Walke',
          num_comments: 3,
          points: 4,
          objectID: 0,
        }, {
            title: 'Redux',
            url: 'https://github.com/reactjs/redux',
            author: 'Dan Abramov, Andrew Clark',
            num_comments: 2,
            points: 5,
            objectID: 1,
        },
    ];

    class ListView extends React.Component {

        constructor(props) {
          super(props);

          this.state = {
              list: list,
          };

          this.onDismiss = this.onDismiss.bind(this);

          // STEP 2: Bind the "onSearchChange" function which we will create to our
          //         component.
          this.onSearchChange = this.onSearchChange.bind(this);
        }

        componentDidMount() {
            // Do nothing.
        }

        componentWillUnmount() {
            // Do nothing.
        }

        onDismiss(id) {
            const isNotId = item => item.objectID !== id;
            const updatedList = this.state.list.filter(isNotId);
            this.setState({ list: updatedList });
        }

        // STEP 3: Creat the function that will be called whenver a user starts
        //         typing in the search field. Because we are creating a synthetic
        //         event, we will have an `event` parameter in the function.
        onSearchChange(event) {
            // Do something...
        }

        render() {
          // STEP 1: Add our search field and attach our sythetic event.
          return (
            <div className="ListView">

            <form>
            <input
              type="text"
              onChange={this.onSearchChange}
            />
            </form>

            { this.state.list.map(item =>
                <div key={item.objectID}>
                    <span>
                      <a href={item.url}>{item.title}</a>
                    </span>
                    <span>{item.author}</span>
                    <span>{item.num_comments}</span>
                    <span>{item.points}</span>
                    <span>
                  <button
                    onClick={() => this.onDismiss(item.objectID)}
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }

    }


    class App extends Component {
      render() {
        return (
          <div className="App">
              <h1>Learn React</h1>
              <ListView />
          </div>
        );
      }
    }

    export default App;
    ```

3. How do we get the search value from the synthetic event? We get it as follows.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    const list = [
        {
          title: 'React',
          url: 'https://facebook.github.io/react/',
          author: 'Jordan Walke',
          num_comments: 3,
          points: 4,
          objectID: 0,
        }, {
            title: 'Redux',
            url: 'https://github.com/reactjs/redux',
            author: 'Dan Abramov, Andrew Clark',
            num_comments: 2,
            points: 5,
            objectID: 1,
        },
    ];

    class ListView extends React.Component {

        constructor(props) {
          super(props);

          this.state = {
              list: list,
              searchTerm: '', // STEP 1: We need to initial our search term with a default value.
          };

          this.onDismiss = this.onDismiss.bind(this);

          this.onSearchChange = this.onSearchChange.bind(this);
        }

        componentDidMount() {
            // Do nothing.
        }

        componentWillUnmount() {
            // Do nothing.
        }

        onDismiss(id) {
            const isNotId = item => item.objectID !== id;
            const updatedList = this.state.list.filter(isNotId);
            this.setState({ list: updatedList });
        }

        onSearchChange(event) {
            // STEP 2: Here we save the search term from the synthetic event.
            this.setState({ searchTerm: event.target.value });
        }

        render() {
          return (
            <div className="ListView">

            <form>
            <input
              type="text"
              onChange={this.onSearchChange}
            />
            </form>

            { this.state.list.map(item =>
                <div key={item.objectID}>
                    <span>
                      <a href={item.url}>{item.title}</a>
                    </span>
                    <span>{item.author}</span>
                    <span>{item.num_comments}</span>
                    <span>{item.points}</span>
                    <span>
                  <button
                    onClick={() => this.onDismiss(item.objectID)}
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }

    }


    class App extends Component {
      render() {
        return (
          <div className="App">
              <h1>Learn React</h1>
              <ListView />
          </div>
        );
      }
    }

    export default App;
    ```

4. How do we filter the values now that we have the search term? We need to use higher order functions.

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    const list = [
        {
          title: 'React',
          url: 'https://facebook.github.io/react/',
          author: 'Jordan Walke',
          num_comments: 3,
          points: 4,
          objectID: 0,
        }, {
            title: 'Redux',
            url: 'https://github.com/reactjs/redux',
            author: 'Dan Abramov, Andrew Clark',
            num_comments: 2,
            points: 5,
            objectID: 1,
        },
    ];

    // STEP 1: Create our higher order function which will return TRUE/FALSE
    //         depending on whether the search term matches the item title.
    const isSearched = (searchTerm) =>
        (item) =>
            !searchTerm || item.title.toLowerCase().includes(searchTerm.toLowerCase());



    class ListView extends React.Component {

        constructor(props) {
          super(props);

          this.state = {
              list: list,
              searchTerm: '',
          };

          this.onDismiss = this.onDismiss.bind(this);

          this.onSearchChange = this.onSearchChange.bind(this);
        }

        componentDidMount() {
            // Do nothing.
        }

        componentWillUnmount() {
            // Do nothing.
        }

        onDismiss(id) {
            const isNotId = item => item.objectID !== id;
            const updatedList = this.state.list.filter(isNotId);
            this.setState({ list: updatedList });
        }

        onSearchChange(event) {
            this.setState({ searchTerm: event.target.value });
        }

        // STEP 2: Apply below the filtering for our terms.
        render() {
          return (
            <div className="ListView">

            <form>
            <input
              type="text"
              onChange={this.onSearchChange}
            />
            </form>

            { this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
                <div key={item.objectID}>
                    <span>
                      <a href={item.url}>{item.title}</a>
                    </span>
                    <span>{item.author}</span>
                    <span>{item.num_comments}</span>
                    <span>{item.points}</span>
                    <span>
                  <button
                    onClick={() => this.onDismiss(item.objectID)}
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }

    }


    class App extends Component {
      render() {
        return (
          <div className="App">
              <h1>Learn React</h1>
              <ListView />
          </div>
        );
      }
    }

    export default App;
    ```

### (3) Destructuring

1. Since we know ``ES6`` supports ``destructuring``, for example:

    ```js
    const user = { firstname: 'Bart', lastname: 'Mika',;

    // ES5
    var firstname = user.firstname;
    var lastname = user.lastname;

    // ES6
    const { firstname, lastname } = user;
    console.log(firstname + ' ' + lastname); // output: Robin Mika
    ```

2. Lets us update our code to be simpler using our new ``destructuring`` concept:

    ```js
    import React, { Component } from 'react';
    import logo from './logo.svg';
    import './App.css';

    const list = [
        {
          title: 'React',
          url: 'https://facebook.github.io/react/',
          author: 'Jordan Walke',
          num_comments: 3,
          points: 4,
          objectID: 0,
        }, {
            title: 'Redux',
            url: 'https://github.com/reactjs/redux',
            author: 'Dan Abramov, Andrew Clark',
            num_comments: 2,
            points: 5,
            objectID: 1,
        },
    ];


    const isSearched = (searchTerm) =>
        (item) =>
            !searchTerm || item.title.toLowerCase().includes(searchTerm.toLowerCase());



    class ListView extends React.Component {

        constructor(props) {
          super(props);

          this.state = {
              list: list,
              searchTerm: '',
          };

          this.onDismiss = this.onDismiss.bind(this);

          this.onSearchChange = this.onSearchChange.bind(this);
        }

        componentDidMount() {
            // Do nothing.
        }

        componentWillUnmount() {
            // Do nothing.
        }

        onDismiss(id) {
            const isNotId = item => item.objectID !== id;
            const updatedList = this.state.list.filter(isNotId);
            this.setState({ list: updatedList });
        }

        onSearchChange(event) {
            this.setState({ searchTerm: event.target.value });
        }

        render() {

          // STEP 1: Destructure our component state.
          const { searchTerm, list } = this.state;

          // STEP 2: Update the code below to use our local variables.
          return (
            <div className="ListView">

            <form>
            <input
              type="text"
              onChange={this.onSearchChange}
            />
            </form>

            { list.filter(isSearched(searchTerm)).map(item =>
                <div key={item.objectID}>
                    <span>
                      <a href={item.url}>{item.title}</a>
                    </span>
                    <span>{item.author}</span>
                    <span>{item.num_comments}</span>
                    <span>{item.points}</span>
                    <span>
                  <button
                    onClick={() => this.onDismiss(item.objectID)}
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }

    }


    class App extends Component {
      render() {
        return (
          <div className="App">
              <h1>Learn React</h1>
              <ListView />
          </div>
        );
      }
    }

    export default App;
    ```

### (4) Controlled Component

* HTML ``<input>``, ``<textarea>`` and ``<select>`` elements hold their own state.

* HTML elements modify the value internally once someone changes it from the outside.

* In React that’s called an **uncontrolled component**, because HTML elements handles their own state.

* In React you should make sure to make those elements **controlled components**.

To set our search field to be a controlled component, do the following:

```js
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

const list = [
    {
      title: 'React',
      url: 'https://facebook.github.io/react/',
      author: 'Jordan Walke',
      num_comments: 3,
      points: 4,
      objectID: 0,
    }, {
        title: 'Redux',
        url: 'https://github.com/reactjs/redux',
        author: 'Dan Abramov, Andrew Clark',
        num_comments: 2,
        points: 5,
        objectID: 1,
    },
];


const isSearched = (searchTerm) =>
    (item) =>
        !searchTerm || item.title.toLowerCase().includes(searchTerm.toLowerCase());



class ListView extends React.Component {

    constructor(props) {
      super(props);

      this.state = {
          list: list,
          searchTerm: '',
      };

      this.onDismiss = this.onDismiss.bind(this);

      this.onSearchChange = this.onSearchChange.bind(this);
    }

    componentDidMount() {
        // Do nothing.
    }

    componentWillUnmount() {
        // Do nothing.
    }

    onDismiss(id) {
        const isNotId = item => item.objectID !== id;
        const updatedList = this.state.list.filter(isNotId);
        this.setState({ list: updatedList });
    }

    onSearchChange(event) {
        this.setState({ searchTerm: event.target.value });
    }

    render() {

      const { searchTerm, list } = this.state;

      // STEP 1: Notice the "value={searchTerm}" line of code.
      return (
        <div className="ListView">

        <form>
        <input
          type="text"
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        </form>

        { list.filter(isSearched(searchTerm)).map(item =>
            <div key={item.objectID}>
                <span>
                  <a href={item.url}>{item.title}</a>
                </span>
                <span>{item.author}</span>
                <span>{item.num_comments}</span>
                <span>{item.points}</span>
                <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"> Dismiss
              </button>
            </span>
             </div>
        )}
        </div>
      );
    }

}


class App extends Component {
  render() {
    return (
      <div className="App">
          <h1>Learn React</h1>
          <ListView />
      </div>
    );
  }
}

export default App;
```


### (5) Homework

1. Assignment. Add close buttons per row (as you learned in this lesson) to the last lessons assignment.

2. Prepare for next tutorial.
   * "The Road to React" pages 46 to 58
