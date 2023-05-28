---
title: "Test React Component Props, DOM Rendering Event Handlers,"
datePublished: Sat Sep 24 2022 06:16:13 GMT+0000 (Coordinated Universal Time)
cuid: cl8mnmhb2000g09l53dhpdx48
slug: test-react-component-props-dom-rendering-event-handlers
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1664433245619/wAFV0egUI.png
tags: react, reactjs, testing, enzyme, jest

---

 

To begin this lesson, let's create a new component.

In App.js:
```js
// ...
export class Link extends Component {
  render() {
    return <a href={this.props.address}>Click</a>
  }
}
//...
```
**Make sure to import Component from React**:
```js
import React, { Component } from 'react';
```
Now we have a simple Link component that returns an 'a' element with an href tag that receives an address via props.

In App.test.js, we create a new describe block for our new Link component:
```js
// ...

describe('<Link />', () => {
  it('link component accepts address prop', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.instance().props.address).toBe('www.google.com')
  })
})
```

**"When it comes to testing component props with Enzyme, it's important to understand which prop of the component we're trying to test, and what I mean by this is, are we trying to test the actual instance of the component? (<Link address='www.google.com' />), or are we trying to test the href value on the returned a tag node? (<a href={this.props.address}>Click</a>)"**

For the test defined above, "we're testing the actual instance of the address prop"

Running our test in the terminal:
```
npm test
```
We should see that our test passes.

We define a new test in App.test.js:
```js
describe('<Link />', () => {
  it('link component accepts address prop', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.instance().props.address).toBe('www.google.com')
  })
  // New test:
  it('a tag node renders href correctly', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.props().href).toBe('www.google.com')
  })
})
```
In this test, we're making sure that our href is using the correct prop value, as opposed to the first test where we use the .instance() method.

"Now we're just using the props method on the wrapper itself. This will return all the props of the component's returned node. In our case, we're looking at the a tag, and it's treating the href like a prop. **Our two tests are testing the same prop essentially, but in different ways."**

What if our component's return method was conditional and depended on the prop that's been passed?

In App.js:
```js
// ...

export class Link extends Component {
  render() {
    return this.props.hide ? null : <a href={this.props.address}>Click</a>
  }
}
```
Now we define a new test for our component:
```js
// ...

describe('<Link />', () => {
  it('link component accepts address prop', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.instance().props.address).toBe('www.google.com')
  })
  it('a tag node renders href correctly', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.props().href).toBe('www.google.com')
  })
  it('returns null with a true hide props', () => {
    const wrapper = shallow(<Link hide={false} />)
    expect(wrapper.find('a').length).toBe(1)
  })
})
```
In the terminal, we see that the test passes. This means that our hide={false} prop is returning our a tag.

Now let's test that our null is being returned correctly:
```js
describe('<Link />', () => {
  it('link component accepts address prop', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.instance().props.address).toBe('www.google.com')
  })
  it('a tag node renders href correctly', () => {
    const wrapper = shallow(<Link address='www.google.com' />)
    expect(wrapper.props().href).toBe('www.google.com')
  })
  it('returns null with a true hide props', () => {
    const wrapper = shallow(<Link hide={false} />)
    expect(wrapper.find('a').length).toBe(1)
    wrapper.setProps({ hide: true })
    expect(wrapper.get(0)).toBeNull()
  })
})
```
In the terminal, we should see our tests pass.

setProps takes an object and passes it through as new props to a component.

**The setProps() method is useful for testing how components behave with changing props.**



## Full DOM Rendering
"Full DOM rendering is ideal for use cases where you have components that interact with DOM APIs, or require React lifecycles."

In App.test.js:
```js
// ...
import { configure, shallow, mount } from 'enzyme'
// ...
```
Now, duplicate our first describe block, and specify the first as using shallow rendering and the second as using mount rendering. Then, change the "shallows" in the second describe block to "mounts":
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
})

describe('<App /> mount rendering', () => {
  it('should contain an element with logo as alt tag', () => {
    const wrapper = mount(<App />)
    expect(wrapper.find({alt: 'logo'}).exists()).toBe(true)
  })
  it('matches the snapshot', () => {
    const tree = mount(<App />)
    expect(toJson(tree)).toMatchSnapshot()
  })
})
```
"Full DOM rendering requires that a full DOM API be available at the global scope. This means that we must run our test in an environment that at least looks like a browser environment."

To satisfy this, we need to import jsdom. If you're not using Create React App (the notes so far HAVE used Create React App), run the following in the terminal:
```
npm install jsdom
```
We can input a second argument in our wrapper mount render:
```js
const wrapper = mount(<App />, {context: {}, attachTo: DOMElement})
```
The context object allows us to pass context into our component, and the attachTo object allows us to attach our component to a specific DOM element. Check out the [resources](#resources) for more information on mount rendering and its arguments.

If you added the argument above, remove it now:
```js
const wrapper = mount(<App />)
```
Moving on:
**"Unlike shallow or static rendering, full rendering actually mounts the component in a DOM, which means that tests can affect each other if they're using the same DOM."**

Adding the unmount() method at the end of each mount render test unmounts the component from the DOM. It can also be used to simulate a component going through an unmount mount lifecycle in React:
```js
describe('<App /> mount rendering', () => {
  it('should contain an element with logo as alt tag', () => {
    const wrapper = mount(<App />)
    expect(wrapper.find({alt: 'logo'}).exists()).toBe(true)
    wrapper.unmount()
  })
  it('matches the snapshot', () => {
    const tree = mount(<App />)
    expect(toJson(tree)).toMatchSnapshot()
    tree.unmount()
  })
})
```
Running our test in the terminal:
```
npm test
```
And updating the snapshot by pressing 'u' in the terminal allows all of our tests to pass.

The mount toJson rendering is slightly different than the shallow rendering, so changing the mount rendering to shallow rendering will cause our snapshot test to fail.
```js
describe('<App /> mount rendering', () => {
  it('should contain an element with logo as alt tag', () => {
    const wrapper = shallow(<App />)
    expect(wrapper.find({alt: 'logo'}).exists()).toBe(true)
    // wrapper.unmount()
  })
  it('matches the snapshot', () => {
    const tree = shallow(<App />)
    expect(toJson(tree)).toMatchSnapshot()
    // tree.unmount()
  })
})
```



## Testing Event Handlers
With Enzyme we can both test components that use event handlers by simulating those events, and test that conditionally rendered attributes work as intended.

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
  // New Test:
  it('on button click changes p text', () => {
    const wrapper = shallow(<App />)
    const button = wrapper.find('button')
    expect(wrapper.find('.button-state').text()).toBe('No!')
  })
})
```
If we check our terminal now:
```
npm test
```
We should see that our newly defined test fails because the elements we're searching for do not yet exist.

In App.js, we introduce the new p tag with class 'button-state' and the button with the onClick. **Important: These notes have used App as a functional component until now. We must change App to a class component, as we are now using setState(). More information on class vs functional components in the [resources](#resources)**
```js
class App extends Component {
  state = { on: false }
  render() {
      return(
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1>Welcome to React</h1>
          <ul className="tyler">
            <li>Test 1</li>
            <li>Test 2</li>
            <li>Test 3</li>
          </ul>
          <li>Test 3</li>
          // Begin new elements
          <p className='button-state'>{this.state.on ? 'Yes!' : 'No!'}</p>
          <button onClick = {() => this.setState({on: true})}>Click</button>
          // End new elements
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
Returning to our terminal, we should see that the only tests that fail are our snapshots tests. Pressing 'u' to update our snapshots should allow all of our tests to pass.

Now, back in App.test.js, we'll simulate a click event and expect our .button-state to change from 'No!' to 'Yes!':
```js
it('on button click changes p text', () => {
  const wrapper = shallow(<App />)
  const button = wrapper.find('button')
  expect(wrapper.find('.button-state').text()).toBe('No!')
  button.simulate('click')
  expect(wrapper.find('.button-state').text()).toBe('Yes!')
})
```
Now let's see how this could be used in the case of an input element that, whenever a user inputs text, updates state.

We define a new test in App.test.js:
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
    input.simulate('change')
    expect(wrapper.find('h2').text()).toBe('Tyler')
  })
})
```
Now, inside our App.js, we add our h2 and input elements, and add our input state:
```js
// Add input state
class App extends Component {
  state = {
     on: false,
     input: ''
  }
  render() {
      return(
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1>Welcome to React</h1>
          <ul className="tyler">
            <li>Test 1</li>
            <li>Test 2</li>
            <li>Test 3</li>
          </ul>
          <li>Test 3</li>
          <p className='button-state'>{this.state.on ? 'Yes!' : 'No!'}</p>
          <button onClick = {() => this.setState({on: true})}>Click</button>
          // Begin new elements
          <h2>{this.state.input}</h2>
          <input onChange={(e) => this.setState({input: e.currentTarget.value})} type='text'/>
          // End new elements
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
After running our tests and updating our snapshots, we should see that our on input change test is failing.

**"This is because our simulated event is looking for this currentTarget property from the event. Our test does not have that being passed through to the method."**

We can fix this by passing an optional object argument to the simulate() method. In App.test.js:
```js
it('on input change, title changes text', () => {
  const wrapper = shallow(<App />)
  const input = wrapper.find('input')
  expect(wrapper.find('h2').text()).toBe('')
  input.simulate('change', {currentTarget: {value: 'Tyler'}})
  expect(wrapper.find('h2').text()).toBe('Tyler')
})
```























## Resources
- [Lesson 8 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/07-test-react-component-props-with-enzyme-and-jest)
- [Enzyme .get() method](https://enzymejs.github.io/enzyme/docs/api/ReactWrapper/get.html)
- [Enzyme .setProps() method](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/setProps.html)
- [Lesson 9 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/08-fully-render-react-components-with-enzyme)
- [Mount Options - Enzyme](https://github.com/enzymejs/enzyme/blob/master/docs/api/mount.md#mountnode-options--reactwrapper)


- [Lesson 10 Code](https://github.com/ParkerGits/react-enzyme-jest/tree/09-test-simulated-event-handlers-with-enzyme)
- [Class vs Functional Components in React](https://medium.com/@Zwenza/functional-vs-class-components-in-react-231e3fbd7108)
- [simulate() - Enzyme](https://enzymejs.github.io/enzyme/docs/api/ShallowWrapper/simulate.html)

