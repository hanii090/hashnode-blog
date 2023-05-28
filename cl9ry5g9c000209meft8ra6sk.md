---
title: "How to Simplify React Apps with React Hooks"
datePublished: Fri Oct 28 2022 03:40:35 GMT+0000 (Coordinated Universal Time)
cuid: cl9ry5g9c000209meft8ra6sk
slug: how-to-simplify-react-apps-with-react-hooks
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1666927839216/ryHVdsqRi.png
tags: javascript, reactjs, reacthooks

---

## What you'll learn 💪

- How to convert React class components into function components
- Usage of different React hooks like `useState`, `useRef`, `useReducer`, `useEffect`
- How to create your own custom hook
- Usage of `React.lazy` and `Suspense` to load your React components lazily
- How to __preload__ your React component when needed

## What you won't learn ❌

- The post is not intended to sell you on the "WHY" of React hooks. It talks more about the "HOW". To know why, you can read [reactjs](https://reactjs.org/hooks). I highly recommend you watch [youtube](https://www.youtube.com/watch?v=dpw9EHDh2bM) video from ReactConf 2018
- It doesn't teach you the basics of React hooks from the basics.  

- This post is not going to teach you `React.lazy` and `Suspense` from scratch. 

## Prerequisites ✅

- Understanding of Javascript ES6 syntax, special features like: destructing, arrow functions, etc
- Prior knowledge of React.  
- Basic familiarity with React hooks.  

# Let's Start 

 ##  Refactor a class component to a function component using React Hooks 

 we have a render prop based **class** component that allows us to make a GraphQL request with a given query string and variables and uses a GitHub graphql client that is in React context to make the request. 
We will refactor this to a **function** component that uses the hooks `useReducer`, `useContext`, and `useEffect`.

```javascript
// Class component
class Query extends Component {
    static proptypes = {
        query: PropTypes.string.isRequired,
        variables: PropTypes.object,
        children: PropTypes.func.isRequired,
        normalize: PropTypes.func
    }
    static defaultProps = {
        normalize: data => data
    }
    static contextType = GitHub.Context
}
```

To start with the refactor, let us first create a function component named `Query` and set the propTypes and defaultProps. To set the defaultProps, destructuring serves good.Now, to get client from GitHub.Context, we will use the hook `useContext`

```javascript
// Refactored function component
function Query({query, variables, normalize = data => data, children}) {
    const client = useContext(GitHub.Context)
}
Query.propTypes = {
    query: PropTypes.string.isRequired,
    variables: PropTypes.object,
    children: PropTypes.func.isRequired,
    normalize: PropTypes.func
}
```

---
Now we will refactor our state declaration. One way to do it is using the `useState` hook, but to save us from writing too much and have minimal change, we will use the `useReducer` hook.

```javascript
class Query extends Component {
    ...
    ...
    state: {loaded: false, fetching: false, data: null, error: null}
}
```

to

```javascript
function Query({query, variables, normalize = data => data, children}) {
    ...
    ...
    const [state, setState] = useReducer(
        // creating new state in a function
        (state, newState) => ({...state, ...newState}),
        // the initial state object
        {loaded: false, fetching: false, data: null, error: null}
    )
}
```

This is how actually `this.setState` works in React.

---
Now, we will use the `useEffect` hook to simulate what is happening in the componentDidMount and componentDidUpdate. In the original class component, the actual query call is made in componentDidMount to fetch data just after the render. We want to make the query call only when the query and variables passed to it change. This is what we are making sure in componentDidUpdate

```javascript
class Query extends Component {
    ...
    ...
    componentDidMount() {
        // this will be called when the the Query component is mounted
        this.query();
    }

    componentDidUpdate(prevProps) {
        // isEqual method from lodash does deep comparison of objects
        if(
            !isEqual(this.props.query, prevProps.query) ||
            !isEqual(this.props.variables, prevProps.variables)
        ) {
            this.query()
        }
    }

    // Network call to fetch data
    query() {
        this.setState({fetching: true});
        const client = this.context;
        client
        .request(this.props.query, this.props.variables)
        .then(res =>
            this.setState({
                data: this.props.normalize.res,
                error: null
                loaded: true
                fetching: false
            })
        )
        .catch(error =>
            this.setState({
                error,
                data: null,
                loaded: false,
                fetching: false
            })
        )
    }
}
```

to

```javascript
useEffect(() => {
    setState({fetching: true});
    client
    .request(query, variables)
    .then(res =>
        setState({
            data: normalize.res,
            error: null
            loaded: true
            fetching: false
        })
    )
    .catch(error =>
        setState({
            error,
            data: null,
            loaded: false,
            fetching: false
        })
    )
}, [query, variables])
```

The second argument to useEffect hooks is an array of dependencies, on change of which the code inside useEffect runs. Just behold the beauty of React hooks. It makes our components so much simpler.
>Note: If you notice, the dependency array of useEffect contains variables which by default will be compared on a shallow basis. We have handled its deep comparison here in the next doc

---
Now, we have the render method in the class component which can just be converted as the return statement in the function component

```javascript
render() {
    return this.props.children(this.state)
}
```

to

```javascript
// we are getting the state here from the useReducer hook, remember?
return children(state)
```







## Handle Deep Object Comparison in React's useEffect hook with the useRef Hook 

With our old Query component, we were actually using this `isEqual` from **lodash** when we had this componentDidUpdate to compare the previous `this.props.query` and the previous `this.props.variables` with the new `prevProps.query` and the new `prevProps.variables`. We are doing that because the variables can actually be an **object**.

__Old Query Component__

```javascript
componentDidUpdate(prevProps) {
    if (
      !isEqual(this.props.query, prevProps.query) ||
      !isEqual(this.props.variables, prevProps.variables)
    ) {
      this.query()
    }
  }
```

We have passed variables as a dependency in the useEffect, but what it will do is it will check the `prevVariables === variables` and if it does not find it true, it is going to rerun our callback passed inside useEffect. 
Now, this is always going to return false because the variables prop is being passed in form of an object like this

```javascript
<Query
    query={userQuery}
    variables={{username}}
    normalize={normalizeUserData}
>
```

So, every single time, it is a brand new object `{username}` and so, every single time, our useEffect callback will run.

So, to have a deep equality check on our object `variables`, we can remove the dependency array (second argument) from the useEffect call and add a condition like this and return if it is true.

```javascript
if(isEqual(previousInputs, [query, variables])) {
    return;
}
```

Now, the question arises, how will we get the prevInputs, we need to keep a reference of some sort. Hmmm 🤔
Well, we can use the `useRef` hook at our disposal.

```javascript
const previousInputs = useRef()
useEffect(() => {
    // each time, our component renders
    previousInputs.current = [query, variables]
})
```

For the first time, the previousInputs.current will be null, so the component will be rendered regardless of this. It is only after the first time, that the reference is set for comparison.
So, we will use previousInput.current in the if condition now

```javascript
if(isEqual(previousInputs.current, [query, variables])) {
    return;
}
```

In this way, we made sure that we don't run our setState call and our client call, unless our previousInputs are different from the new inputs.

So, finally the useEffect calls will look something like this:

```javascript
useEffect(() => {
    if(isEqual(previousInputs.current, [query, variables])) {
        return;
    }
    setState({fetching: true});
    client
    .request(query, variables)
    .then(res =>
        setState({
            data: normalize.res,
            error: null
            loaded: true
            fetching: false
        })
    )
    .catch(error =>
        setState({
            error,
            data: null,
            loaded: false,
            fetching: false
        })
    )
})

const previousInputs = useRef();
useEffect(() => {
    previousInputs.current = [query, variables]
})
```




##  Safely setState on a Mounted React Component through the useEffect Hook 

> Q. What does safe setState mean?


**Ans **- Setting the state safely means checking whether our component is mounted before trying to call setState. This is done normally when the client is unable to cancel in-flight requests on its own.

__Old Query Component__

```javascript
class Query extends Component {
    ...
    ...
    componentDidMount() {
        this._isMounted = true
        this.query()
    }

    componentWillUnmount() {
        this._isMounted = false
    }


    safeSetState(...args) {
        this._isMounted && this.setState(...args)
    }
}
```

There are chances that our component is unmounted while our query is still is in out in flight. So, save us from the sideEffect, in our old component, we were using `this.safeSetState` whenever we wanted to update the state.

We have a `_isMounted` variable is set to true as soon as the component is mounted. Whenever we update state, we first check the `_isMounted` value and if it is true, we do a setState. During the time of unmounting, we equate the `_isMounted` value to false.

>Note: This is not a proper solution to the problem. We are doing this just because the client we are using, doesn't support auto-canceling requests.

**How do we implement it in our newly refactored function component?**

```javascript
const mountedRef = useRef(false)

useEffect(() => {
    mountedRef.current = true;
    return () => (mountedRef.current = false)
}, [])
```

This bit of code here takes care of the the operation componentDidMount and the componentWillUnmount lifecycle method. We had to track the mounted state of our component with `mountedRef`
When the component is mounted, mountedRef.current is marked true and as the cleanup function after the unmounting happens, the value is marked false again. The dependency array is empty because we want to let this piece of code run only once when the component is mounted.

For, `safeSetState` now,

```javascript
const safeSetState = (...args) => mountedRef && setState(...args)
```

Now, we will change setState to safeSetState wherever we feel that the component could potentially be unmounted. After this, the final code will look like this:

```javascript
useEffect(() => {
    if(isEqual(previousInputs.current, [query, variables])) {
        return;
    }
    // no need to change
    setState({fetching: true});
    client
    .request(query, variables)
    .then(res =>
        safeSetState({
            data: normalize.res,
            error: null
            loaded: true
            fetching: false
        })
    )
    .catch(error =>
        safeSetState({
            error,
            data: null,
            loaded: false,
            fetching: false
        })
    )
})
```


## Extract Generic React Hook Code into Custom React Hooks 
The hooks code is regular JavaScript, extracting it to its own function is trivial and it enables code sharing in a really nice way. It also allows encapsulation and separation of concerns really cleanly.

Let us start by looking at our state setup code

```javascript
const [state, setState] = useReducer(
    (state, newState) => ({...state, ...newState}),
    {loaded: false, fetching: false, data: null, error: null}
)
```

This looks a bit complex by the look of it. Also, if we want to use it again in some other component, we again have to write this same piece of code and it is not generic enough, if we want to use a different initialState object.

Let us try to write a function (a custom hook) `useSetState` that will make it generic and reusable

```javascript
function useSetState(initialState) {
  const [state, setState] = useReducer(
    (state, newState) => ({...state, ...newState}),
    initialState,
  )
  return [state, setState];
}
```

Note that we have used initialState as the argument of the hook, so that whatever state object is passed to, this hook return a getter and setter for it.
>If you would like a more comprehensive useSetState hook, give the npm module `use-legacy-state` a try.

---
Coming onto the next piece of code that we can make generic

```javascript
const mountedRef = useRef(false)

useEffect(() => {
    mountedRef.current = true;
    return () => (mountedRef.current = false)
}, [])
const safeSetState = (...args) => mountedRef && setState(...args)
```

This seems like a usefully generic function. Let us try to make a custom hook `useSafeSetState` for this

```javascript
function useSafeSetState(initialState) {
    const [state, setState] = useSetState(initialState);
    const mountedRef = useRef(false)

    useEffect(() => {
        mountedRef.current = true;
        return () => (mountedRef.current = false)
    }, [])
    const safeSetState = (...args) => mountedRef && setState(...args)
    return [state, safeSetState]
}
```

Accordingly, we will update at respective places and use this custom hook for setting states safely in a more generic way.

---

## Track Values Over the Course of Renders with React useRef in a Custom usePrevious Hook 

We have one more segment of code in our `Query` component, where we are getting and tracking previous value of our inputs

```javascript
const previousInputs = useRef()
useEffect(() => {
    // each time, our component renders
    previousInputs.current = [query, variables]
})
```

This again is a generic operation and can be separated out in its own custom hook implementation

```javascript
function usePrevious(previousValue) {
    const ref = useRef()
    useEffect(() => {
        ref.current = previousValue
    })
    return ref.current
}
```

In this way, we can use the `usePrevious` hook in our old code

```javascript
const previousInputs = usePrevious([query, variables]);
```

As this is returning the `.current` value of the reference, we can use the early return in the useEffect as

```javascript
// not previousInputs.current
if(isEqual(previousInputs , [query, variables])) {
    return
}
```



## Refactor a React Class Component with useContext and useState Hooks

We've got a pretty simple User class component that manages a bit of state and uses some context. Let's refactor this over to a function component that uses the `useContext` and `useState` hooks.
**Old component class**

```javascript
class User extends Component {
  static propTypes = {
    username: PropTypes.string,
  }
  static contextType = GitHubContext
  state = {filter: ''}

...
}
```

to

```javascript
function User({username}) {
    const client = useContext(GithubContext);
    const [filter, setFilter] = useState('');
}
User.propTypes = {
    username: PropTypes.string
}
```

The only thing remainining to port to this function component is the return statement. We will return whatever the `render()` method in Class component returns.
That's it, or is it?
We also have to remove all the occurences of `this.` in the code. Also, as we have destructured our props, we don't need `props.` at every place, so need to get rid of that.

Voila! You have your arguably simple function component working as expected 🙂



##  Refactor a render Prop Component to a Custom React Hook 

**user.js**

```javascript
function User({username}) {
  const {logout} = useContext(GitHubContext)
  const [filter, setFilter] = useState('')
  return (
    <Query
      query={userQuery}
      variables={{username}}
      normalize={normalizeUserData}
    >
      {({fetching, data, error}) =>
        ...
      }
    </Query>
  )
}
```

`renderProps` are an excellent way of sharing code. Our `<Query/>` component is a render prop based component that the `<User/>` component uses. But because it doesn't render anything, we can actually just change it to a custom hook. This will make our code clean and less complex.
Here, we create a `useQuery` hook that returns the state from the hooks the Query component uses and use that instead.

Something like this:

```javascript
function User({username}) {
  const {logout} = useContext(GitHubContext)
  const [filter, setFilter] = useState('')

  const {fetching, data, error} = useQuery({
    query: userQuery,
    variables: {username},
    normalize: normalizeUserData,
  })

  ...
}
```

Now, let us go to the `query.js` and write down our useQuery custom hook there, later to be imported in `user.js`
This is how `Query` function component looks like as of now
Reference: [query.js](https://github.com/kentcdodds/react-github-profile/blob/egghead-2018/refactor-09/src/screens/user/components/query.js)

```javascript
function Query({query, variables, normalize = data => data, children}) {
    const [state, setState] = useSafeSetState({
        loaded: false,
        fetching: true,
        data: null,
        error: null
    })
    ...
    return children(state)
}
```

Let us make this

```javascript
function useQuery({query, variables, normalize = data => data}) {
    const [state, setState] = useSafeSetState({
        loaded: false,
        fetching: true,
        data: null,
        error: null
    })
    ...
    return state;
}
```

All good, right?

Now, there can be a situation where we'll still need the renderProps based implementation like the previous Query component. So, let us recreate one using this useQuery custom hook only

```javascript
const Query = ({children, ...props}) => children(useQuery(props))
```
##  Handle componentDidMount and componentWillUnmount in React Component Refactor to Hooks )

Now, we will be looking at the GitHubClientProvider class and trying to refactor it to a function component
Reference: [github-client.js](https://github.com/kentcdodds/react-github-profile/blob/egghead-2018/refactor-09/src/github-client.js)

__Old Class component__

```javascript
class GitHubClientProvider extends React.Component {
  constructor(...args) {
    super(...args)
    this.state = {error: null}

    //Conditional setting state properties
    if (this.props.client) {
      this.state.client = this.props.client
    } else {
      const token = window.localStorage.getItem('github-token')
      if (token) {
        this.state.client = this.getClient(token)
      }
    }
  }
  componentDidMount() {
    if (!this.state.client) {
      navigate('/')
    }
    this.unsubscribeHistory = history.listen(() => {
      if (!this.state.client) {
        navigate('/')
      }
    })
  }
  componentWillUnmount() {
    this.unsubscribeHistory()
  }
  ...
  ...
}
```

Now, before starting the refactor, we see that that there is a state property called client which is getting assigned a value conditionally and in that condition, we are looking up the localStorage. It is not a good practice that everytime, the component renders, you have to look up the localStorage. So, we will try to refactor in a way, that this part of code is called only once when the initial state is set.
`useState` hook can also take an initializer function which will return something and that something will be set as the initial value of the state

```javascript
function GitHubClientProvider(props) {
    const [error, setError] = useState(null);
    const [client, setClient] = useState(() => {
        if (props.client) {
            return props.client
        } else {
            const token = window.localStorage.getItem('github-token')
            if (token) {
                return getClient(token)
            }
        }
    })

    useEffect(() => {
        if(!client) {
            navigate('/')
        }
        const unsubscribeHistory = history.listen(() => {
            if (!client) {
                navigate('/')
            }
        })
        return () => unsubscribeHistory()
    }, [])
    ...
    ...
}
```

- useEffect hook does not exactly function as the componentDidMount lifecycle method.
- useEffect hook is a combination of componentDidMount, componentWillUnmount and componentDidUpdate. 
- The best thing about useEffect is that it allows us to setup as well as it gives us slot for a cleanup or teardown function that we can put as the return value of useEffect.

>>**Ques:** Why did I put an empty array as the dependency list argument in useEffect?
**Ans:** This is because we just want to do the operations inside useEffect once when the render happens and when the component unmounts. The inside code won't run if any value or anything changes. Empty array signifies that there are no variables on change of which the callback function will rerun. So, that is why we pass an empty array as the dependency list argument.

Now, let us refactor the `getClient` method. This is going to be pretty straight-forward and easy-peasy.

__Old component code__

```javascript
class GitHubClientProvider extends React.Component {
    ...
    ...
    getClient = token => {
        const headers = {Authorization: `bearer ${token}`}
        const client = new GraphQLClient('https://api.github.com/graphql', {
        headers,
        })
        return Object.assign(client, {
        login: this.login,
        logout: this.logout,
        })
    }
    logout = () => {
        window.localStorage.removeItem('github-token')
        this.setState({client: null, error: null})
        navigate('/')
    }
    login = async () => {
        const data = await authWithGitHub().catch(error => {
        console.log('Oh no', error)
        this.setState({error})
        })
        window.localStorage.setItem('github-token', data.token)
        this.setState({client: this.getClient(data.token)})
    }
    ...
    ...
}
```

to

```javascript
const getClient = token => {
    const headers = {Authorization: `bearer ${token}`}
    const client = new GraphQLClient('https://api.github.com/graphql', {
        headers,
    })
    return Object.assign(client, {
        login,
        logout
    })
}
function logout() {
    window.localStorage.removeItem('github-token')
    setClient(null);
    setError(null);
    navigate('/')
}
async function login() {
    const data = await authWithGitHub().catch(error => {
    console.log('Oh no', error)
    setError(error)
    window.localStorage.setItem('github-token', data.token)
    setClient(getClient(data.token))
}
```

Last thing left to refactor is the render() function which is again right up straight-forward. Just copy whatever render() function returns and return it within our GitHubClientProvider function.

I hope this refactor revised whatever you have learnt so far🙂

## Dynamically Import React Components with React.lazy and Suspense 

With React 16.6.0, [React Suspense](https://reactjs.org/docs/concurrent-mode-suspense.html) was officially released as a stable feature (with limited support for [React.lazy](https://reactjs.org/docs/code-splitting.html#reactlazy)).
Suspense lets your components “wait” for something before they can render.
The React.lazy function lets you render a dynamic import as a regular component. This helps in code-splitting and lets you ship minimal JS code for the first time and let the components to render lazily when needed.

**Before**

```javascript
import OtherComponent from './OtherComponent';
```

**After**

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

This will automatically load the bundle containing the OtherComponent when this component is first rendered.

Coming to our code, we are already using `react-loadable` library which does the same thing.
What we are doing here is that we are dynamic importing our Home and User pages so that we can leverage [code-splitting](https://reactjs.org/docs/code-splitting.html) and make our users download less of our application all at once. Here we are passing the pages to Reach Router and when the right url path hits, the component is loaded and rendered.

**src/index.js**

```javascript
const Home = loadable({
  loader: () => import('./screens/home'),
  loading: LoadingFallback,
})

const User = loadable({
  loader: () => import('./screens/user'),
  loading: LoadingFallback,
})

function App() {
  return (
    <ThemeProvider>
      <GitHubContext.Provider>
        <ErrorBoundary FallbackComponent={ErrorFallback}>
          <Router>
            <Home path="/" />
            <User path="/:username" />
          </Router>
        </ErrorBoundary>
      </GitHubContext.Provider>
    </ThemeProvider>
  )
}
```

React has now got the 
We can get rid of the `react-loadable` import and refactor this to

```javascript
const Home = React.lazy(() => import('./screens/home'))
const User = React.lazy(() => import('./screens/user'))
```

If we save this and go over here, we're going to get a big error that says "A component suspended while rendering but no fallback UI was specified." We need to add a Suspense fallback component higher in the tree to provide a loading indicator or some sort of placeholder to display.

![react-dynamically-import-react-components-with-react-lazy-and-suspense-component-suspended-error.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1666928540088/Nx51_zND1.jpg align="left")

We are going to pull in Suspense from "react" and wrap our component tree with Suspense just one level down the `<ErrorBoundary/>`.

```javascript
<ThemeProvider>
    <GitHubContext.Provider>
    <ErrorBoundary FallbackComponent={ErrorFallback}>
    <Suspense
        fallback={
            <LoadingMessagePage>Loading Application</LoadingMessagePage>
        }
    >
        <Router>
            <Home path="/" />
            <User path="/:username" />
        </Router>
    </Suspense>
    </ErrorBoundary>
    </GitHubContext.Provider>
</ThemeProvider>
```

If an error is thrown in any of the pages, Suspense will throw it and ErrorBoundary will catch it.

**Summarizing what we did:**
We're importing `Suspense` and using `React.lazy`. Then we provide our Suspense inside of our `<ErrorBoundary>`, somewhere above where we're using React.lazy components and we're providing a fallback for the `<LoadingMessagePage>` of Loading Application.

##  Preload React Components with the useEffect Hook 

Sometimes, it is great to pre-load the next page before hand because we know that the user is always going to navigate to it after the current page. For example, in our use-case:
While users are filling out the form on our home page, it would be a good idea to pre-load the next page they will be going to so they don't have to wait for it to load once they've finished filling out the form. `useEffect` hook makes this really easy.

We are right now using `React.lazy` to dynamically load our pages. Suppose, our user is running on very low connectivity, the next page (User) will take time to load and that can be a bad user experience. Instead, what we can do is load `User` also at the time of loading `Home`.

One method to do this is that in our Home component, we will useEffect and inside it we will preload the User page. When the component mounts, this piece of code will run

```javascript
function Home() {
  useEffect(() => {
    // preload the next page
    import('../user')
  }, [])

  ...
}
```

Now, if I refresh, after the resources for Home page are fetched and the bundles and chunks for `Home` are loaded, we're actually going to follow up with a request for our other chunks that we need for the `User` page.
