---
track: "React Fundamentals"
title: "Group Git Exercise"
week: 19
day: 1
type: "lecture"
---

![github](https://i.imgur.com/uWteCty.gif)

# Git Collaboration 


### Objectives
*After this lesson students will be able to*
- Describe the different steps of the git workflow process
- Explain basic git commands in terms of this model, e.g., commit, add, log
- Safely work on a feature branch and merge it back to the main branch
- Be aware of 2 pitfalls when working with git in a Group and how to resolve/avoid them.

<br><br>

### The Git Workflow

![Basic Commands](https://i.imgur.com/blN6xuD.png)

Layer| Description
---- | ----
Working Directory | Local file system (your computer's files like normal)
Staging Area | Changes that have been `add`ed and are ready to commit
History | Changes that have been committed in a series of commits uniquely identified by a `SHA1` hash
Remote | An associated version of the repository on a remote host accessible via networking

<br><br>

### Git Command Review

Now let's go over some `git` commands used in the workflow.  Each command will typically either be used to inspect the changes at a particular layer(s) or it will transition a set of changes from one layer to another.

Command | Function | Data Layer(s)
----- | ----- | -----
add | move changes | WD -> Staging
commit | move changes | Staging -> History
status | inspect changes | WD/Staging/History (via what commit is the last one)
branch | inspect | WD/History (via what branch is current)
log | inspect | History
push | move changes/sync | Local History -> Remote History
pull | move changes/sync | Remote History -> Local History/WD/Current Branch
checkout | move index | WD (moves WD reference to a different HEAD of History)
merge | move changes | applies new changes from one branch to the HEAD of the current branch

<br><br>

### Feature Branching + Merging

Conceptually what branching looks like:
![Git Branch
Diagram](https://wac-cdn.atlassian.com/dam/jcr:389059a7-214c-46a3-bc52-7781b4730301/hero.svg)

<br><br>

## Activity 

**Groups of 3 or 4 Students**

1. Choose 1 person to be the **Repo Owner** & Primary Contributor. 
  - This person will be in charge of maintaining the ingreity of your main branch and making sure no broken code gets added to your "production branch"

2. Choose 1 person to be the **Project Manager / Primary Organizer.** 
  - This person will be in charge of maintaining the Kanban Board (Trello, Miro, Figma, Etc...), making sure tasks are assigned and moved when completed

3. Choose 1 person to be the Primary **Frontend Lead**
  - This person will have the final design say on all front end features and scope 

4. Choose 1 person to be the Primary **Backend Lead**
  - This person will have the final say on all back end features and scope


### All members of the group will contribute equally to all parts of the code - frontend, backend, planning tools, github etc...
* These roles help keep things organized and provideds a tie breaker for project decisions when needed. They are not meant to dictate what part of the code each member works on. **i.e. The Frontend Lead works on all parts of the code not just "frontend".**
* We will be looking at git history for **equal contributions**. The best way to play the game is to get as many commits and pull requests in as you can.



## Repo Owner

1. Create a new repository called `demo-project` in github - make sure it is public and select the option to add a readme.md

2. Post a thread in slack asking your group members to share their github usernames

3. Add your group mates as "collaborators" in github. Go to demo-project > Settings > Collaborators. Search for their username and add them.

4. Clone down your new repo and add a new file called **index.js**

5. Add a new line at the top of **index.js** on line 1. 

  ```js
  let myVar = "a"
  ```

6. Stage > Commit > Push

```ss
git add . 
git commit -m "init commit"
git push origin main
```

7. Post a link to your repo in your group slack thread

<br><br>

## Group Members # 2, 3, 4


1. Navigate to the repo on github

2. Clone down a copy of the repository to your local machine

3. Check out your own "feature branch" to add some feature code.
  - Feature code refers to a small amount of code in one or two files that accomplishes one purpose. (Think adding a new route or the css for a button component)
  - Feature code is not small changes all over the place in multiple files or editing the formatting of another developers code. Think small and centralized. This will minimize merge conflicts. 

```ss
git checkout -b "<feature-branch-name>"
```

4. Open **index.js** and add an addition to the code on line 1. 

```js
let myVar = "a + b"
```

5. Stage > Commit > Push

```ss
git add . 
git commit -m "my first contribution"
git push origin <feature branch name>
```

6. Checkout the repo on github and see your feature branch added to the list of branches.



**Hit Pause!**

<br><br>

## Repo Owner

1. While still on the `main` brach open **index.js** and make a change to the code on line 1. 

```js
let myVar = "a + c"
```

2. Stage > Commit > Push

```ss
git add . 
git commit -m "first update"
git push origin main
```

<br><br>

## Group Members # 2, 3, 4

1. Navigate to the repo on github and find your feature branch name

  - Verify that your newest code is there. 

2. Select the "Pull requests" tab

![create pull request tab](https://i.imgur.com/F9nijj0.png)

3. Follow the prompts in github to create a pull request to merge your feature branch into main. 

![create pull request button](https://i.imgur.com/ejuqjn4.png)

<br><br>

### You will get warnings that your branch cant automatically merge. Rats!


You and someone else (the repo owner or a group mate) checked out the code base at the same starting point and made changes to the same line of code. It happens ALL the time. This results in a `merge conflict` for whoever doesn't merge their code in first. How do you resolve this? It's not too bad, just take it one step at a time. 


<br><br>

### Uh Oh! What happened?? Here is a recap

![merge conflict](https://i.imgur.com/vDAHieQ.png)

<br><br>


## Option 1

1. **Option One:** Fix the conflict in github and merge your code. You will be taken to a screen where you can decide which code to keep and which to delete. It will also let you commit the changes you make right there in Github. Just make sure everyone pulls down these changes after you make them. 

**We will not be using this option for this demo.**


![merge notes](https://i.imgur.com/miABHQC.png)


2. After you merge your branch you can delete it remotely and locally. This is completely optional, but you will start to see these branches stack up when you use the commands:

```sh

git branch -v

or 

git remote -v
```


To delete locally and remove the github remote link: 

```sh
git branch -d <feature-branch> 

and 

git remote remove <feature-branch>
```

3. Make sure to pull down the latest version of main and stop working on your now merged feature branch. 


```sh 
git checkout main
git pull
git checkout -b <next-feature-branch>
```


<br>


## Option 2


1. **Option Two:** pull down the most recent changes to main and merge the two branches locally. 
 - VS code will show you the conflicts and ask you how you would like to resolve them. 


```js

<<<<<<< main

let myVar = "a + b"

=========

let myVar = "a + c" // <- your code that needs to be adapted

>>>>>>> feature-branch

```

Let's do it together!


## Group Members # 2, 3, 4

1. Let's try option 2:  Checkout main, pull the latest changes and switch back to your feature branch.

```ss
git checkout main
git pull
git checkout <feature-branch>
git merge main
```

2. Fix your conflicts in vs code.


3. Commit and push merge resolution
```ss
git commit -m "fixes merge conflict"
git push origin feature-branch
```

4. Attempt to create a new pull request in github but dont merge.... No errors detected!

**DONT MERGE- This is just a demonstration.**

Because your other group mates are at the same point, the second one of you completes their PR, the others will get errors again. They will need to pull those changes and merge them into thier feature branches again. If you want to see this full process, take turns merging and pulling each others new changes. 



### Anytime a pull request is merged into main ALL GROUP MEMBBERS will need to pull the latest changes to main.

**Merge the changes into the branch you are working on so that you have the latest changes.**

```ss
git checkout main
git pull
git checkout <other-feature-branch>
git merge main
```

<br><br>

✅ **Practice this with your group a few times until everyone has had the chance to resolve a merge conflict.**


## Using a "DEV" Branch 

It is highly advised to use a "DEV" branch when you are working on your project for unit 3. Here is an example of that git flow. 

![dev branch flow](https://i.imgur.com/JImDSLZ.png)

All group members including the repo owner check out the dev branch and treat it as "main". 
This keeps main "pure" and conflict free, as all merge conflicts will be resolved while merging things into the dev branch. The repo owner will periodically merge dev into main. We sometimes call this a "production branch."

<br><br>


## Extra Resources

### Rebasing branches

If time permits the instructor willl demo how to rebase branches.


![](https://i.imgur.com/PRPhtu6.png)

[`git rebase`](https://git-scm.com/docs/git-rebase)

Rebasing rewrites history.  This adds the commits from another branch and puts your commits on top of your branch.  (Actually it puts _new copies_ of your commits on top). Typically, we rebase `main` from another branch.  This does not add an extra merge-commit.

**Ex**: From some branch: `git rebase main` will take anything that was added to main since branched off (or last rebased) and put those commits _before yours_.  Your commits are then added on top of your branch.

Technically, `git pull` is a shorthand for `git fetch origin HEAD` together with `git merge origin/HEAD`.  In other words, `pull` is an alias for fetching the changes from origin and merging them into the local copy of the branch.  adding the `--rebase` flag to `pull` will rebase rather than merge, thereby not adding a merge commit to your history but carrying with it additional pain when conflicts emerge.

<br><br>

### Resets
`git reset` can be used to undo a committed history and place the changes back either into the staging area `--soft` or in the working directory `--mixed` or discard them entirely `--hard`.  Be very careful with `git reset` especially with the `--hard` option since this is potentially destructive.

If you undo a public history you will have to `git push --force` after making the changes.

[How to Reset (almost) anything](https://github.com/blog/2019-how-to-undo-almost-anything-with-git)

[Reset, Checkout, Revert](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)

[**Oh boy I really messed up:** ](https://ohshitgit.com/)

### Extra Resources
- [An Incredible Git Tutorial](http://gitimmersion.com/) probably the second most helpful git thing I've ever come across . . .by our friend `Jim Weirich`

- [a nice set of cheat sheets](https://www.atlassian.com/git/tutorials/atlassian-git-cheatsheet) from Atlassian

- [A more in depth and practical look at git rebase](https://dev.to/maxwell_dev/the-git-rebase-introduction-i-wish-id-had) Helpful to strengthen your rebase sorcery

- [Linus Torvalds nerding out about git](https://www.youtube.com/watch?v=4XpnKHJAok8) Buckle up

- [Obligatory Junio Hamano interview](https://www.youtube.com/watch?v=qs_xS1Y6nGc)