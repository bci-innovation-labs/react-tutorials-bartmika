## Lesson 11 - Container component pattern
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-11
  cd lesson-11
  npm start
  ```

### (2) Theory

* Read https://medium.com/@learnreact/container-components-c0e67432e005

* A container does data fetching and then renders its corresponding sub-component. That’s it.

* Currently your component is responsible for both fetching data and presenting it. There’s nothing “wrong” with this but you miss out on a few benefits of React.

* Our component is opinionated about data structure but has no way of expressing those opinions. This component will break silently if the json endpoint change.

### (3) Practice

1. Lets pretend we have the following source code. Populate the ``src/App.js`` code with the following:

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

2. How do we separate the ``ListView`` to be ``ListView`` and ``ListViewContainer`` ? The key idea is we want *containers* to only **fetch data** and offset rendering to a component.

    ```js
    import React, { Component } from 'react';
    // import logo from './logo.svg';
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

    // STEP 1: Component used only for RENDERING and nothing else.
    class ListView extends Component {
        render() {
          const {list,onDismiss} = this.props;
          return (
            <div className="ListView">
            { list.map(item =>
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
                    type="button"> Dismiss
                  </button>
                </span>
                 </div>
            )}
            </div>
          );
        }
    }

    // STEP 2: Component used for FETCHING DATA and using OTHER COMPONENTS FOR RENDERING.
    class ListViewContainer extends React.Component {

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
          // STEP 3: Therefore our containers ``render()`` function should be as
          //         simple looking as this - all rendering is offset to another
          //         component.
          const {list} = this.state
          return (
            <div className="ListView">
                <ListView
                  list={list}
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
              <ListViewContainer />
          </div>
        );
      }
    }

    export default App;
    ```
