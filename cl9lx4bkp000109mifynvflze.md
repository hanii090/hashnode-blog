---
title: "React Real Time Messaging With GraphQL"
datePublished: Sun Oct 23 2022 22:25:05 GMT+0000 (Coordinated Universal Time)
cuid: cl9lx4bkp000109mifynvflze
slug: react-real-time-messaging-with-graphql
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1666563328245/xN9NDo2H7.png
tags: javascript, react, reactjs

---

# Query Multiple Services with OneGraph GraphiQL Editor

 

## Summary

`OneGraph` provides a single GraphQL endpoint that sits between our applications and third party services we might want to use. It aggregates these separate services together into a single GraphQL endpoint, and creates a consistent authentication flow for our application.

## Problem

Modern applications typically make several requests to third party services to handle isolated functionality - charging a card with Stripe, fetching tweets from Twitter, issues from GitHub etc. This requires your application to know how to talk to each of these individual services.

![1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562523984/mm6aYXu2f.png align="left")


It also requires your application to speak to each of these services in their language of choice.

 
![2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562566177/wxB2IL4ps.png align="left")


`OneGraph` sits between your application requests and these services.
 
![3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562576348/nYFNYV4Wg.png align="left")




`OneGraph` exposes these services via a single GraphQL endpoint - meaning your application only needs to speak one language.

 
![4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562586397/psKYvc7rP.png align="left")

## OneGraph

`OneGraph` aggregates a collection of SaaS (Software as a Service) products together into a single, consistent GraphQL endpoint. It also allows us to authenticate each service once, and then query or mutate data across all our services, without leaving GraphiQL.

Since most of these services require authentication or api keys to know who is making the request, we must first create a `OneGraph` account, and a new application. Navigate to the [OneGraph Dashboard](https://www.onegraph.com/dashboard) and create an app.

> Note: The app is a collection of services that you want to be able to query and mutate from your application. `OneGraph` will bundle these services up into one graph (no pun intended), to make interacting with these different services super simple. Each app can have its own unique collection of services, and its own graph.

`OneGraph` has a great collection of integrated services, that is constantly growing. You can check out all the supported services and start building queries in their [OneGraphiQL Explorer](https://www.onegraph.com/graphiql).


#  Query Multiple Services with urqls GraphQL Client
 
## Summary

`urql` is a light-weight data fetching library that we can use to send queries and mutations to our `OneGraph` endpoint. It provides us with some convenient variables to determine whether our application is in a loading or error state, or whether we have the data required to display our component.

## Create React App

In this video we will create a new React application using the `create-react-app` package.

```bash
npx create-react-app name-of-our-app
```

The `create-react-app` package allows us to quickly create a React application without needing to write a bunch of boilerplate code, or configure Webpack, babel etc. Feel free to swap this out with your own custom React application, or another framework built on React - such as Next or Gatsby - but this may take some extra configuration and googling.

## Urql

`urql` is a data fetching library that we can use to send queries and mutations to our GraphQL API (OneGraph). In order to specify queries, `urql` also requires the `graphql` package.

`yarn add urql graphql`

Now we need to create an `urql` client and wrap our application in their provider.

```js
// src/index.js

// other imports
import {createClient, Provider} from 'urql'

const client = createClient({
  url: 'replace-with-graphql-endpoint-from-onegraph',
})

ReactDOM.render(
  <React.StrictMode>
    <Provider value={client}>
      <App />
    </Provider>
  </React.StrictMode>,
  document.getElementById('root'),
)
```

Now from within any of our React components we can declare a GraphQL query that describes the data our component wants.

```js
const QUERY = `
  query {
    npm {
      package(name: "graphql") {
        name
      }
    }
    devTo {
      articles {
        nodes {
          title
        }
      }
    }
  }
`
```

Then we can pass this query to the useQuery hook from `urql`. This hook returns the result of the query, as well as a function to trigger this query again.

```
const [result, reExecuteQuery] = useQuery({ query: QUERY })
```

> Note: the query gets executed as soon as the component is mounted, if we want to re-run this query after some user action - such as clicking a button to add a todo - we would call the `reExecuteQuery` function.

Now, `console.log({result})` the result from the Query and open the developer tools. You'll see that the same data structure from our GraphiQL Editor is being returned in our application.
 

# 03. Refactor commonly used variables into a utils file to be shared across our app
 

**Goal: Share Urls, Ids, and common strings throughtout the app**

When you have strings that need to be a specific, consistent pattern (think urls and Ids), you can export them as variables in a utils folder so you don't have to worry about typing them out across your app.

```js
// src/utils/auth.js

export const APP_ID = `9068d449-be97-4449-a4f0-d5c663a3e7dc`
export const CLIENT_URL = `https://serve.onegraph.com/graphql?app_id=${APP_ID}`
```

```js
// src/index.js

import {CLIENT_URL} from './utils/auth'

const client = createClient({url: CLIENT_URL})
```




# Write an Authenticated Query in OneGraph
 
`OneGraphiQL` is a playground to build up queries and mutations across multiple services. It also consolidates authentication logic across multiple services, so we can authenticate and authorize `OneGraph` once, and then make as many queries as we want. This allows us to focus on building the queries and mutations that make our app awesome!

The format for queries and mutations in `urql` match `OneGraphiQL` so we can easily copy them across. The `useQuery` hook from `urql` also accepts a variables parameter, which we can use to pass across any dynamic values our query might need.

`onegraph-auth` provides us a simple way to authenticate with different `OneGraph` services within our application. By using `React Context` we can share this authentication logic across our components.

## OneGraphiQL

`OneGraphiQL` is a data explorer that allows us to build up our GraphQL queries and mutations. It is the `OneGraph` implementation of [GraphiQL](https://github.com/graphql/graphiql), which can be used with any GraphQL endpoint. `GraphiQL` is the perfect way to discover the different things we can request. It is generated from the GraphQL schema and provides helpful documentation for the graph's queries, mutations and types. Additionally, it can intelligently suggest options while we are building our queries and mutations.

⌨️ Pressing `Ctrl + Space` will display suggestions, and pressing `Tab` will autocomplete. This massively improves the speed at which you can build up GraphQL queries and mutations.

## Building queries

`OneGraphiQL` has a helpful `Explorer` on the left, that can be used to discover what our application can query. This demonstrates the awesome convenience of `OneGraph` as we can really easily query a number of services from one consistent GraphQL endpoint. This list of available services is already impressive and will continue to grow over time.

While browsing through the available services, we can click to reveal the fields we can query, and slowly build up our complex request, without even writing code!

## Variables

Sometimes we need to provide a dynamic value to our query or mutation. This is where GraphQL variables can help. We can declare variables that we are expecting to be passed along with the GraphQL request, using the `$` syntax.

```gql
query Todo($id: String!) {
  todo(id: $id) {
    id
    title
  }
}
```

When declaring variables for queries, we need to also declare the type (in this case `String`) and whether the variable is mandatory or optional. The `!` is used to mark a variable as `mandatory` or `required`. Declaring a variable as `required` means that the request will return an error if it is not provided.

We can then use that variable in the place of a value - `todo(id: $id)` - and GraphQL will handle the substitution.

In order to test our query we will need to provide values for any of the `required` variables. This can be done in the `Query Variables` section of `OneGraphiQL`, which accepts a JSON object of key value pairs.

```json
{
  "id": "3"
}
```

Since we declared the `id` variable's type as `String`, we cannot pass the number version of `3`, it must be the string representation `"3"`. Again, the GraphQL request will return an error if we pass it variables of the wrong type.

## Authenticated request

⌨️ To send the GraphQL request, click the `play` button or `Cmd + Enter`.

This will cause an error in the case of the video example, as Github requires us to authenticate and provide an access token. Thankfully, `OneGraph` takes care of this for us and provides a super convenient button to log in to a particular service. In the case of the video example, this button is `Log in to GitHub`. This will require you to authenticate with `GitHub` and grant permissions for `OneGraph` to make requests on your behalf.

Now that we have authenticated with the `GitHub` service, we can re-send our GraphQL request and get back some data 🎉

`OneGraph` keeps that authentication logic in one place, so we can authenticate a collection of services once and then forget about it all together. Now we have authenticated queries, so we can just focus on the data our application needs 🥳

## Urql

Moving the queries and mutations that we built in `OneGraphiQL` to our application is easy! We simply copy and paste the entire query into our query string variable.

```js
// src/App.js

const TODO_QUERY = `
query Todo($id: String!) {
  todo(id: $id) {
    id
    title
  }
}
`
```

## Variables

We do need to handle passing our `id` variable through to the query though. This can be done when we call the `useQuery` hook.

```js
// src/App.js

const [result, reExecuteQuery] = useQuery({
  query: TODO_QUERY,
  variables: {
    id: '3',
  },
})
```


#  Install and create an OneGraph authentication object
 
## Authentication

The last thing we need to implement in order to fetch data from our services is authentication. `OneGraph` provides a package called [onegraph-auth](https://www.onegraph.com/docs/) that we can import into our project.

```bash
yarn add onegraph-auth
```

We can create a new auth instance by passing `onegraph-auth` our `OneGraph` `appId`.

```js
// src/utils/auth.js

import OneGraphAuth from 'onegraph-auth'

export const auth = OneGraphAuth({
  appId: APP_ID,
})
```

# 06. Create an AuthProvider component for our react application to use through React Context

 Basically what this file does is:

1. Creates a new `onegraph-auth` instance
2. Sets the headers for our authenticated GraphQL requests
3. Provides convenient login and logout functions

 
 
## 🤔 React Context

[React Context](https://reactjs.org/docs/context.html) is a way that we can share global variables to any components in our React application. Since we want to be able to use our auth logic multiple places in our application, we can encapsulate this in one `Context` and share it with other components by wrapping the root of our application in the `AuthProvider`.

```js
// src/index.js

// other imports
import {auth} from './utils/auth'
import {AuthProvider} from './contexts/AuthContext'

const client = createClient({
  url: 'replace-with-graphql-endpoint-from-onegraph',
  fetchOptions: {
    headers: auth.authHeaders(),
  },
})

ReactDOM.render(
  <React.StrictMode>
    <AuthProvider auth={auth}>
      <Provider value={client}>
        <App />
      </Provider>
    </AuthProvider>
  </React.StrictMode>,
  document.getElementById('root'),
)
```

Now any components that need to implement authentication logic - such as logging into GitHub - can consume the `AuthContext`.

```js
// src/App.js

// other imports
import {AuthContext} from './contexts/AuthContext'

function App() {
  // we can destructure any values exposed in the AuthProvider
  const {login, status} = React.useContext(AuthContext)

  // before we return our component markup
  // check if authenticated to github
  // if not display a login button

  if (!status || !status.github) {
    return <button onClick={() => login('github')}>Log in with GitHub</button>
  }
}
```
#  Query Github Comments with One Graph through Urql's Client

 
## Summary

This steps through building a new query to fetch all the comments for a particular issue in GitHub. We start by building the query in `OneGraphiQL` and then move this across to our application. Since we stepped through all the authentication logic in the previous video, we should just be able to trigger the query with its variables and get back the data 👍

This demonstrates how simple `OneGraph` makes authenticating requests to our different services!

## Comments

Now that we have authentication and variables sorted in `OneGraphiQL` and our React application, we can use the explorer to select any other fields we need from the GitHub service. In order to get comments we can extend our query to look like this.

```gql
query CommentsListQuery(
  $repoOwner: String!
  $repoName: String!
  $issueNumber: Int!
) {
  gitHub {
    repository(name: $repoName, owner: $repoOwner) {
      issue(number: $issueNumber) {
        id
        title
        bodyText
        comments(last: 100) {
          nodes {
            author {
              login
            }
            body
            id
            url
            viewerDidAuthor
          }
        }
      }
    }
  }
}
```

Again, we can copy and paste that into a string variable in our application. For example we will create a new component for `Comments`.

```js
const COMMENTS_QUERY = `query CommentsListQuery(
  $repoOwner: String!
  $repoName: String!
  $issueNumber: Int!
) {
  gitHub {
    repository(name: $repoName, owner: $repoOwner) {
      issue(number: $issueNumber) {
        id
        title
        bodyText
        comments(last: 100) {
          nodes {
            author {
              login
            }
            body
            id
            url
            viewerDidAuthor
          }
        }
      }
    }
  }
}`
```

Then we need to pass the dynamic variable values for `$repoOwner`, `$repoName` and `$issueNumber`, when we call `useQuery`.

```js
function Comments() {
  // we are just destructuring `result`
  // as we do not need to retrigger this
  // query later
  const [result] = useQuery({
    query: COMMENTS_QUERY,
    variables: {
      repoOwner: 'theianjones',
      repoName: 'egghead-graphql-subscriptions',
      issueNumber: 1,
    },
  })

  // actually render some stuff
  // this will come in the next video
}
```

Now extend our `App.js` file to render the `Comments` component.

```js
// src/App.js

function() {
  // auth logic from previous video

  return (
    <div className="App">
      <header className="App-header">
        <Comments />
      </header>
    </div>
  )
}
```

You can check out the `Network` tab in the developer tools, or `console.log` the result of the `COMMENTS_QUERY` to see the data coming back from `OneGraph`.


#  Display GraphQL Data with A React Component

 
## Summary

When we call useQuery a request is fired off to `OneGraph`, which in turn requests data from GitHub, this means we do not have the `data` immediately. We need to describe how our components look while the data is still loading, and what we display if there is an error in that process.

Conveniently, the `result` variable returned by `useQuery` contains `fetching` and `error` properties that we can use to display our component in those different states.

## Displaying comments

Time to build out the markup to display the comments from GitHub.

```js
// src/components/Comments.js

function Comments() {
  const [result] = useQuery(...) // collapsed for simplicity

  return (
    <ul>
      {result.data.gitHub.repository.issue.comments.nodes.map((commentNode) => {
        return <li key={commentNode.id}>{commentNode.body}</li>
      })}
    </ul>
  )
}
```

This will throw an error as `result.data` does not contain the data until we get it back from `OneGraph`, so we need a way to know whether we are still fetching data.

We can add a check before returning the markup for `data` to confirm we actually have the data, if not we can just render `null`.

```js
// src/components/Comments.js

function Comments() {
  const [result] = useQuery(...) // collapsed for simplicity

  if (!result.data) return <p>Loading...</p>

  // if we reach this line then we must have data
  return (
    <ul>
      {result.data.gitHub.repository.issue.comments.nodes.map((commentNode) => {
        return <li key={commentNode.id}>{commentNode.body}</li>
      })}
    </ul>
  )
}
```

🤔 Conveniently, the `result` variable also contains `fetching` and `error` properties that can be used to determine the state of the query. We can destructure these values off `result`, along with our `data` once it has been returned and refactor our code above to give the user some feedback throughout the request lifecycle.

```js
// src/components/Comments.js

function Comments() {
  const [result] = useQuery(...) // collapsed for simplicity
  const { fetching, error, data } = result

  if (fetching) return <p>Loading...</p>
  if (error) return <p>Oh no... {error.message}</p>

  // if we have not returned a value yet, then we are not loading
  // and haven't got an error, so we must have the data
  return (
    <ul>
      {data.gitHub.repository.issue.comments.nodes.map((commentNode) => {
        return <li key={commentNode.id}>{commentNode.body}</li>
      })}
    </ul>
  )
}
```

# Style A List Component
 
## Summary

The `style` prop can be used to quickly apply CSS rules to our components. It accepts a json object of `key` `value` pairs, where the `key` is the CSS property, and the `value` is the value of that property. Since this is a json object, all keys must be `camelCase` and values surrounded by quotes.

```js
<span style={{fontSize: '2rem'}}>Hello</span>
```

## Styling

There are many ways to apply CSS rules to style React components (see [this article](https://medium.com/@dmitrynozhenko/9-ways-to-implement-css-in-react-js-ccea4d543aa3)). 🤔 Without installing additional packages or writing configuration, `create-react-app` supports three styling options:

1. Inline styles or the `style` prop

```js
<span style={{color: 'red'}}>Red text</span>
```

2. Stylesheets

```css
/* src/heading.css */

.heading {
  font-size: 5rem;
}
```

```js
// src/Heading.js

import './heading.css'

function Heading() {
  return <h1 className=".heading">This is big!</h1>
}
```

3. CSS Modules

```css
/* src/Heading.module.css */

.heading {
  font-size: 5rem;
}
```

```js
// src/Heading.js
import styles from './Heading.module.css'

function Heading() {
  return <h1 className={styles.heading}>This is big!</h1>
}
```

## Style Prop

The quickest and most simple way to style a component is by using the `style` prop. This allows us to declare a json object of key value pairs describing our styling rules. This object can be passed directly to the `style` prop, or declared as a variable outside the component.

```js
// inline

function Heading() {
  return (
    <div>
      <h1 style={{fontSize: '1rem'}}>This text is big</h1>
      <p stlye={{color: 'red'}}>This text is RED!</p>
    </div>
  )
}
```

```js
// outside component

const headingStyle = {
  fontSize: '1rem',
}

const textStyle = {
  color: 'red',
}

function Heading() {
  return (
    <div>
      <h1 style={headingStyle}>This text is big</h1>
      <p style={textStyle}>This text is RED!</p>
    </div>
  )
}
```

As this is a json object, keys follow the same rules as variable identifiers. This means we cannot declare a key using `kebab-case` (the convention for CSS). We instead need to `camelCase` our keys, which may look a little strange if you often write CSS. Values must be either strings or numbers.

```js
const headingStyle = {
  fontSize: '1rem',
  backgroundColor: 'green',
  paddingTop: '2rem',
}
```

[The React docs](https://reactjs.org/docs/dom-elements.html#style) consider the `style` prop to be for convenience purposes - as well as some specific use cases around dynamically calculated styles. It is advised that `stylesheets`, `CSS Modules` or `CSS-in-JS` be used instead.




# Use a GraphQL Mutation to Create a Github Issue Comment

 
## Summary

`Mutations` are like `queries` that allow us to write data. The arguments passed to the `mutation` are wrapped in an `input` object. We still declare the fields we would like to get back from the `mutation`. This is like declaring a `query` that gets run immediately after the `mutation`.

## Mutations

In GraphQL, `queries` are used to read data and `mutations` are used to write data. The way we structure a `mutation` is the same as a `query`, except that we wrap any dynamic variables we want to pass to the `mutation` in an `input` key.

```gql
mutation NewTodo($title: String!, $isCompleted: Boolean) {
  todo(input: {title: $title, isCompleted: $isCompleted}) {
    id
    title
    isCompleted
  }
}
```

We still specify the fields we want to get back after the `mutation` is complete, kind of like a `query` to run immediately after the `mutation`. This allows us to request any fields we want to display in our application, including those that were generated by the server - in the example above `id` was generated with the creation of the todo.

In the video example we are sending an `addComment` mutation to the GitHub service. This requires a `subjectId`, to determine which issue we are commenting on, and a `body`, containing the actual comment text. The `OneGraphiQL` explorer allows us to click through the fields we want to get back after the `mutation` is complete - exactly the same as when we were building our `queries`.
 

#  Use Urqls useMutation to Create Github Issues in a React App

 
## Summary

`useMutation` is a hook we can use to send mutations to our GraphQL endpoint. It's interface is very similar to `useQuery`. We pass it a mutation string, and it returns a `result` and `executeMutation` function that we can call and pass the mutation variables.

## useState

In order to submit comments from our application, we need an `input` element. This requires us to keep a track of what the user has typed. Using the `useState` hook allows us to save the user's input in a variable, that we can pass across to our `NewComment` mutation.

## useMutation

`urql` provides us another handy hook for dealing with mutations called `useMutation`. Similarly to `useQuery`, we pass it our GraphQL mutation string and it gives us back a result and a function to trigger the mutation.

```js
const [result, executeMutation] = useMutation(MUTATION_STRING)
```

> Note: `useMutation` does not trigger the mutation as soon as our component is mounted. This would not make sense with mutations, as we need to get the input from the user first.

The `executeMutation` function allows us to trigger the mutation, once the user has inputted their comment and clicked submit. This function takes an object of key value pairs for our mutation variables.

🤔 Similarly to `useQuery`, the `result` variable contains `data`, `fetching` and `error` values, that we can use to determine the state of our mutation.

# Style our Input Component with CSS-inJS

 
## Summary

The `style` prop can be used to quickly style the different elements of our component. It accepts a json object of key value pairs - the `key` being the property name, and `value` being the value for that property. These styles can be declared inline, in a variable outside our component, or in a separate file. Values can only be strings or numbers. When a number is provided as a value, it is automatically turned into the pixel value - `padding: 8` becomes `padding: '8px'`.

## Styling

We are going to use the `style` prop to style our chat app. The `style` prop accepts a json object of key value pairs, representing each style rule to apply.

```js
<span style={{color: 'red'}}>Hi</span>
```

When a number is specified as the value, it is interpreted as pixels.

```js
<span style={{marginTop: 16}}>Hi</span>
```

^^^ is the same as:

```js
<span style={{marginTop: '16px'}}>Hi</span>
```

`prop` styling can be declared inline or moved into a separate variable.

**inline**

```js
<span style={{color: 'red'}}>Hi</span>
```

**separate variable**

```js
const textStyles = {
  color: 'red',
  padding: '1rem 2rem',
}

function Text() {
  return <span style={textStyles}>Hi</span>
}
```

👍 Inline styles work great for one or two simple rules, any more than that you probably want to declare them in a separate variable. This helps to reduce the clutter in the rendering logic of your component. As the size of the project and number of components grow, it is a good idea to move these styles into their own separate files, and import them in your component.

```js
// src/styles/Text.js

const textStyles = {
  backgroundColor: 'green',
  color: 'red',
  padding: '2rem',
}

export default textStyles
```

```js
// src/components/Text.js

import textStyles from '../styles/Text'

function Text() {
  return <span style={textStyles}>Hi</span>
}
```

🤔 This pattern is very similar to [CSS Modules](https://github.com/css-modules/css-modules), which are also supported by default with `create-react-app`.

#  Write a GraphQL Subscription Query in a Graphiql Editor

 ## Summary

Subscriptions are a way for our client to `subscribe` to particular events - such as a new comment on our GitHub issue. Subscriptions allow two-way communication between our client and server, as we are asking the server to notify our client whenever there is a new comment.

## Subscription

A subscription in GraphQL is similar to a magazine or newspaper subscription in real life. You say something like:

> "I care about this thing, so whenever a new edition comes out I want you to deliver it to my house."

Every time there is new content, the delivery person knocks on your door and you have a fresh copy in your hand!

GraphQL subscriptions work in a similar way, you tell it which events you would like to subscribe to - in the case of the video, new comments on this GitHub issue - and it will notify you whenever new data is available.

## Two-way communication

This requires a slight change in our mental model for how API requests and responses work. With our queries and mutations, we are calling our OneGraph API and it either gives us back the data, or does some mutation stuff and gives us back the data. At that point the communication between our application (the client) and OneGraph (the server) is over. This unidirectional communication is how HTTP requests work - the client makes a request and the server sends a response.

 
![5.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562741940/A-R30oNK7.gif align="left")

With GraphQL subscriptions our client is asking the server to notify it whenever a particular thing happens - this could be immediately, or after a few seconds, days or even months. So the server is not only responding to a single request with a single response, but has a direct connection with the client that it can push data to a number of times. A common implementation of this kind of bidirectional communication is [Websockets](https://medium.com/@tfarguts/websockets-for-beginners-part-1-10796106e207).



![6.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1666562756667/0HGjfK6go.gif align="left")
 
## OneGraph

Again, OneGraph abstracts away this complexity and allows us to build up a GraphQL subscription, and confirm that we are receiving data whenever a new comment is added to our issue in GitHub.

# Extract a view component from our CommentQuery Component
 
## Summary

Currently our `<Comments />` component is responsible for both fetching data, and rendering the result - these are two separate concerns. It is good practice to break up components into small reusable, single responsibility chunks - in this case, one component responsible for fetching the data, and the other describing how that data should actually look. Whenever our components rely on external data, we need to wrap the rendering logic in an `if` statement (or guard), to make sure we have the data we need to render.

## Refactoring Components

It is a good idea to separate your React components into small, single responsibility chunks. In this video we are separating the data fetching logic from the rendering logic. This means we could reuse the data fetching logic somewhere else, and swap out the rendering component to display it in a different way, or visa versa.

When we build a component solely responsible for rendering data that is passed to it, it is a good idea to add a guard statement to ensure you have the data you need to render. In the video example this is done by checking whether `comments` are truthy, and contain at least one `comment`.

```js
if (!comments || comments.length === 0) {
  return null
}
```

This ensures that anything after this line can assume that `comments` exists, is an array, and has at least one `comment`.

🤔 Additionally, we could abstract the data fetching logic into it's own reusable wrapping component using the [render props](https://reactjs.org/docs/render-props.html) or [higher order component](https://reactjs.org/docs/higher-order-components.html) patterns, or abstract this logic into its own [custom hook](https://reactjs.org/docs/hooks-custom.html).

# Write a Subscription GraphQL Query with Urql
 
## Summary

The `onegraph-subscription-client` allows us to add the subscription logic to our `OneGraph` client. The `useSubscription` hook allows our application to subscribe to events from `OneGraph`. In the case of this video, anytime a new comment is added to our GitHub issue, the `useSubscription` result is updated with the new data. This allows us to build complex real time applications, that subscribe to events on external services. Very cool!

## OneGraph

OneGraph have published a package called `onegraph-subscription-client` to make subscriptions super simple.

```bash
yarn add onegraph-subscription-client
```

Again, we need to create a new subscription client at the root of our application.

```js
// src/index.js

// other imports
import {SubscriptionClient} from 'onegraph-subscription-client'

const subscriptionClient = new SubscriptionClient(
  'replace-with-app-id-from-onegraph',
  {oneGraphAuth: auth},
)
```

Next we need to extend our `urql` import statement to include `defaultExchange` and `subscriptionExchange`, and pass them to our createClient statement.

```js
// src/index.js

import { /* other imports */, defaultExchanges, subscriptionExchange } from 'urql'

// subscription logic

const client = createClient({
  // previous options
  exchanges: [
    ...defaultExchanges,
    subscriptionExchange({
      forwardSubscription: (operation) => subscriptionClient.request(operation)
    })
  ]
})
```

Full `index.js` file should look something like this.

```js
import React from 'react'
import ReactDOM from 'react-dom'
import './index.css'
import App from './App'
import {
  createClient,
  Provider,
  defaultExchanges,
  subscriptionExchange,
} from 'urql'
import {CLIENT_URL, auth, APP_ID} from './utils/auth'
import {AuthProvider} from './contexts/AuthContext'
import {SubscriptionClient} from 'onegraph-subscription-client'

const subscriptionClient = new SubscriptionClient(APP_ID, {
  oneGraphAuth: auth,
})

const client = createClient({
  url: CLIENT_URL,
  fetchOptions: {
    headers: {...auth.authHeaders()},
  },
  exchanges: [
    ...defaultExchanges,
    subscriptionExchange({
      forwardSubscription: (operation) => subscriptionClient.request(operation),
    }),
  ],
})

ReactDOM.render(
  <React.StrictMode>
    <AuthProvider auth={auth}>
      <Provider value={client}>
        <App />
      </Provider>
    </AuthProvider>
  </React.StrictMode>,
  document.getElementById('root'),
)
```

## useSubscription

The `urql` package gives us another awesome hook for dealing with subscriptions - `useSubscription`. This takes an object parameter, with a `query` property - the subscription string we built in the `OneGraphiQL` explorer - and a `variables` property - for our dynamic values. It gives us back the `subscriptionResult`, which is automatically updated each time it new data is sent.

```js
const [result] = useSubscription({
  query: SUBSCRIPTION_STRING,
  variables: {
    // any dynamic values we need to provide the subscription
  },
})
```

🤔 Again, this `result` has different properties that we can use to determine the state.

- `data` contains the data sent from the subscription - it is `undefined` until we receive our first comment
- `error` contains any errors that have occurred
- `fetching` is always set to `true` as this is a live connection that can be updated any time.


#  Reduce A GraphQL Subscription Stream into a Collection
 
## Summary

The `result.data` property will be updated with the new event, each time it is triggered. This is great if we only want to display the most recent event, but in most applications we want to display some kind of list of these events. `urql`'s `useSubscription` function takes a second argument - a reducer function - that can help us keep a track of previous events.

## Event data

Each time an event you are subscribed to triggers, the `data` property of the `useSubscription` result will be updated to contain the new event.

```js
const [result] = useSubscription({...}) // simplified

console.log(result.data)
```

The first time a comment is added something similar to this will be printed to the console.

```js
{
  github: {
    issueCommentEvent: {
      action: 'CREATED',
      comment: {
        author: { login: 'theianjones' },
        body: 'This was a cool movie',
        ...
      }
    }
  }
}
```

The next time this is triggered `result.data` will contain the new information.

```js
{
  github: {
    issueCommentEvent: {
      action: 'CREATED',
      comment: {
        author: { login: 'dijonmusters' },
        body: 'The movie was just okay',
        ...
      }
    }
  }
}
```

So since this `result.data` value is being updated with the new value each time, we can't simply render it out and see a log of our comments.

```js
// ❌ This will not work!

function Comments() {
  const [result] = useSubscription({...}) // simplified

  return result.data.map(comment => <p>{comment}</p>)
}
```

We could wire up our own custom way of remembering each of these values with some magical combination of `useState` to declare and array, and `useEffect` to subscribe to those changes, but that sounds like a lot of work!

Thankfully, `urql` saves the day again! The second parameter we can pass to `useSubscription` is a callback that conveniently helps us retain that previous state.

```js
// ✅ This will work!

function Comments() {
  const handleSubscription = (comments = [], commentEvent) => {
    return [...comments, commentEvent.github.issueCommentEvent.comment]
  }

  const [result] = useSubscription({...}, handleSubscription) // simplified

  // now `result.data` is an array so we can iterate over it to render!

  return result.data.map(comment => <p>{comment.body}</p>)
}
```

This style of handler function is often called a `reducer`. Reducers take two parameters - an `accumulator` and the current `value`. `urql` will now stitch together an array of each comment event we receive and use this to update the `result.data` property.
 


 #  Display GraphQL Subscription Results to the UI

 
## Summary

Webhooks allow services to subscribe to events on other services. It can be thought of as a reverse API request, where the subscriber provides a URL to send a request to when an event occurs. `OneGraph` uses webhooks to subscribe to GitHub events - this requires us to explicitly allow non-admin subscriptions for the repo.

## Webhooks

Webhooks are a way for different applications or APIs to subscribe to events on other services. The subscriber says "I want to know anytime this type of event happens, and here is my URL to let me know". Anytime a new event occurs the subscriber receives a request on that URL - usually a HTTP POST request - with the attached event data - usually in the `body` of the request.

This is kind of like a reverse API request. Instead of calling an API to get data, you are providing a URL for the API to call when a new event occurs.

OneGraph uses webhooks to subscribe to GitHub events. This requires the user to be an admin of the repository being subscribed to. Thankfully, OneGraph gives us the ability to allow non-admin subscriptions to particular repos. This option is at the bottom of the `Subscriptions` page.

# Use a GraphQL Query and Subscription Together to Fetch the History and Current Comments
 
## Summary

`useSubscription` allows us to subscribe to new events, but we can't subscribe to a history of events. We can use `useQuery` to fetch a log of historical comments, and merge in new comments from our subscription.

The `useQuery` hook configuration object has a `pause` property which can be set to `false` in order to disable refetching.

## Event History

When we subscribe to new events, we will only get new events that occur after we load the application, and previous events will disappear if we refresh. In order to display the full comment history, we will need to combine our `useQuery` and `useSubscription` logic.

We will use our `useQuery` logic to fetch historical comments, and stitch this together with any new comments that come through our subscription. Conceptually, something like this:

```js
const [queryResult] = useQuery(COMMENTS_QUERY_STRING)
const [subscriptionResult] = useSubscription(COMMENTS_SUBSCRIPTION_STRING)

const comments = [...queryResult.data, ...subscriptionResult.data || []]

return (
  // render the comments
)
```

One issue we will hit with this logic is that `useQuery` is refetching our data. This is causing a double up of new comments. The `useQuery` configuration parameter has a `pause` property. This allows us to explicitly tell useQuery to pause fetching.

```js
// src/components/hooks/useCommentsHistory.js

const [result] = useQuery({
  query: QUERY_STRING,
  variables: {...},
  pause: true
})
```

We can dynamically set the `pause` value based off props.

```js
// src/components/hooks/useCommentsHistory.js
const { pause = false } = props

const [result] = useQuery({
  query: QUERY_STRING,
  variables: {...},
  pause
})
```

In our `CommentsSubscription` component we can use a combination of `useState` and `useEffect` to pause the fetching after the first render.

```js
// src/components/CommentsSubscription.js
const [pauseFetching, setPauseFetching] = React.useState(false)

const commentsHistory = useCommentsHistory({pause: pauseFetching})
const commentsHistoryLength = commentsHistory.length

React.useEffect(() => {
  if (commentsHistoryLength !== 0) {
    setPauseFetching(true)
  }
}, [commentsHistoryLength])
```

This will ensure that we only fetch the comments history once, and then append to it with new comments from our subscription.
 

