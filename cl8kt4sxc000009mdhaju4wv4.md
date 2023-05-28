---
title: "How to Set Up Enzyme and Jest for Testing"
seoTitle: "Setting Up Enzyme & Jest for Testing"
seoDescription: "Optimize Enzyme & Jest Testing: Quick Guide
1. Utilize Create React App.
2. No file changes required.
3. Install Enzyme (Dev Dependency).
4. Resources..."
datePublished: Tue Sep 13 2022 06:13:16 GMT+0000 (Coordinated Universal Time)
cuid: cl8kt4sxc000009mdhaju4wv4
slug: how-to-set-up-enzyme-and-jest-for-testing
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/xkBaqlcqeb4/upload/v1664319875844/NrzV2opgg.jpeg
tags: react, reactjs, testing, enzyme, jest

---

## Create React App

Create React App is the best way to start building a new single-page React application. More information on Create React App in the [resources below](#resources).

## Important Note about Create React App

With Create React App, the changes that the instructor makes to the project files are now unneeded. Simply run the following in the terminal to get started:

```bash
npx create-react-app react-enzyme-jest
```

```bash
cd react-enzyme-jest
```

With our project created, we can install Enzyme as a Dev Dependency:

```bash
npm install --save-dev enzyme
```

That's it! We don't need to make a **.babelrc** file, nor do we need to edit our test script in **package.json**.

## Resources

* [Using Jest with Create React App Issue](https://github.com/facebook/create-react-app/issues/2564)
    
* [Create React App](https://reactjs.org/docs/create-a-new-react-app.html)
    
* [Using Jest with Webpack](https://jestjs.io/docs/en/webpack)