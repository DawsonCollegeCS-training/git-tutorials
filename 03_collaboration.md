# Git Tutorial: Collaboration

Summary: keep your repository up-to-date, compare branches, combine branches, fix conflicts, delete branches, add subset of changes.

Commands shown: `git merge`, `git checkout`, `git add -p`, `git branch -d`

## Protected branches

Recall that a _branch_ in git is a pointer to a commit. A branch represents a
a different path in our history, a different line of development.

```                       
              o -- o master
             /
o--o--o--o--o
             \
              o featureA
```

There are many ways to organize and use branches in a repo. By convention,
the `master` branch usually represents the main line of development.
As another example, we could have several "special" branches in a project to represent different states of quality: `release`, `beta`, `experimental` are common names for such branches. The special branches are considered __protected__.

A __protected__ branch is only ever modified when the whole team agrees that specific
commits should be added. In some settings, only senior team members are allowed to push
to protected branches.

### Branches are used to manage teamwork

When many people are all pushing work to the
same remote repo, they all work on different, temporary branches. These temporary branches
are called "feature" branches because you typically create a new branch whenever you start
working on a new feature.

When the team agrees that a feature branch is good enough, they _merge_ that good branch into a protected branch like `master`, for example, to incorporate the feature into the software they want to release. A feature branch undergoes __code review__ by the team before it can
be approved and merged.

### Rules that enable productive collaboration

*   The team agrees in advance which branches are protected (one or more branches). Usually,
    `master` is protected.
*   The team agrees what conditions must be met to merge into a protected branch. Example:
    after a feature branch is reviewed and approved by two team members, the author
    may merge.
*   Otherwise, no one pushes any commits to the protected branches on the shared remote repo.
*   Once a commit is pushed on a protect branch on the remote, it cannot be modified or
    removed. That is, we agree not to rewrite history that has already been shared to the
    rest of the team.
*   Every commit on a protected branch should be a snapshot of working code: the code
    should run, and all tests must pass.

## Clone git-playground

We will practice collaboration with branches in a repo called `git-playground`.

1.  Create a directory for this tutorial: you can call it `tutorial3` for example.

2.  Ask your instructor for the URL of a repo called
    `git-playground`. As a placeholder, we'll write the URL
    as `some/url/to/git-playground`.

3.  Clone the git-playground repo in your `tutorial3` directory.

     ```
     cd tutorial3
     git clone some/url/to/git-playground
     ```

## Fetch a branch

Let's say that you are collaborating with a team on `git-playground`, and it's been
decided that `master` and `beta` are protected branches. When you clone you get
the master branch by default, but you may need to fetch beta the first time you use it.

Check which branches are available.

```
(master)$ git branch
* master
```

Try to checkout the beta branch.

```
(master)$ git checkout beta
Switched to a new branch 'beta'
Branch beta set up to track remote branch beta from origin.
```

If the above doesn't work and git says beta doesn't exist, you can download the beta branch from origin and call it beta locally as follows:

```
(master)$ git fetch origin beta:beta
```

If you do this right, `git branch` should now list both `master` and `beta`
and `git log beta` should show the following as the most-recent commit:

```
commit 361edbc9f3f69ef43da7c39dd1726da6bd1fbc80 (beta)
Author: Maja Frydrychowicz <mfrydrychowicz@dawsoncollege.qc.ca>
Date:   Mon Sep 11 21:49:02 2017 -0400

    Annotate Point.toString with Override
```


## Working on a feature branch

You've agreed that your team will synchronize their work on the beta branch.

1.  Now you want to start some work based on `beta`. The first thing to do is make sure
    `beta` has all the latest changes from your collaborators.

    ```
    (master)$ git checkout beta
    (beta)$ git pull origin beta
    ```

    In this case there are no new changes because we just fetched beta moments ago, but
    you will have to `pull` all protected branches from time to time to stay up to date.

2.  Now create your feature branch, based on beta:

    ```
    (beta)$ git checkout -b readmeUpdate
    ```

    > `git checkout -b branchname` creates a new branch and switches to that branch in one
    > step. It's the same as `git branch branchname; git checkout branchname`.

3.  Edit the README file so it looks as follows:

    ```
    # Git Playground

    ## Introduction

    This repo is intended for experimenting with git commands
    and concepts.

    ## Conventions

    In this repo, we agree to treat master and beta as protected branches.
    ```    

4.  Create a new file called `Authors.txt` with the following content:

    ```
    Sherlock Holmes
    Mycroft Holmes
    ```

### Staging a subset of changes

5.  Up to now, we've always committed entire files, but what if you only want to stage
    and commit a subset of your changes? Use `git add -p`! It shows you a diff and
    lets you choose which pieces (called _hunks_) to add.

    Let's try to only add the part of the README after the section `## Conventions`

    Enter `git add -p`. You'll see a diff, the question __"Stage this hunk?"__
    and a list of options at the bottom. You can always enter `?` to see a description of
    all the options. The
    main options to pay attention to are `y` (yes), `n` (no), `q` (quit), and `s` (split).

    The first hunk you are presented with shows all your README changes. Choose _s_ to
    split the hunk into smaller pieces.

    ```
    diff --git a/README.md b/README.md
    index 19bc775..29e3397 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,4 +1,10 @@
     # Git Playground

    +## Introduction
    +
     This repo is intended for experimenting with git commands
    -and concepts.
    +and concepts.
    +
    +## Conventions
    +
    +In this repo, we agree to treat master and beta as protected branches.
    Stage this hunk [y,n,q,a,d,/,s,e,?]?
    ```

    After pressing __s__, we get a smaller hunk, but it's not the part we want to
    stage, so answer __n__ for No.

    ```
    Split into 2 hunks.
    @@ -1,3 +1,5 @@
     # Git Playground

    +## Introduction
    +
     This repo is intended for experimenting with git commands
    Stage this hunk [y,n,q,a,d,/,j,J,g,e,?]?
    ```

    After pressing __n__, we get a second hunk, which shows only the second part of
    our README change (see below). This is the part we want, so press __y__ for yes:

    ```
    @@ -3,2 +5,6 @@
     This repo is intended for experimenting with git commands
    -and concepts.
    +and concepts.
    +
    +## Conventions
    +
    +In this repo, we agree to treat master and beta as protected branches.
    ```

    After this there are no more hunks in our tracked files (Authors.txt is not tracked),
    so the we return to the command prompt.

5.  We have staged part of README.md. `git status` should show the following:

    ```
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

            modified:   README.md

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   README.md    <----- there are other changes that we haven't staged!

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

            Authors.txt
    ```

    and `git diff --staged` will show only the Conventions section:

    ```
    diff --git a/README.md b/README.md
    index 19bc775..528a696 100644
    --- a/README.md
    +++ b/README.md
    @@ -1,4 +1,8 @@
     # Git Playground

     This repo is intended for experimenting with git commands
    -and concepts.
    +and concepts.
    +
    +## Conventions
    +
    +In this repo, we agree to treat master and beta as protected branches.
    ```

6.  Commit your first README change, that you just staged:

    ```
    git commit -m "List protected branches in README"
    ```

7.  Add and commit your other changes:

    ```
    (readmeUpdate)$ git add Authors.txt
    (readmeUpdate)$ git commit -m "Add list of authors"
    (readmeUpdate)$ git add README.md
    (readmeUpdate)$ git commit -m "Add introduction heading to README"
    ```

## Requesting to have your feature branch merged (i.e. request code review)

Now to merge your `readmeUpdate` branch into `beta`, you would normally push
your `readmeUpdate` to `origin` and use the gitblarg
web UI to open a pull request/merge request and get code review, which is what we practice in our previous git tutorial. If you want to practice this again, try these steps:

1.  (Optional) Push your branch to origin.

    ```
    # make sure you're on readmeUpdate branch
    git checkout readmeUpdate
    # publish your readmeUpdate branch to a new branch
    # called readmeUpdate_yourname on the remote called origin
    git push origin readmeUpdate:readmeUpdate_yourname <--- adding yourname to avoid name clash
    ```

    You normally don't have to add your name to your branch when you push, we're just
    doing so in this tutorial because many students are completing this exercise at the
    same time in the same git-playground repo.

2.  (Optional) In the web UI, find your branch and click the button the creates/opens a new
    pull request (or merge request, whatever the web UI calls it.)

3.  (Optional) This time make sure the request is about the `beta` branch compared to the
    `readmeUpdate_yourname` branch. __Notice that there's a green checkmark__ that says
    this branch can be merged automatically.

__The direction of the merge is important.__ When you merge `readmeUpdate_yourname` into `beta`,
`beta` receives the commits `readmeUpdate_yourname`. We say that `beta` is the "base" branch.

```
readmeUpdate_yourname ----merge----> beta (base)
```

## Merging a feature branch

What does merging do? It reunites two diverging branches into one. In this case, we
are incorporating a changes from a feature branch (readmeUpdate) into a protected branch (beta):

```
          o--o--o  readmeUpdate
         /        \
--o--o--o          o <--- merge commit of readmeUpdate and beta
         \        /
          o beta
```

What would happen if someone pressed the button in the web UI to accept your request? It
amounts to running the `git merge` command on the remote repository.


### Manually merge readmeUpdate into beta

__Normally you'd merge branches by using the web UI__, but in this exercise we will
perform a merge at the command-line so you can see how it works.

1.  Check out the branch we want to merge __into__, which is `beta`. This is called the `base`
    branch or the `target` branch. It is going to receive new commits from `readmeUpdate`.

    ```
    git checkout beta
    ```

2.  Merge the `readmeUpdate` branch: an editor will open to edit a message for the "merge"
    commit, which combines the two branches. Save the message as is.

    ```
    git merge readmeUpdate
    ```

3. If you look at `git log` now, you should see that the history of `beta` shows the
   merge commit as well as the commits from `readmeUpdate`.

Again, the direction of the merge is important. We merged readmeUpdate into beta.
Merging beta into readmeUpdate is __not__ the same thing.

4. Now that the `readmeUpdate` branch has been merged, you can delete it.

  ```
  git branch -d readmeUpdate
  ```

> Normally, you would not perform this `git merge` process at the command-line in
your local repo like we just did. Instead,
your collaborator would accept your pull/merge request in the web UI, thus merging
`readmeUpdate` into `beta` in the remote. Then you (and all your teammates) would
`git pull origin beta` to download the new commits added to `beta` as a result of
the merge.

### What if the merge doesn't work?

In the previous example, git was able to perform the merge by itself, but sometimes it
needs help from you.

Let's look at a different branch that __can't be merged automatically
__. We've created a __conflicting__ branch for you to experiment with. It's called
`removeOverride`. The relationship with `beta` looks like this.

```
          o removeOverride
         /
--o--o--o
         \
          o beta
```

As you'll see in a moment, the `removeOverride` branch deletes the `Point.toString` method,
whereas as the last commit on the `beta` branch modifies the `Point.toString` method.
Merging these two histories will resolve in __conflict__, which just means that git doesn't
know how to combine the changes so it asks you for help.

First let's download the `removeOverride` branch.

1. `git checkout removeOverride` or if that doesn't work, try `git fetch origin removeOverride:removeOverride`
2.  Look at the last commit of `removeOverride`: `git show removeOverride`

Now let's try to merge into beta.

```
$ git checkout beta
(beta)$ git merge removeOverride
```

You get the following message:

```
Auto-merging src/geometry/Point.java
CONFLICT (content): Merge conflict in src/geometry/Point.java
Automatic merge failed; fix conflicts and then commit the result.
```

And your command prompt now ends with `(beta|MERGING)` to indicate that git is in the middle
of merging branches.

> If someone opened a pull/merge request of removeOverride against the beta branch
in the web UI, the request would show an error icon and a message about merge conflicts,
or not being able to automatically merge. In that case, the author has to fix the problem locally
and push again.

### Dealing with merge conflicts

Okay, so how to do you fix this?

First of all, if you don't want to merge at all, you can use the command `git merge --abort`
to exit the merge process to get back to the previous state.

If you want to fix conflicts, you have to:
* edit the conflicted file(s)
* add it to the staging area to mark the conflict as "resolved"
* continue the merge

1. Open `src/geometry/Point.java`; at the bottom you should see something like this:

```
<<<<<<< HEAD
        @Override
        public String toString() {
                return "[" + x + "," + y + "]";
        }

=======
>>>>>>> removeOverride
```

The `<<<` `===` and `>>>` are __conflict markers__. The way to read this is as follows:

```
<<<<<<< HEAD
This is the state of these lines at the current commit (HEAD, which beta points to now)
=======
This is the state of the lines at the last commit of the removeOverride branch
>>>>>>> removeOverride
```

So the conflict markers are telling us that on `beta` there a `toString` method, and
on `removeOverride` there's nothing. Now all we need to do is edit the file to remove
the conflict markers and change the contents to whatever we want.

2.  Let's say we want to final version of Point.java to have no `toString` method. In that
    case, edit `Point.java` to remove all the conflict markers and everything in between.

3.  After saving and closing `Point.java`, mark the conflict as resolved by adding it to
    the staging area:

    ```
    (beta|MERGING)$ git add src/geometry/Point.java
    (beta|MERGING)$ git diff --staged
    diff --git a/src/geometry/Point.java b/src/geometry/Point.java
    index 2b2e26e..02b23da 100644
    --- a/src/geometry/Point.java
    +++ b/src/geometry/Point.java
    @@ -22,10 +22,4 @@ public class Point {
            public void setY(double y) {
                    this.y = y;
            }
    -
    -       @Override
    -       public String toString() {
    -               return "[" + x + "," + y + "]";
    -       }
    -
     }
    ```

4.  All the conflicts are marked as resolved, so the next step is to continue the merge.

    ```
    (beta|MERGING)$ git merge --continue
    ```

    An editor will open to edit the "merge commit" message: leave the message as-is.

5.  The merge is done, now `git log` will show that `beta` includes a commit from
    `removeOverride`.


The next tutorial will show another way to deal with merge conflicts, using `git rebase`.
