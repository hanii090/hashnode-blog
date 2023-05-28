---
title: "Test New Component State"
datePublished: Sun Sep 18 2022 06:10:30 GMT+0000 (Coordinated Universal Time)
cuid: cl8mnrdqk000j09l50d72hxho
slug: test-new-component-state
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1664433279004/2vnmVD4KJ.png
tags: reactjs, testing, jest, reacthooks

---

## Simulating Component State in Enzyme
With Enzyme, we can simulate and test different component contexts. We can use the setState() method to change the state of our component, and then we can test that it renders correctly under the new state.

In App.test.js, we define a new test:
```js
describe('<App /> shallow rendering', () => {
  it('should contain an element with logo as alt tag', () => {
    const wrapper = shallow(<App />)
    expect(wrapper.find({alt: 'logo'}).exists()).toBe(true)
  })
  it('matches the snapshot', () => {
    const tree = shallow(<App />)
    expect(toJson(tree)).toMatchSnapshot()
  })
  // Begin new test
  it('updates className with new State', () => {
    const wrapper = shallow(<App />)
    expect(wrapper.find('.blue').length).toBe(1)
    expect(wrapper.find('.red').length).toBe(0)
    wrapper.setState({mainColor: 'red'})
    expect(wrapper.find('.blue').length).toBe(0)
    expect(wrapper.find('.red').length).toBe(1)
  })
  // End new test
  it('on button click changes p text', () => {
    const wrapper = shallow(<App />)
    const button = wrapper.find('button')
    expect(wrapper.find('.button-state').text()).toBe('No!')
    button.simulate('click')
    expect(wrapper.find('.button-state').text()).toBe('Yes!')
  })
  it('on input change, title changes text', () => {
    const wrapper = shallow(<App />)
    const input = wrapper.find('input')
    expect(wrapper.find('h2').text()).toBe('')
    input.simulate('change', {currentTarget: {value: 'Tyler'}})
    expect(wrapper.find('h2').text()).toBe('Tyler')
  })
})
```
Now we navigate to our App.js component and define both our mainColor state and an h3 element with class name as the mainColor state:
```js
class App extends Component {
  // Add new main color state
  state = {
     on: false,
     input: '',
     mainColor: 'blue'
  }
  render() {
      return(
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1>Welcome to React</h1>
          // Begin new element
          <h3 className={this.state.mainColor}>Everyone is Welcome!</h3>
          // End new element
          <ul className="tyler">
            <li>Test 1</li>
            <li>Test 2</li>
            <li>Test 3</li>
          </ul>
          <li>Test 3</li>
          <p className='button-state'>{this.state.on ? 'Yes!' : 'No!'}</p>
          <button onClick = {() => this.setState({on: true})}>Click</button>
          <h2>{this.state.input}</h2>
          <input onChange={(e) => this.setState({input: e.currentTarget.value})} type='text'/>
          <p className="App-intro">
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <Title text="Some title" />
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
          <p>Hello World</p>
        </header>
        <Test />
      </div>
    );
  }
}
```
When we run our tests:
```
npm test
```
And update our snapshots by pressing 'u' in the terminal, we should see our tests all pass.

**"Now, as you can imagine, when we use this setState method on our wrapper, it will invoke setState on the root component and cause it re-render. This is useful for testing our components in different states."**



## Testing Appropriate Usage and Purpose of Lifecycle Methods
We can use Enzyme to test that our Lifecycle Methods are being called appropriately, and that our conditionally rendered components are being rendered appropriately.

We begin by writing a new test in App.test.js:
```js
describe('<App /> shallow rendering', () => {
  // ...
  it('calls componentDidMount, updates p tag text', () => {
    jest.spyOn(App.prototype, 'componentDidMount')
    const wrapper = shallow(<App />)
    expect(App.prototype.componentDidMount.mock.calls.length).toBe(1)
    expect(wrapper.find('.lifeCycle').text()).toBe('componentDidMount')
  })
})
```
**"Jest's spyOn method gives this ability to mock out the componentDidMount method inside of our App component."**

Our Jest assertion then tests for the lifecycle method to have been called once.

When we run our tests:
```
npm test
```
We see that our newest test for the componentDidMount lifecycle method failed. This is because the componentDidMount method does not exist on our component.

To make this test pass, we add a componentDidMount method to our App component:
```js
class App extends Component {
  state = {
     on: false,
     input: '',
     mainColor: 'blue',
  }
  componentDidMount() {
    this.setState({lifeCycle: 'componentDidMount'})
  }
  // ...
}
  ```
We can return to our terminal to see that all of our tests pass.

To actually utilize our lifecycle method, we can add a 'lifeCycle' property to our state, update it with componentDidMount(), and render it under a p tag:
```js
class App extends Component {
  state = {
     on: false,
     input: '',
     mainColor: 'blue',
     lifeCycle: ''
  }
  componentDidMount() {
    this.setState({lifeCycle: 'componentDidMount'})
  }
  render() {
      return(
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1>Welcome to React</h1>
          <h3 className={this.state.mainColor}>Everyone is Welcome!</h3>
          <ul className="tyler">
            <li>Test 1</li>
            <li>Test 2</li>
            <li>Test 3</li>
          </ul>
          <li>Test 3</li>
          // Begin new element:
          <p className='lifeCycle'>{this.state.lifeCycle}</p>
          // End new element
          <p className='button-state'>{this.state.on ? 'Yes!' : 'No!'}</p>
          // ...
    );
  }
}
```
With that in place, we can write a new assertion that expects our p tag to contain the text 'componentDidMount' after our componentDidMount lifecycle method is called:
```js
it('calls componentDidMount, updates p tag text', () => {
    jest.spyOn(App.prototype, 'componentDidMount')
    const wrapper = shallow(<App />)
    expect(App.prototype.componentDidMount.mock.calls.length).toBe(1)
    expect(wrapper.find('.lifeCycle').text()).toBe('componentDidMount')
  })
```
Returning to our terminal, we see that our only failing tests are the snapshot tests because we update our App component. Update the snapshots by pressing 'u' in the terminal, and we should see that all of our tests pass.

**"Now, in another test we've written, we've set props to test some come conditional content. We can take that once step further and test a corresponding lifecycle method."**

We'll write a new test that expects setProps to call the componentWillReceiveProps lifecycle method. In App.test.js:
```js
it('setProps calls componentWillReceiveProps', () => {
  jest.spyOn(App.prototype, 'componentWillReceiveProps')
  const wrapper = shallow(<App />)
  wrapper.setProps({ hide: true })
  expect(App.prototype.componentWillReceiveProps.mock.calls.length).toBe(1)
})
```
Again, we see that this test fails if we return to our terminal because this function does not yet exist on our component.

As we fixed the failing test with componentDidMount, we can add componentWillReceiveProps to our App component:
```js
class App extends Component {
  state = {
     on: false,
     input: '',
     mainColor: 'blue',
     lifeCycle: ''
  }
  componentDidMount() {
    this.setState({lifeCycle: 'componentDidMount'})
  }
  componentWillReceiveProps() {

  }
  render() {
      // ...
    );
  }
}
```
With our method in place, we can return to our terminal and see that our tests pass.

Taking this one step further, we can have our componentWillReceiveProps lifecycle method update the lifecycle property in our component's state to be 'componentWillReceiveProps'.
```js
componentWillReceiveProps() {
  this.setState({lifeCycle: 'componentWillReceiveProps'})
}
```
Then we can return to our App.test.js and create a new assertion that expects our p tag to contain the text 'componentWillReceiveProps' after the componentWillReceiveProps lifecycle method is called.
```js
it('setProps calls componentWillReceiveProps', () => {
  jest.spyOn(App.prototype, 'componentWillReceiveProps')
  const wrapper = shallow(<App />)
  wrapper.setProps({hide: true})
  expect(App.prototype.componentWillReceiveProps.mock.calls.length).toBe(1)
  expect(wrapper.find('.lifeCycle').text()).toBe('componentWillReceiveProps')
})
```



## Enzyme Method Testing
We can use Enzyme to test the functionality of our component methods.

In our App.test.js:
```js
describe('<App /> shallow rendering', () => {
  // ...
  it('handleStrings function returns correctly', () => {
    const wrapper = shallow(<App />)
    const trueReturn = wrapper.instance().handleStrings('Hello World')
    expect(trueReturn).toBe(true)
  })
})
```
When we run our tests in the terminal:
```
npm test
```
We see that our newest test is failing because handleStrings() does not yet exist.

**"We're able to access methods on this class because of enzyme's instance function. It returns the component that we've shadowed rendered, and give this access to its properties."**

In our App.js file, we add the handleStrings() method to our component:
```js
class App extends Component {
  state = {
    on: false,
    input: '',
    mainColor: 'blue',
    lifeCycle: ''
  }
  handleStrings(str) {
    return true
  }
  // ...
}
```
We should now see that our test passes on returning to the terminal.

Let's now set up our test to expect a false return given a different input. In App.test.js:
```js
it('handleStrings function returns correctly', () => {
  const wrapper = shallow(<App />)
  const trueReturn = wrapper.instance().handleStrings('Hello World')
  const falseReturn = wrapper.instance().handleStrings('')
  expect(trueReturn).toBe(true)
  expect(falseReturn).toBe(false)
})
```
On returning to our terminal, we should see that our test fails because our handleStrings function always returns true.

We'll now refactor our handleStrings function to conditionally return true or false:
```js
handleStrings(str) {
  if (str === 'Hello World') return true
  return false
}
```












## Resources
- [Lesson 11 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/10-test-new-component-state-with-set-state-in-enzyme)
- [setState() - Enzyme](https://github.com/enzymejs/enzyme/blob/master/docs/api/ShallowWrapper/setState.md)
- [Lesson 12 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/11-test-react-component-lifecycle-methods-with-enzyme)
- [spyOn() - Jest](https://jestjs.io/docs/en/jest-object#jestspyonobject-methodname)
- [Lifecycle Methods - React](https://reactjs.org/docs/state-and-lifecycle.html#adding-lifecycle-methods-to-a-class)
- [Lesson 13 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/12-test-react-component-methods-with-enzyme)
- [How to Directly Test React Component Methods with Enzyme](https://bambielli.com/til/2018-03-04-directly-test-react-component-methods/)
- [.instance() - Enzyme](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/instance.html)
