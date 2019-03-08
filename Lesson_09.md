## Lesson 09 - Component Styling
### (1) Create our app.
  ```
  cd ~/javascript/bcinnovationlabs/
  create-react-app lesson-09
  cd lesson-09
  npm start
  ```

### (2) Instructions

1. Open up your ``src/index.css`` file and copy and paste the following styling.

    ```css
    body {
        color: #222;
        background: #f4f4f4;
        font: 400 14px CoreSans, Arial,sans-serif;
    }
    a{
        color: #222;
    }
    a:hover {
        text-decoration: underline;
    }
    ul, li { list-style: none; padding: 0; margin: 0;
    }
    input {
        padding: 10px; border-radius: 5px; outline: none; margin-right: 10px; border: 1px solid #dddddd;
    }
    button {
        padding: 10px; border-radius: 5px; border: 1px solid #dddddd; background: transparent; color: #808080;
        cursor: pointer;
    }
    button:hover { color: #222;
    }
    *:focus {
        outline: none;
    }
    ```

2. Now open up your ``src/App.css`` file and copy and paste the following styling.

    ```css
    .page {
        margin: 20px;
    }
    .interactions {
        text-align: center;
    }
    .table {
        margin: 20px 0;
    }
    .table-header {
        display: flex;
        line-height: 24px;
        font-size: 16px;
        padding: 0 10px; justify-content: space-between;
    }
    .table-empty {
        margin: 200px; text-align: center; font-size: 16px;
    }
    .table-row {
        display: flex; line-height: 24px;
        white-space: nowrap;
        margin: 10px 0;
        padding: 10px;
        background: #ffffff;
        border: 1px solid #e3e3e3;
    }
    .table-header > span { overflow: hidden; text-overflow: ellipsis; padding: 0 5px;
    }
    .table-row > span { overflow: hidden; text-overflow: ellipsis; padding: 0 5px;
    }
    .button-inline {
    border-width: 0;
    background: transparent;
    color: inherit;
    text-align: inherit; -webkit-font-smoothing: inherit; padding: 0;
      font-size: inherit;
      cursor: pointer;
    }
    .button-active {
    border-radius: 0;
    border-bottom: 1px solid #38BB6C;
    }
    ```

3. We will begin to use React ``className`` instead of ``class`` as HTML attribute.

4. Open up your ``src/App.js`` and change the code to support our new styling.

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
          <div className="table">
            { list.filter(isSearched(pattern)).map(item =>
             <div key={item.objectID} className="table-row">
             <span style={{ width: '40%' }}>
           <a href={item.url}>{item.title}</a>
         </span>
         <span style={{ width: '30%' }}>
           {item.author}
         </span>
         <span style={{ width: '10%' }}>
           {item.num_comments}
         </span>
         <span style={{ width: '10%' }}>
           {item.points}
         </span>
         <span style={{ width: '10%' }}>
           <Button
             onClick={() => onDismiss(item.objectID)}
             className="button-inline"
           >
             Dismiss
           </Button>
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

          return (
              <div className="page">
                  <div className="interactions">
                      <Search
                        value={searchTerm}
                        onChange={this.onSearchChange}
                      />
                   </div>

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



### (3) Homework

1. Prepare for next tutorial.
    * "The Road to React" pages x to y
