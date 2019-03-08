## Lesson 08 - Splitting Up Components, Reusable Components & Component Declarations
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-08
  cd lesson-08
  npm start
  ```

### (2) Splitting Up Components

1. Our current code can further be split up into more components. Let us do that now.

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


    // STEP 1: Create a search component to render our search field and attach to functionality.
    class Search extends Component {
        render() {
            const { value, onChange, children } = this.props;
            return (
                <form>
                    {children} <input
                      type="text"
                      value={value}
                      onChange={onChange}
                    />
                </form>
            );
        }
    }


    // STEP 2: Create a table component which will render the table row items along
    //         with attaching the functionality.
    class Table extends Component {
      render() {
        const { list, pattern, onDismiss } = this.props;
        return (
          <div>
            { list.filter(isSearched(pattern)).map(item =>
              <div key={item.objectID}>
                <span>
                  <a href={item.url}>{item.title}</a>
                </span>
                <span>{item.author}</span>
                <span>{item.num_comments}</span>
                <span>{item.points}</span>
                <span>
                  <button
                    onClick={() => onDismiss(item.objectID)}
                    type="button"
                  > Dismiss
                  </button>
                </span>
           </div> )}
        </div> );
      }
    }



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

        // STEP 3: Split our elements into components.
        render() {

          const { searchTerm, list } = this.state;

          // STEP 1: Notice the "value={searchTerm}" line of code.
          return (
            <div className="ListView">
            <Search
              value={searchTerm}
              onChange={this.onSearchChange}
            />
            <Table
              list={list}
              pattern={searchTerm}
              onDismiss={this.onDismiss}
            />
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

### (3) Re-usable Component.

1. Instead of re-defining the button, lets create a re-usable component we can call anytime.

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


    class Button extends Component { render() {
        const {
            onClick,
            className = '',
            children,
        } = this.props;
        return (
            <button
               onClick={onClick}
               className={className}
               type="button"
            >Dismiss
            </button> );
        }
    }

    class Search extends Component {
        render() {
            const { value, onChange, children } = this.props;
            return (
                <form>
                    <input
                      type="text"
                      value={value}
                      onChange={onChange}
                    />
                </form>
            );
        }
    }


    class Table extends Component {
      render() {
        const { list, pattern, onDismiss } = this.props;
        return (
          <div>
            { list.filter(isSearched(pattern)).map(item =>
              <div key={item.objectID}>
                <span>
                  <a href={item.url}>{item.title}</a>
                </span>
                <span>{item.author}</span>
                <span>{item.num_comments}</span>
                <span>{item.points}</span>
                <span>
                <Button onClick={() => onDismiss(item.objectID)} />
                </span>
           </div> )}
        </div> );
      }
    }



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
            <Search
              value={searchTerm}
              onChange={this.onSearchChange}
            />
            <Table
              list={list}
              pattern={searchTerm}
              onDismiss={this.onDismiss}
            />
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

### (4) Component Declarations

#### What type of components are there?
There are two types of components that we can declare:

**Functional Stateless Components:** These components are functions which get an input and return an output. The input is the props object. The output is a component instance. So far it is quite similar to an ES6 class component. However, functional stateless components are functions (functional) and they have no internal state (stateless). You cannot access the state with this.state because there is no this object. Additionally they have no lifecycle methods. You didn’t learn about lifecycle methods yet, but you already used two: constructor() and render(). Keep this fact about functional stateless components in mind, when you arrive at the lifecycle methods chapter later on.

**ES6 Class Components:** You already used this type of component declaration. In the class definition they extend from the React component. The extend hooks all the lifecycle methods - available in the React component API - to the component. As I mentioned, you already used two of them. Additionally you can store and manipulate state in ES6 class components.

#### When do I use either component?

A rule of thumb is to use **functional stateless components** when you don’t need internal component state or component lifecycle methods. Usually you start to implement your components as functional stateless components. Once you need access to the state or lifecycle methods, you have to refactor it to an ES6 class component.

#### Code

1. Know this, let us refactor some of our code.

2. Find the line with the code:

    ```js
    class Search extends Component {
        // ...
    }
    ```

3. Currently this component is a ``ES6`` class component but we can change to be a functional stateless component.

4. We can replace that entire component to look like this:

    ```js
    function Search(props) {
        const { value, onChange, children } = props;
        return (
            <form>
               {children} <input
               type="text"
               value={value}
               onChange={onChange}
               />
            </form>
        );
    }
    ```

5. Also we can use ES6 destructuring and simplify the code to look like this:

    ```js
    function Search({ value, onChange, children }) {
        return (
            <form>
                {children} <input
                type="text"
                value={value}
                onChange={onChange}
            />
        </form>
      );
    }
    ```

6. In addition, we know ES6 arrow function so we can simplify the code even further like this:

    ```js
    const Search = ({ value, onChange, children }) =>
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
    ```

7. (OPTIONAL) If you want to do some pre-processing in step 6, then you can write it as follows:

    ```js
    const Search = ({ value, onChange, children }) => {

      // do something ..

      return (
            <form>
                {children} <input
                type="text"
                value={value}
                onChange={onChange}
                />
            </form>
        );
    }
    ```

8. When you finish, your code should look like this:

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


    class Button extends Component { render() {
        const {
            onClick,
            className = ''
        } = this.props;
        return (
            <button
               onClick={onClick}
               className={className}
               type="button"
            >Dismiss
            </button> );
        }
    }

    // STEP 1: Update to use a functional stateless component.
    const Search = ({ value, onChange, children }) =>
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>


    class Table extends Component {
      render() {
        const { list, pattern, onDismiss } = this.props;
        return (
          <div>
            { list.filter(isSearched(pattern)).map(item =>
              <div key={item.objectID}>
                <span>
                  <a href={item.url}>{item.title}</a>
                </span>
                <span>{item.author}</span>
                <span>{item.num_comments}</span>
                <span>{item.points}</span>
                <span>
                <Button onClick={() => onDismiss(item.objectID)} />
                </span>
           </div> )}
        </div> );
      }
    }



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
            <Search
              value={searchTerm}
              onChange={this.onSearchChange}
            />
            <Table
              list={list}
              pattern={searchTerm}
              onDismiss={this.onDismiss}
            />
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

1. Prepare for next tutorial.
   * "The Road to React" pages 59 to 65
