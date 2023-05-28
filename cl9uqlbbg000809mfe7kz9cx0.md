---
title: "Learn to Fix Common Git Mistakes"
datePublished: Sun Oct 30 2022 02:32:16 GMT+0000 (Coordinated Universal Time)
cuid: cl9uqlbbg000809mfe7kz9cx0
slug: learn-to-fix-common-git-mistakes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1667096483317/eqh0rD6zf.png
tags: git, beginners

---

This **workshop ** will explain the* common mistakes* many people make when utilizing
Git. It will begin by breaking down states in which a file can exist and build
on it from there. When you complete this workshop, you can expect to never have
to worry about losing data or messing up a repo again.

##  Change a Commit Message that Hasn't Been Pushed Yet

To start a new Git project, go to `New` on Github and enter a repository name. After creating the repository, find the clone link for the repo and `git clone <repo-link>`. You can then `cd` into that folder and `touch index.html` to create your first file. Inside your `index.html` file you can insert code and add the file to your commit.

### index.html
```html
<html>
  <head>
  </head>
  <body>

    <h1>Fixing git mistakes</h1>

  </body>
</html>  
```

You can do a `git status` and see that the `index.html` is an untracked file. Once you `git add index.html`, it stages the file and prepares it for a commit. You can commit that with a message like `git commit -m "Adding index.html to git-mistokes"`.

After typing the commit message, we see we typed it incorrectly and want to change it. Since we haven't pushed it yet, you can use `git commit --amend -m "Adding index.html to git-mistakes"` to change the message. Now, if we `git log --oneline`, we can see the initial commit Github made to the project along with our rewritten message `Adding index.html to git-mistakes`.



##  Add More Files and Changes to a Commit Before Pushing

We can use `git log --oneline` to see information about our previous commits like what files were added to certain commits. There is something we can do if we want to add more than one file to the same commit. We can `touch app.js` and edit it.

### app.js
```js
// our app js code
```

To see that, we need to add it in the `<head>` of the `index.html` in our previous commit. We type `<script type="text/javascript" src="/app.js"></script>`.

### index.html
```html
<html>
  <head>
    <script type="text/javascript" src="/app.js"></script>
  </head>
  <body>

    <h1>Fixing git mistakes</h1>

  </body>
</html>  
```

Now that we have our `script` tag added to our `index.html`, when we do `git status` we have one untracked file and one modified file. However, we want to add those to the same commit. To do this, we add both to the stage with `git add -A` and we have both as a change to be committed. We then `git commit --amend -m "Adding index.html and app.js"` to add them to the same previous commit. Now, with `git status`, we have no changes to be committed. A `git log --oneline`, all files were added to the same commit along with a rewritten commit message.


##  Remove Files from Staging Before Committing

We can add another Javascript file to our project with `touch lib.js`. After doing so, we can `git status` to confirm it is untracked. Then, a `git add lib.js` will stage it for a commit. However, we might realize after adding it that we may not want to commit that file at all.

Although Git tells us how to do it in the terminal (`git reset HEAD <file>...`), we want to figure out what the `HEAD` means. With `git log --oneline` we find the `HEAD` is just a pointer to a branch and that branch is just a pointer to the commit specified by a hash on the left.

After running `git reset HEAD lib.js`, we can check if we successfully removed it from being staged for a commit with `git status.



Inside of the `app.js` created earlier, we can create a function `helloWorld` that contains `alert("hello!")`. Later, you might close that file and open the `index.html` and make changes to that file like adding a description.

### app.js
```js
// our app js code

function helloWorld() {
  alert("hello!")
}
```

### index.html
```html
<html>
  <head>
    <script type="text/javascript" src="/app.js"></script>
  </head>
  <body>

    <h1>Fixing git mistakes</h1>
    <p>Here's how to fix some common git mistakes</p>
  </body>
</html>  
```

You may be tempted to use `git add -A` alongside a simple commit message after that, but it will be clear that you are ahead by 2 commits. If you're able to see this before committing, you want to see what you're about to push. To check, run `git diff origin/master HEAD` to see the changes to all files edited.

If you realize you want to get rid of something before you push, check the log with `git log --oneline` to see which commit you'd like to undo. You have to decide where you want to reset to, so after finding the desired commit use `git reset {LOCATION}`. The location can be a hash associated with a commit or something like `HEAD~1` which takes you to HEAD, then back down the tree 1 time.

After performing a `git reset`, your files should return to the unstaged location. **It's important to be careful to not use `git reset` after pushing a commit because it'll change the history other people may have already downloaded.** So, only use reset on branches that aren't pushed yet.

Now, you can `git add index.html` and `git commit -m "Adds desc for index"` to fix what went wrong earlier. Using `git status` will us we have the `app.js` not staged for commit and that we are two commits ahead of origin/master. You can use `git log --oneline` to verify the old commit was replaced with the new one.

 


## Use and Compare the Different git Reset Options: --hard, --soft, and --mixed

If our `app.js` is modified and we run the commands

```
git add app.js
git commit -m "app js changes"
git log --oneline
```

it will commit the file locally and we can see the commit in your tree.

**Running `git --help reset` will show the different reset options.**

The common flags you will see are `--soft`, `--hard`, and `--mixed`. If you try `git reset --soft HEAD~1` to reset back one from the HEAD, it will take the previously committed `app.js` and move it back to the staging area.

We can redo this with `git commit -m "take 2"` and try a reset with `git reset --mixed HEAD~1`, the `app.js` removes the commit as well as unstages the changes.

Trying another reset, we can run `git add app.js` and `git commit -m "take 3"` to prepare our file. We can enter `git reset --hard HEAD~1`, but see that it actually causes us to lose our work. It gets rid of the commit, unstages the changes, and removes them from our directory. **Because we lose our work doing so, you don't usually want to do a `git reset --hard`.**




## Recover Local Changes from `git reset --hard` with `git reflog`

After doing a `git reset --hard HEAD~1`, it removed the code in our `app.js`, but we want to get it back. We can look at `git reflog`, something used to look at all the different things you have done in your local git repository. You can see the latest resets you've done with that command, note the hashes which you'll be using later.

One thing to note is that we are trying to recover a hard reset, so the commit is considered abandoned and will get garbage collected eventually if we don't save it. **`git reflog` will work to save commits, but only if they haven't been garbage collected by git yet.**

Once you find the commit you need to return to to recover your code, use that hash and run `git reset --hard {HASH}`. It will then return your code back to its state. Using `git log --oneline` shows the regular commits, as well the old commit. After successfully recovering, remember to `git push` to store in Github to view.

##  Undo a Commit that has Already Been Pushed

If we just made a push to Github, we can use `git log --oneline` to see our local branch is up to date with our origin. We are presented with a problem when we try to undo a commit that's already been pushed. **We have to be careful because if we undo a commit already pushed, we would affect those who pulled the changes, effectively rewriting history.**

We are going to use `git revert {HASH}` with the hash being the commit we want to revert. After doing so, it'll want you to make a revert commit. It's similar to a merge commit because it adds another commit to the tree, plus we have to give it a message. After entering the message, running `git log --oneline` we find that it successfully reverted the last commit.

**The important thing is that when using `git revert`, the history is still there.** You can go back to an earlier commit to revive previous work/code. In addition, anyone who pulled the origin/master branch during the time it took to revert will have a clean history tree because we used `git revert {HASH}` instead of `git reset --hard {HASH}`.



## Push a New Branch to github that Doesn't Exist Remotely Yet

We can create branches with `git branch {BRANCH-NAME}` or `git checkout -b {BRANCH-NAME}`. Let's create one called `js-changes` with `git checkout -b js-changes`. Using `git status` shows you which branch you're currently on and `git branch -vv` shows the current commit and remote you're on for each branch. We can make a change on the branch by creating a function in our `app.js`.

### app.js
```js
// our app js code

function helloWorld() {
  alert("Hi!")
}
```

After saving, we stage the file with `git add app.js` and a commit message `git commit -m "Adds hello world"`. Using the `git log --oneline` shows that we have diverged from the master branch. If we tried to push at this point, we would receive a fatal error. Using `git branch -vv` displays we don't have `js-changes` linked to any remote branch. However, Git gives us the fix in the terminal. We have to push while setting the upstream to `origin js-changes`. **We then run `git push -u origin js-changes` to push it, as `-u` is just an alternative to `--set-upstream`.** Now, we successfully pushed our new branch to Github and trying `git branch -vv` can show us that the old `js-changes` is now mapped to `origin/js-changes`.


## Copy a Commit from One Branch to Another

If we do `git branch`, we notice we have two branches. In addition, `git log --oneline` shows us our latest branch has our commit diverging from `master`, the reversion of the `take 3` commit. We can switch over to `master` with `git checkout master` leaving our `app.js` empty.

A problem we could face is that we want the function we created on our separate branch `js-changes` transferred/copied to `master` without pulling the entire branch. We simply want to pick the individual commit to `js-changes` and move it to `master`.

`git log --oneline` shows us our current latest commit in `master` isn't up to date with the separate branch. **What we can do to match the commit in our branches is use cherry picking. We run `git cherry-pick {HASH}` with the hash being the desired commit.** Now, our `git log --oneline` actually shows there was a new commit hash created as it came over. **What happened is we copied the commit from `js-changes` over to `master` and pushed it up, so the commit exists in both trees.**




## Move a Commit that was Committed on the Wrong Branch

Using `git branch` shows us we have two branches, `js-changes` and `master`, and we are on `master`. We can write a second function in our `app.js`.

### app.js
```js
// our app js code

function helloWorld() {
  alert("Hi!")
}

function secondFunction() {
  alert("This is number 2")
}
```

After modifying, we add and commit it.

### Terminal Input
```
git add -A
git commit -m "My second function"
```

However, after doing so, doing `git branch` shows us we made this commit on `master` branch and we meant to do it on the `js-changes` branch. `git log --oneline` shows us which commit we need to move from `master` to `js-changes`, but the problem is master is pointing to it. **A lot of solutions to this involve `git reset --hard`, but beware because they can cause a loss of data.**

What we want to do is switch to the `js-changes` branch with `git checkout js-changes`. Now, to get the function into it, we copy the commit hash and cherry-pick that commit hash we want to move over with `git cherry-pick {HASH}`. Now, on the `js-changes` branch, we have the code we want and can push that with `git push`.

At this moment, our code on `js-changes` is correct, but `master` still contains that old commit, as **cherry-picking copies the commit but doesn't remove it**. To switch `master` to be back at a previous commit, we do a `git reset {HASH}` to the commit we want to return to. An alternative to returning the hash would be making the it `HEAD~1`.

Now, `git status` shows `app.js` is still a modified file. **However, after resetting we can `git checkout app.js` to reset it whatever it was at the hash we reset to.** After entering that, `git status` shows `app.js` is not changed and `git log --oneline` shows the commit is gone. This approach is like manually doing a reset hard, but we can choose exactly what files we want to reset making it a safe and powerful option.


##  Use `git stash` to Save Local Changes While Pulling

Save the local changes,
```
git stash
```

Get remote changes
```
git pull
```

To apply the stashed changed
```
git stash pop
```

You will need to fix the merge conflict.

Then drop the change from the stash
```
git stash drop stash@{0}
```


##  Explore Old Commits with a Detached HEAD, and then Recover

checkout the hash of an old commit
```
git checkout [HASH]
```

we'll be in a "detached HEAD" state
Save the work by creating a new branch
```
git checkout -b my-new-branch
```

## Fix a Pull Request that has a Merge Conflict
```
git checkout -b conflicts_branch
```

Add 'Line4' and 'Line5'
```
git commit -am "add line4 and line5"
git push origin conflicts_branch
git checkout master
```

Add 'Line6' and 'Line7'`
```
git commit -am "add line6 and line7"
git push origin master
```

##  Cleanup and Delete Branches After a Pull Request

use the github interface to delete the branch remotely

Locally

Confirm that remote is gone
```
git remote prune origin --dry-run
git remote prune origin
```

clean up the feature branch
```
git branch -d feature-branch
```

## Change the Commit Message of a Previous Commit with Interactive Rebase
```
git log --oneline
```

start the interactive rebase
```
git rebase -i HEAD~3
```
and then change pick to reword.

We can now reword the commit message
## git Ignore a File that has Already been Committed and Pushed

We make a file and accidentally push it to github
To remove it, add it to .gitignore file
remove all of our files from our git cache
```
git rm -r --cached .
```

add back all the files we want with
```
git add -A
```
## Add a File to a Previous Commit with Interactive Rebase
```
git rebase -i HEAD~2
```

during the interactive rebase, we can add the file, and amend the commi
```
git commit --amend --no-edit

git rebase --continue
```

##  Fix Merge Conflicts While Changing Commits During an Interactive Rebase

enter interactive rebase
```
git rebase -i HEAD~2
```

Then we can fix that merge conflict like normal, but finish up the rebase
```
git rebase --continue
```

##  Squash Commits Before they are Pushed with Interactive Rebase
```
git rebase -i HEAD~3
```
Make the changes in interactive rebase, make the commit message for that commit, and once we save the message, we'll be left with just a single commit

##  Completely Remove a File from Pushed git History

Prune the entire history and garbage collect the remains
```
git reflog expire --expire=now --all && git gc --prune=now --aggressive
```
use git push to push that change to github, and remove the .env file from all of the history

## Resources

[When to Use Git Reset, Git Revert & Git Checkout](https://dev.to/neshaz/when-to-use-git-reset-git-revert--git-checkout-18je)
