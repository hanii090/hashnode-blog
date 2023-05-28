---
title: "Commit Like a PRO"
datePublished: Fri Nov 11 2022 21:07:32 GMT+0000 (Coordinated Universal Time)
cuid: claczps28000008mdfptz4gao
slug: commit-like-a-pro
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1668200759706/K0nriX0kW.png
tags: productivity, beginners, programming-tips

---


Creating code is easy, but creating *great code* is not. When we start to learn programming, we make what works and we're happy just with that. As we mature our skills. 

> it's important to give ourselves the best environment to produce quality code.

In this article, **we'll learn **

> how to improve our developer experience (DX).

Let's go ahead and dive right in!



## Tools 🧰

To create such a useful environment, we'll use these packages to check our code through git hooks:

- **`Husky`** 🐶: Supercharges our DX by linting, testing, or formatting code as code is committed with git.
- **` ESLint`** ✔️: Checks for certain code patterns to stop errors or potential bugs.
- **` Lint-Staged`** 🔧: Lints code before a commit occurs to keep production code clean.
- **`Prettier`** ✨: Keeps code formatting consistent based on our own preferences.

## Committing Clean Code to a Project

We'll use create-react-app to make the initial project. We won't be doing anything too much with React and you should still be able to follow along.

```Bash
mkdir nothanii-better-code
cd nothanii-better-code
npx create-react-app ./
```

Now that the code has been made, let's go ahead and run it in our browser to see if everything is correct.

```Bash
npm run start
```

Everything should be working just fine! Now before we add our tools, lets make that first commit with just the CRA project.

```Bash
git add .

git commit -m 'init'
```

Great! Now that we know the web app is working, let's add our tools to make clean code.

### Adding ESLint and Prettier

ESLint is put in 'create-react-app` by default, but we will create custom configuration files for both ESLint and Prettier.

Let's install Prettier and eslint-config-prettier and make our config files in root project directory.

```Bash
npm install --save-dev --save-exact prettier eslint-config-prettier
```

Create an ESLint config and check off whatever specifications you'd like for your project.

```Bash
npm init @eslint/config 
```
You can select your own preferences through that command. If you're unsure what to pick, just make a copy of this `.eslintrc.json` in our root directory with the following configuration:

```JSON:.eslintrc.json
{
	"env": {
		"browser": true,
		"es2021": true
	},
	"extends": ["eslint:recommended", "plugin:react/recommended"],
	"parserOptions": {
		"ecmaFeatures": {
			"jsx": true
		},
		"ecmaVersion": "latest",
		"sourceType": "module"
	},
	"plugins": ["react"],
	"rules": {
		"indent": ["warn", "tab"],
		"quotes": ["error", "single"],
		"semi": ["error", "always"]
	}
}
```

Let's also make a `.eslintignore` file to prevent linting on tests:
```Markup
src/*.test.js
```

You'll notice there are a lot more errors appearing in the code than before. 
That is ESLint enforcing our selected code style based in the config file. 
Before we fix these errors, lets make our Prettier config file to help format 
our linted code.

```Bash
touch .prettierrc.json //or create manually
```

Once your config file is made, add the following code to sync up both ESLint and Prettier:

```JSON:.prettierrc.json
{
	"tabWidth": 2,
	"useTabs": true,
	"printWidth": 80,
	"semi": true,
	"trailingComma": "es5",
	"jsxSingleQuote": true,
	"singleQuote": true
}
```

Then modify `eslintrc.json` to include the following as the final item in the `"extends"` configuration:

```JSON
...,
"extends": [
  ...,
  "prettier"
],
...,
```

Now with those two set up, we can move on to Husky and lint-staged.

### Configuring Husky

Let's initialize Husky to our project:
```Bash
npx husky-init && npm install       
```
This command will freshly add Husky to our project in the `.husky` folder. Inside this folder, we can create files that match the git hooks we want to use. 

There are many Git hooks you can use to set up certain actions when using Git with your code. This tutorial will only deal with `pre-commit` Git hook provided to format and lint our code before it gets committed to our repository. Refer to [Husky's Documentation](https://typicode.github.io/husky/#/) for more examples of Git Hooks to use to improve your developer experience on your projects.

While we're here, let's install lint-staged:

```Bash
npm i --save-dev lint-staged
```

Modify the `pre-commit` Git Hook in `.husky`. This will run lint-staged before a commit can be pushed to our codebase.

```Shell
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

npx lint-staged
```

Add the lint-staged configuration to `package.json` so when certain files are staged for a commit, we run ESLint and Prettier.

```JSON:package.json
...,
"lint-staged":{
    "**/*.{js,jsx,ts,tsx}":[
      "npx prettier --write",
      "npx eslint --fix",
    ]
  }
```

### Testing Our Setup

Now that we've installed and configured our tools, let's test this out in action!

Let's find out what happens if we commit our code as-is:

```Bash
git add.
git commit -m 'test commit with husky'
```

Oops! Seems our commit failed. Luckily, the packages we use inform us what is causing the linting problem in `App.js`. Let's fix it and try again:

```Bash
git add.
git commit -m 'test commit with husky again :)'
```

Success! 🥳🥳 We have just committed clean code to our repository like a true pro 😎. 


## Further Resources

To further your own DX while you code, refer back to the documentation 
of these technologies to discover more ways to use git hooks.

- [Prettier Docs](https://prettier.io/)
- [ESLint Docs](https://eslint.org/)
- [Husky Docs](https://typicode.github.io/husky/#/)
- [Lint-Staged Docs](https://github.com/okonet/lint-staged)
- [Automating Code Checks and Tests with Git Hooks](https://www.antstack.io/blog/adding-git-hooks-to-your-project/)
