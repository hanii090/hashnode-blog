---
title: "Learn to Scale React Development with Nx"
datePublished: Wed Nov 02 2022 00:00:04 GMT+0000 (Coordinated Universal Time)
cuid: cl9yvh512000a09l17ijnhjpg
slug: learn-to-scale-react-development-with-nx
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1667346901749/xH5esQLlD.png
tags: reactjs, beginners, scale, nx

---

# Create a new empty Nx Workspace

 

> Nx is a set of extensible dev tools for monorepos, which helps you develop like Google, Facebook, and Microsoft. [Nx site](https://nx.dev/react)

We can create and configure a new NX workspace with the command

```shell
npx create-nx-workspace <workspace name>
```

Running this command will give you a set of options that allows you to create the best workspace for your needs.

- empty
- oss
- web components
- angular
- angular-nest
- react
- react-express

Then it will also ask you about which CLI should be activated in your workspace

- NX - used for all workspaces
- Angular CLI - used for Angular workspaces

Finally, you get asked if you want to use the NX cloud, this is the commercial version of NX. There is a free tier which provides better performance and distribution of cache between co-workers.

## The Nx workspace

The workspace contains different files and folders.

- `Apps` folder - contains all the applications we want to host
- `Libs` folder - contains all the code, usually, applications will link to code inside this folder
- A single `node_modules` folder for the workspace
- The `package.json` serves as the global file for packages for the whole workspace
- `workspace.json` contains all the configuration of your projects and how to build it
- `nx.json` contains the configuration of the workspace


# Generate a new React app with Nx
 

When using the `nx list` command, we can see all the plugins available to be used in our workspace. We can use plugins for next, nest, node, angular, etc. There are also community plugins that give us more options like support for vue or go - just to name two of the options.

## Creating a React App

We can install `@nrwl/react` to get a react app started.

Using yarn

```shell
yarn add @nrwl/react
```

Using npm

```shell
npm i @nrwl/react
```

Now we can run `yarn nx list @nrwl/react` to see all the possibilities that this plugin gives us.

- Create an application
- Create a library
- Create a component
- Create a redux slice for a project
- Set up storybook
- Generate storybook story
- Create a cypress spec

_**Note:** Hanii is using yarn on the course, so these notes will only contain the yarn version of running the commands, but you can use npm instead._

### Let's create the React Application

By running the command `yarn nx generate @nrwl/react:application <name>` you can also use `yarn nx g @nrwl/react:application <name>` for short.

When running this command, Nx will ask you what kind of stylesheet format you want to use - for example, CSS, SASS, Stylus, etc.

We can also append the `--dry-run` flag to the previous command to allow us to see all the files that this command would create.

When creating a React Application there are a few configuration files that are automatically created for us.

- `.eslintrc`
- `babel.config.json`
- `jest.config.js`

# Generate new Projects for Nx with Nx Console
 

In Nx, you will usually use the command line to generate new things.

You can install the Visual Studio Code extension [Nx Console]

(https://nx.dev/latest/react/cli/console#nx-console) from the [VS Code extension tab](https://marketplace.visualstudio.com/items?itemName=nrwl.angular-console)

This extension allows you to have a better view of your workspace, what projects you have, common Nx commands, and also run project commands.

Using Nx Console will help you save time looking for the commands available in Nx by using the CLI.
# Running a React App in the Browser with Nx
 

Let's talk about the `workspace.json` - this file contains all the projects available in the workspace, current configuration and how you can run, build, test or anything that you can do in your projects.

```json
{
  "version": 1,
  "projects": {},
  "cli": {},
  "schematics": {},
  "defaultProject": "store"
}
```

Inside `projects` we can see all the projects inside our workspace. Currently, we have `store` and `store-e2e`.

```json
 "store": {
      "root": "apps/store",
      "sourceRoot": "apps/store/src",
      "projectType": "application",
      "schematics": {},
      "architect": {
        "build": {},
        "serve": {},
        "lint": {},
        "test": {}
      }
 }
```

If we open the `store`, we can see the basic information for the application, we also have this `architect` which contains all the targets that an app can run.

- Build the app
- Serve the app
- Lint the app
- Test the app

## Running the React App

To running the React Application we need to `serve` the app. If we open `serve` we can see a few things:

```json
{
  "serve": {
    "builder": "@nrwl/web:dev-server",
    "options": {
      "buildTarget": "store:build"
    },
    "configurations": {
      "production": {
        "buildTarget": "store:build:production"
      }
    }
  }
}
```

- The `builder` entry is responsible for running the application.
- The `options` allow you to pass options to the command, for example, to change a port.

Let's go into our command line and use the command to run the React Application.

```shell
yarn nx run <name of the project>:serve
```

So in our case we will run `yarn nx run store:serve`

_**Note:** `workspace.json` contains a `defaultProject`, this allows us to run the command `yarn start` to run the `serve` command of the project set as default. Have a look at `package.json`._



# Install and use external npm packages in an Nx Workspace

 

We can install npm packages in an Nx Workspace like we install any npm package.

### Using yarn:

```shell
yarn add <package name>
```

### Using npx:

```shell
npx install <package name>
```

The thing to keep in mind is that, since we have a global `package.json` file, the package that we are installing, will be available for the entire workspace.
 


# Add Styling to a React app inside an Nx workspace
 
  we have selected to use SASS for our stylesheet, so any style being placed inside the `styles.scss` file will be compiled for us.

There are a few ways you can manage your stylesheets.

- You can import them into that `styles.scss` file
- Update the `styles` array inside the `build` in the `workspace.json` file
  - Any file added to the array will be compiled at build time

## Using the `style` flag

If you already know what sort of file extension you want to use for your styles, you can use the `--style` flag together with the `npx generate` command to specify a format.

For example to use Less you can run the command

```shell
yarn npx generate @nrwl/react:application --style less
```

_**Note:** Nx will save your style options and will use that throughout the whole workspace._

# Configure Assets for my React app in an Nx Workspace
 

Nx creates an _assets_ folder inside our apps where we can serve assets such as images, sounds, videos, etc.

Any file inside this _assets_ folder, will be available for us to link to by just using the relative path of the file. For example:

```html
<img src="/assets/beans.png" />
```

## Changing or adding folders to assets

```json
{
  "version": 1,
  "projects": {
    "store": {
      "root": "apps/store",
      "sourceRoot": "apps/store/src",
      "projectType": "application",
      "schematics": {},
      "architect": {
        "build": {
          "builder": "@nrwl/web:build",
          "options": {
            "outputPath": "dist/apps/store",
            "index": "apps/store/src/index.html",
            "main": "apps/store/src/main.tsx",
            "polyfills": "apps/store/src/polyfills.ts",
            "tsConfig": "apps/store/tsconfig.app.json",
            "assets": ["apps/store/src/favicon.ico", "apps/store/src/assets"],
            "styles": ["apps/store/src/styles.scss"],
            "scripts": [],
            "webpackConfig": "@nrwl/react/plugins/webpack"
          },
        },
    },
```

Similar to the stylesheet, you can add more folders to be used as assets by opening your `workspace.json` and inside the `build` you can see an `assets` array. Add the path of the folder that you want to include and Nx will start serving the contents from that folder.

You can also copy assets by using the extended way of copying assets.

```json
{
  "assets": [
    "apps/store/src/favicon.ico",
    "apps/store/src/assets",
    {
      "input": ".",
      "glob": "*.png",
      "output": ""
    }
  ]
}
```

Let's have a look at what each of these means:

- `input` points to some folder location
- `glob` pattern of what you want to copy - in the example we are copying only PNG files
- `output` points to the output directory, where the asset should be copied


# Create a Shared React Library in an Nx Workspace

 
> "The real power of Nx comes when you have to scale your development across different teams and across different applications. Nx is actually designed to work in large monorepos where you have most of your applications or a subset of correlated applications within the same workspace."

Let's look at the difference between **Applications** and **Library**.

In essence, the applications living inside the `apps` folder will use different libraries, so they will bundle the code inside the `libs` folder.

The Library will contain code that we want to import from the `apps` folder. This is where all our components and other code will live and the amount of code inside the `libs` folder is always larger than the one in the `apps` folder.

## Create a React Library

Similar to how we can create a new React Application, we will use the command line to create a new Library.

```shell
yarn nx g @nrwl/react:lib <name of library> --directory=<name of directory>
```

Since you can have different applications in your Nx workspace, it's a good idea to put new libraries inside a directory that contain the name of the application that will use this library.

This is why we used the `--directory` flag, to create a new library inside a `store` folder, since we will use this library on our store.

## Generating a React Component

When we create a React Library, Nx will also generate a placeholder component for us. Let's see how you can generate a React component with the command line.

```shell
yarn nx g @nrwl/react:component <name of component> --project=<name of library>
```

Remember that we created a `ui-shared` library earlier. By using the `--project` flag, we can specify which library should this component be generated in. This flag takes the combination of the library path.

\*_Example:_

```shell
yarn nx g @nrwl/react:component header --project=store-ui-shared
```

This command will create a header component inside the `store` and inside the `ui-shared` folder.

_**Note:** You can go into `workspace.json` and inside `projects` you can see a `store-ui-shared` node created - this is what we use in the `--projects` flag._

After running the component generate command, Nx will ask if you want the component to be exported.

If you are using the component in the Application and is not an internal component to the library, then you should select yes.

## Using the newly created component

Since we exported this component, we can open the `tsconfig.base.json` file and scroll down to `paths` to see the paths of each created component.

```json

{
  "compileOnSave": false,
  "compilerOptions": {
    "rootDir": ".",
    "sourceMap": true,
    "declaration": false,
    "moduleResolution": "node",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "importHelpers": true,
    "target": "es2015",
    "module": "esnext",
    "typeRoots": ["node_modules/@types"],
    "lib": ["es2017", "dom"],
    "skipLibCheck": true,
    "skipDefaultLibCheck": true,
    "baseUrl": ".",
    "paths": {
      "@nxnothanii/store/ui-shared": ["libs/store/ui-shared/src/index.ts"]
    }
  },
  "exclude": ["node_modules", "tmp"]
```

You can see that we have `@nxnothanii/store/ui-shared` created inside `paths`. This is what we need to use to import our component.

```react
import { Header } from @nxnothanii/store/ui-shared
```




# Generate a TypeScript library in an Nx Workspace

 
To create a plain TypeScript library, we can run the command:

```shell
yarn nx g @nrwl/workspace:lib <name of library> --directory=<directory name>
```

_**Note:** Since we are using a plain TypeScript, we don't need to use `@nrwl/react` plugin, we instead just generate a library inside the workspace with the command `@nrwl/workspace:lib`._

Similar to creating components, we can check how this TypeScript library is being exported by looking in the `paths` node inside the `tsconfig.base.json` file. Then we can use that to import the TypeScript library inside our Application.

```react
import { formatRating } from '@nxnothanii/store/util-formatters'
```

_**Note:** `formatRating` was just a function that we used in this new library that we created._


# Use the Nx Dependency Graph to Visualize your Monorepo Structure

 
> "Behind the scenes, Nx understands the structure of your workspace. It create that dependency graph that understands the relationship between applications that live up here and libraries down here, or even between the libraries."

We can launch the visual tool that draws the dependency graph with the command:

```shell
yarn nx dep-graph
```

Then you can go to http://127.0.0.1:4211 in your browser, to see and interact with the graph.

The visual tool starts empty, but you can select apps, projects and libraries by using the checkboxes. You can also click the _Select All_ button to see a visual representation of your whole monorepo.

**Good to know**

A few things that are good to know about the graph representation of your project:

- Applications are represented by squares
- Libraries are represented by circles
- The arrows show the libraries that an application imports

If you click on each element of the graph you can focus the view on that element to get a better understanding of what relationships each element has.



# Create React Feature Libraries in an Nx Workspace

 
Let's see why you should keep the bulk of your code inside the `libs` folder.

- Reusability of the code, you can use it on different apps.
- Bounded context, you can group together some functionality and use it in an app.
  - This makes you think what should be private and what should be public.
  - Help speed compilation/testing/linting.

 we create a new library, but we are using a new flag `--appProject`. This flag allows us to generate a router component in that project and link it to the generated library.

```shell
yarn nx g @nrwl/react:lib future-game-detail --directory=store --appProject=store
```

In the  example, we are creating the `future-game-detail` inside the `store` directory under `libs` and we are setting that router component.

# Create an Express Backend API in an Nx Workspace

 
As we have seen before, we can install a various number of plugins in our Nx workspace.

Let's install [Express](https://expressjs.com/) to create an API for the project.

```shell
yarn add -D @nrwl/express
```

After installation of this plugin, we can now generate a new express application with the command:

```shell
yarn nx generate @nrwl/express:application <name>
```

You can also use useful flags on this command. For example, using the `--frontendProject` flag will set up that project as the frontend and Nx will automatically set up a proxy for us.

```shell
yarn add -D @nrwl/express api --frontendProject=store
```

This command will start a new application called `api` and set up `store` as our frontend project.

If we run this command, we can see that inside our `store` folder, a `proxy.conf.json` file was set up by Nx.

```json
// content of proxy.conf.json
{
  "/api": {
    "target": "http://localhost:3333",
    "secure": false
  }
}
```

Now if we run our store application, the `/api` route will be forward to http://localhost:3333.

## Use Run Commands to launch the API and App of an Nx Workspace

 
To have the two applications working (the frontend and the backend), we need to run two commands - one to run each application.

```shell
yarn run <app name>:serve
```

For example to run both the api and the store application we can run in one terminal:

```shell
yarn run api:serve
```

And in another terminal

```shell
yarn run store:serve
```

When we have both applications running we can now go to http://localhost:4200/api to get the api results.

_**Note:** Since Nx created a proxy for us, the store application is running on port 4200 whilst the api one is running on a different port. We can access the api application, because of that proxy._

## Running both applications with a single command

Nx allows us to run as many applications as we need with the command `run-many`.

```shell
yarn nx run-many --target=serve --projects=api,store --parallel=true
```

That's a lot of flags! Let's have a look at them:

- `--target` is the type of command we want to run - we want to use `serve` to run both applications
- `--projects` these are all the applications we want to run in the command, they are comma-separated
- `--parallel` is used because we want to serve both projects together at the same time
  - If we don't use it, Nx would run the first project until terminated, before starting the next one

## Creating a run command to launch both applications

Let's open the `workspace.json`, inside the `store` we can see that inside the `architect` node we have a few targets like _build_, _serve_, _lint_ and _test_.

We can create a custom target that will allow us to start the `api` application together with the store. That way we won't rely on the `run-many` command.

```json
{
  // Inside the architect node
  "serveAppAndApi": {
    "builder": "@nrwl/workspace:run-commands",
    "options": {
      "commands": [
        {
          "command": "nx run api:serve"
        },
        {
          "command": "nx run store:serve"
        }
      ]
    }
  }
}
```

- Each command has to have a builder which has the implementation to run the job.
- We can now choose `options` which are commands that we want to execute.

_**Note:** `run-commands` will run the commands in parallel by default._

Now we can run our custom target with the command:

```shell
yarn nx run store:serveAppAndApi
```

> "Run commands are very powerful because you can encapsulate whatever script you want to run within your workspace configuration, where all our scripts and run commands are."
 


## Connect to an Express.js API from a React app in an Nx Workspace

 
Since we set up `store` as the frontend when we created our `api` application and Nx created that proxy for us, we can now use that url to fetch data straight from our backend.

So if we try to fetch `/api/games`, that request will use our proxy and fetch the data from out backend which is running on port 3333:

```react
fetch('/api/games')
  .then((x) => x.json())
  .then((res) => {
      setState({
          ...state,
          data: res,
          loadingState: 'success'
      })
  })
```
 
## Share and Reuse functionality with libraries in Nx

 
Similar to how we import shared libraries in our Applications, we can also do the same in our libraries inside the `libs` folder.

To use our `formatRating` we can import it as we did on the `store` application.

```react
import { formatRating } from '@nxnothanii/store/util-formatter'
```

 



## Share code between a React Frontend and Node.js Backend Application in Nx

 
Let's see how we can share code between applications and libraries.

We start by creating a new library called `util-interfaces` with the generate command.

```shell
yarn nx generate @nrwl/workspace:lib util-interfaces --directory=api
```

You can see that we are creating a new directory inside our `libs` folder because this library is related to our `api` application.

Then we can create a new Game interface inside the file `api-util-interfaces.ts`

```typescript
export interface Game {
  id: string
  name: string
  image: string
  description: string
  price: number
  rating: number
}
```

_**Note:** We renamed the file `game.ts`_

This interface represents the structure of a game that gets exposed by our api.

Since we changed the name of the file, we need to update the export inside our `index.ts` file (`libs/api/util-interfaces/src/index.ts`)

```typescript
export * from './lib/game'
```

We can know import our Game interface with:

```react
import { Game } from '@nxnothanii/api/util-interfaces'
```

> " having the backhand also in the same monorepo can bring a lot of advantages because, in this way, we cannot even share just simple types as we did in our very simple example here, but maybe even some business logic that needs to be repeated on the frontend side as well as on the backend side once the data arrives."


## Use Storybook to Develop React Components in Nx

 
Nx comes with a dedicated packaged for storybook - `@nrwl/storybook` that adds a series of useful code generators to add storybook to an existing React library.

Let's install the storybook package.

```shell
yarn add -D @nrwl/storybook
```

Now we can run the following command to create a Storybook for our shared components.

```shell
yarn nx generate @nrwl/react:storybook-configuration store-ui-share --configureCypress --generateStories
```

_**Note:** Our Header component is inside the `store-ui-share` so this is the name of the product that we want to add the Storybook configuration._

Let's take a brief look at these flags.

- `--configureCypress` set up a Cypress test configuration
- `--generateStories` generate stories automatically for components inside the `store-ui-share` library

We can run Storybook with the command:

```shell
yarn nx run store-ui-shared:storybook
```

Storybook will be run on http://localhost:4400, we can go to this url in our browser and see all the available stories that were automatically generated for us.

## Use Cypress and Storybook to test your React Components in Isolation

 
Since we created a storybook with the `--configureCypress`, we can run Cypress with the command:

```shell
yarn nx run store-ui-shared-e2e:e2e
```

_**Note:** `store-ui-shared-e2e` was created because we added that `--configureCypress` flag._

We can use the `--watch` flag to the command that starts the Cypress test and allow us to see what is happening.

## Run Jest tests for a React app with Nx

 
When we generate an Application or Library inside an Nx workspace, Nx will preconfigure Jest and will set some basic tests.

We can run our tests with the command:

```shell
yarn nx run store:test
```

If we run this command, the basic tests that were created will fail, because we have changed our application so we need to update our tests.

If you want to have the same experience with jest by using the jest watcher, if you pass the `--watch` flag to the previous command.

```shell
yarn nx run store:test --watch
```

## Building your React app with Nx

 
As we have been doing throughout the course, we can open the `workspace.json` file and see what targets are available to us.

To build our project we can run the command:

```shell
yarn nx run store:build --configuration=production
```

Alternatively, you can run the `nx build` command that is set up for us by Nx

```shell
yarn nx build store --configuration=production
```

You can see that we _production_ to the `--configuration` flag, that tells Nx that you are building for production.

After running the command we can see that the production-ready build was created on `dist/app/store`.






## Linting a React app with Nx

 

Nx also generates a `.eslintrc` files when we start a new workspace - this file contains some linting rules that are set up for us.

Since the lint rules were set up for us we can run the lint command:

```shell
yarn nx run store:lint
```

This will lint our `store` applications for potential issues.




## Scale CI runs with Nx Affected Commands
 

One of the issues with working with monorepos is that the more our workspace grows, the slower our CI will be.

Also, you probably don't want to build the whole workspace if you have changed a few files in a part of the workspace like an application.

Nx provides an _affected command_ that helps us build only the things that have changed.

```shell
yarn nx affected:<target> --base=<branch - defaults master>
```

We can have a visual representation of the changes done on our workspace with the command:

```shell
yarn nx affected:dep-graph --base=16-adjust-jest-test
```

Let's say that on our CI we want to run all the affected tests and linting for the changes that we are trying to merge.

We can run the `test` target for all affected changes.

```shell
yarn nx affected:test --base=16-adjust-jest-test
```

Then we can run the `lint` target for all the affected changes.

```shell
yarn nx affected:lint --base=16-adjust-jest-test
```

> " As you can imagine, this is really powerful especially in your CI configuration. For instance, to run all the test cases, you would just use the test target here on your CI configuration to run only those projects that have been touched by a given PR."
 



## Speed up with Nx computation Caching
 

> "Nx shines when it comes to scale your monorepo to the masses."

Nx uses a computation cache. This means that whenever we run a command, Nx will cache it so if we run the same command again it will get the results from the cache and return it.

This is useful because in large monorepos using cache will save us a large chunk of time. Also, if we haven't touched a particular part of our workspace there is no point of running a _test_ for example on that project since nothing has changed.

Nx also has a flag that allows you to skip the cache when running the `affected` command.

```shell
yarn nx affected:test --all --skip-nx-cache
```

## Nx cloud

When we created our Nx workspace, we got prompted if we wanted to use Nx cloud.

Nx cloud brings benefits when dealing with cache because it will be shared among all the users of our workspace. So our coworkers can benefit from it.





## References

- [List of available options for the serve command](https://nx.dev/latest/react/plugins/web/builders/dev-server)

- [Material UI website](https://material-ui.com/)


- [Why Nx?](https://nx.dev/latest/react/getting-started/why-nx)
- [ExpressJs](https://expressjs.com/)


- [Nx run-many command](https://nx.dev/latest/react/cli/run-many#run-many)
- [Nx run-commands](https://nx.dev/latest/react/plugins/workspace/builders/run-commands)

- [WebPack dev proxy](https://webpack.js.org/configuration/dev-server/#devserverproxy)


- [Storybook](https://storybook.js.org/)

- [Cypress](https://www.cypress.io/)
 
- [Affected command](https://nx.dev/latest/react/cli/affected#affected)


