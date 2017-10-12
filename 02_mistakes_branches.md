<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Git Tutorial: Branches, Mistakes](#git-tutorial-branches-mistakes)
  - [Look at a repo via web UI](#look-at-a-repo-via-web-ui)
  - [Clone a repo](#clone-a-repo)
    - [Access different branches](#access-different-branches)
    - [Make your own branches](#make-your-own-branches)
    - [Additional info](#additional-info)
  - [Collaboration: Show your branch to team members](#collaboration-show-your-branch-to-team-members)
      - [Creating the actual pull request on the github website](#creating-the-actual-pull-request-on-the-github-website)
      - [Give feedback about a branch](#give-feedback-about-a-branch)
  - [Dealing with some common mistakes  (optional section)](#dealing-with-some-common-mistakes--optional-section)
    - [Oops, I added the wrong thing to the staging area](#oops-i-added-the-wrong-thing-to-the-staging-area)
    - [Argh, all of these changes are bad, can I undo everything?](#argh-all-of-these-changes-are-bad-can-i-undo-everything)
    - [Oh noooo I committed the wrong thing! My commit message is terrible!](#oh-noooo-i-committed-the-wrong-thing-my-commit-message-is-terrible)
    - [Darn, I accidentally committed my changes on `master` instead of a feature branch :(](#darn-i-accidentally-committed-my-changes-on-master-instead-of-a-feature-branch-)
    - [Additional info](#additional-info-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Git Tutorial: Branches, Mistakes

Summary: clone an existing repository, switch between branches, create your
own branches, practice undoing some git commands, push your branch, request review in a
web UI (pull request/merge request)

Commands shown: `git clone`, `git checkout`, `git branch`, `git reset`, `git show`, `git commit --amend`, `git fetch`, `git rm`, `git diff` of two branches.


## 1 Look at a repo via web UI

1.  We will be using a repo  called
    `git-playground`. As a placeholder, we'll write the URL
    as `some/url/to/git-playground`.  
2.  The actual URL you will use is https://github.com/DawsonCollegeCS-training/git-playground
	click on it to open.

In this repo the history looks like the diagram below, where every `o` represents a commit:

```
                          master
o--o--o ... o--o--o--o--o
                         \
                          o
                           another_branch
```


## 2 Clone a repo

You have been given access to a team repo as a collaborator.
To start working on the repo, you need a local copy for yourself.

1. In git-bash:

   ```{.bash}
	cd /h/
        git clone https://github.com/DawsonCollegeCS-training/git-playground
   ```

2.  This will create a `git-playground` directory with the
    repo contents.

    ```{.bash}
	    cd git-playground
    ```
    you will see something like

    ``` {.bash}
    username path/to/git-playground (master)  <-- the command prompt
    ```
    > In our instructions from now on we will represent the prompt
    > as `(branchname)$` so you always know which branch you should be on.

3.  When you clone, git automatically sets a remote called
    `origin` for you. It's the label for the URL of the
    repo you cloned from. Check what the URL is:

    ```
    (master)$ git remote -v
    origin	https://github.com/DawsonCollegeCS-training/git-playground (fetch)
    origin	https://github.com/DawsonCollegeCS-training/git-playground (push)
    ```

4.  Look at the list of commits on master (type `q` to quit)

    ``` {.bash}
    git log
    ``` 
5.  See information about the most
    recent commit on master.

    ``` {.bash}
     git show
    ```

### Access different branches

3.  First, check what files you have in the `src` dir of the repo.

    ```
    (master)$ ls src/
    geometry/ review/
    ```

1.  Let's checkout the branch we want to work on:
    
    ``` {.bash}
   
    (master)$ git checkout another_branch

    ```
    
2.  We have two local branches:

    ```{.bash}
    (another_branch)$ git branch
    * another_branch   			<-- the asterisk means we're on another_branch now
    master           
    ```

3.  Now your command prompt will end with `(another_branch)`.

    ```
    (another_branch)$ ls src/
    geometry/
    ```
    The `review` directory is gone! That's because the history recorded on `another_branch`
    includes a commit that deletes `review`.

     > __only one version of your files exists on disk at any time, that associated with the current branch__
    
### Make your own branches

1.  Create a branch called "fix_comments" that points to the same commit
    as master:

    ```{.bash}
    (somebranch)$ git checkout master            <- make sure we're on master
    (master)$ git branch fix_comments            <- create a mew branch
    (master)$ git log 				 <- check our branch
    commit 83bc44725ae6ec90211cf0c3 (HEAD -> master, origin/master, origin/HEAD, fix_comments)
    ...
    ```
    The most-recent commit, which is at the top of the log, is referenced
    by both `master` and `fix_comments`: they both point to the same commit!

    ```
    o--o--o ... o--o--o--o--o master, fix_comments
                             \
                              o another_branch

    ```

2.  Next, switch to your new branch so that you can work on it.  (Right now master & fix_comments files are the same)

    ```
    (master)$ git checkout fix_comments
    (fix_comments)$  				<- prompt will change

    ```

3.  Let's make a change on our new branch. Using any editor open `src/review/ArrayExamples.java` delete the line `//throw new ArrayOutOfBoundsException...`
    

4.  Use the `git diff` command to check that only the line is removed.

5.  `git add src/review/ArrayExamples.java` then
    `git commit -m "Remove commented-out code"`

6.   Now look at `git log` again: you should see that `fix_comments`,
     and `master` point to different
     commits in the log. Our history now looks like this:


     ```
                               o fix_comments
                              /
     o--o--o ... o--o--o--o--o master
                              \
                               o another_branch
     ```

## 3. Collaboration: Show your branch to team members

"Pull Requests" are used to ask a project owner or
teammate if they'd like to accept your changes by merging into the
master branch. In other words, it's a request for code review, discussion or feedback.

1. Let's create a new branch based on `master` and switch to it:

   ``` {.bash}
   $ git checkout master <-- if you're already on master this won't hurt
   (master)$ git branch featurex_yourname
   (master)$ git checkout featurex_yourname
   ```

2. Open `src/review/Person.java` in an editor, add the line `// I changed this file`

3. Open `src/geometry/PointUtils.java` in an editor, Fix the spelling error in the line `System.out.println("After srting");` -- should be "sorting"

4. Add and commit the two files.

5.  You can compare any two branches. Let's compare this branch to `master`:

    ```{.bash}
	git diff master featurex_yourname`
     ```

    This shows a summary of all changes on `featurex_yourname` compared
    to `master`

6.  Push your branch to origin (you can do this because we have given you permission):

     ```
     git push origin featurex_yourname
     ```

    This will create a new branch called `featurex_yourname` on the remote
    repo labelled `origin` and upload your commits to it from the current branch. 

10.  Now open the git-playground repo url https://github.com/DawsonCollegeCS-training/git-playground
      in your browser again, you may have to sign into your account.

#### Creating the actual pull request on the github website  

When you fill out the web form to create a request (see steps below), you will see
information about the changes you are submitting. 

*   Click on the "branches" link to see all the branches. Find your branch.
*   Click "New Pull Request" next to your branch. This loads a form.
*   In the Comment field, mention Maja's username (`@mfrydr`)
    * "Could you review this please `@mfrydr`?"
*   At the top of the form, make sure the two branches are 
    * base: `master`
    * compare: `featurex_yourname` --    
*   Click "Create Pull Request"

## 4 Demo PRs and feedback

We will now show some samples.
