# Git Tutorial: Mistakes, Branches

Summary: clone an existing repository, switch between branches, create your
own branches, practice undoing some git commands, push your branch, request review in a
web UI (pull request/merge request)

Commands shown: `git clone`, `git checkout`, `git branch`, `git reset`, `git show`, `git commit --amend`, `git fetch`, `git rm`, `git diff` of two branches.


## Look at a repo web UI

1.  Ask your instructor for the URL of a repo called
    `git-playground`. As a placeholder, we'll write the URL
    as `some/url/to/git-playground`.

2.  Open the URL in your favourite browser: it shows an
    overview of the repo and its files.

3.  Click on the link to "commits" (likely on the top left)
    to view the list of commits in reverse chronological order.

4.  While looking at the list of commits, notice that
    there's a dropdown at the top that shows `master` and ` another_branch`. Compare the list of commits for `master` and `another_branch`: there's a difference.

5.  Go back to the main URL for the repo where you see an
    overview of its files. You should see a similar dropdown
    in this view, too: switch between `master` and `another_branch` to see how the files differ.

So this repo has two _branches_: `master` and `another_branch`.

>A __branch__ is a pointer
>to a commit; it represents a point in history that may progress in another direction. We
>use branches to pursue different lines of work in a project at the same time.

In this repo the history looks like the diagram below, where every `o` represents a commit:

```
                          master
o--o--o ... o--o--o--o--o
                         \
                          o
                           another_branch
```

The two branches share all the same commits, except one: `another_branch` has one extra
commit that `master` doesn't have.

### Why branches?

`master` is the default branch in git, and it's usually treated as the "production" version of your code -- what you would actually release to users. We create additional, temporary branches in our repos to work on different features or just try something out. That way, `master` always stays in good shape.

So your local history could look like this:

```
      welcome_animation
     o
    /
o--o--o--o--o master
       \
        o--o--o admin_panel
```

We call these temporary branches __feature branches__: `welcome_animation` and
`admin_panel` are feature branches. When many developers contribute to the same repo, they
all work on one or more feature branches, then push their branches to the same remote repo
on a server. Each branch is reviewed, and if it's approved, it can be "merged" into master.
We'll learn more about merging in the next lab.

## Clone a repo

In this exercise, we're simulating the scenario that
you have been given access to a team repo as a collaborator.
To start working on the repo, you need a local copy for yourself.

1.  In your h drive: `git clone some/url/to/git-playground`
    This will create a `git-playground` directory with the
    repo contents.

2. `cd git-playground`.

    Depending on the configuration of your shell,
    the command prompt __might__ show that your current branch is `master`;
    it might look something like this:

    ```
    username path/to/git-playground (master)  <-- the command prompt
    ```

    If your prompt doesn't show the branch name, that's okay.

> In our instructions from now on we will represent the prompt
> as `(branchname)$` so you always know which branch you should be on.


3.  When you clone, git automatically sets a remote called
    `origin` for you. It's the label for the URL of the
    repo you cloned from. Check what the URL is:

    ```
    (master)$ git remote -v
    origin some/url/git-playground.git (fetch)
    origin some/url/git-playground.git (push)
    ```

4.  Use `git log` to look at the list of commits on
    master. Should be the same as what you saw on the web.
    To exit the log, press `q`.

5.  Use `git show` to see information about the most
    recent commit on master. This shows a diff of the
    files as well. _Which file was changed? Was code
    added or removed?_

## Access different branches

6.  Use `git branch` to see all the branches you have
    locally.

    ```
    (master)$ git branch
    * master      <--- the asterisk means that's your current branch
    ```

    It's only showing `master`! Where's `another_branch`?
    You don't have it yet!

> When you clone a repo, you don't receive all the remote branches by default, just master.

7.  Let's "fetch" the missing branch:

    ```
    (master)$ git fetch origin another_branch:another_branch
    From https://github.com/317-2017-02/git-playground
    * [new branch]      another_branch -> another_branch
    ```

    This command means: download, from the remote called `origin`, the history of
    the remote branch called `another_branch` and
    put in a local branch also called `another_branch`.

    It created a new branch for you, `another_branch`, and this new branch has all the
    history from `origin/another_branch`.

8.  Now we should have two local branches:

    ```
    (master)$ git branch
    another_branch
    * master           <-- the asterisk means we're on master now
    ```

Next we'll use the `checkout` command to switch to a different branch.

> The `checkout` command makes the content of the repo working directory match
the state of tracked files recorded at that commit.

9.  First, check what files you have in the `src` dir of the repo.

    ```
    (master)$ ls src/
    geometry/ review/
    ```

10. Switch branches to `another_branch` and check the
    `src` dir again.

    ```
    (master)$ git checkout another_branch
    Switched to branch 'another_branch'
    ```

    Now your command prompt will end with `(another_branch)`.

    ```
    (another_branch)$ ls src/
    geometry/
    ```

    The `review` directory is gone! That's because the history recorded on `another_branch`
    includes a commit that deletes `review`.

> __When you use `git checkout`, the files you have on disk get modified
because you are moving to a different snapshot of history.__

So after you checkout `another_branch`, the `review` directory and its
files no longer exist on your computer's storage. But when you switch back
to `master` the state of all the files will be reconstructed from history and
they will reappear again.

For example, let's say you were on `master` and had opened `src/review/Swaps.java`
in an editor, then switched to `another_branch` in your bash shell.
After changing branches, you can still see `Swaps.java` in the editor, but its
content only exists in RAM, like an unsaved file. It is not stored on disk anymore
because `another_branch` has a commit that deleted all of `review`.

> You can only checkout __one branch at a time__ in your repo's working
directory. So even if you previously opened files from `master` in an editor,
or you have a second bash window that was looking at `master`, if you
switch to `another_branch`, that affects all running programs that might be
looking at your files.

It's normal to switch branches while you have files open; people do that all the
time. Just be aware that only one version of your files exists on disk at any
one time, depending on the branch, and make sure the correct branch is checked out
before you save any changes.


## Make your own branches

Remember that a branch is just a pointer to a commit.

When you create a branch, you first have to choose a "base" for it: at
which point in history do you want your branch to begin?

The easiest way to choose a base is to checkout an existing branch as our starting point.

1.  Create a branch called "fix_comments" that points to the same commit
    as master:

    ```
    ## First switch to master
    ## If you're already on master, this command won't hurt
    $ git checkout master
    ## Now create the branch
    (master)$ git branch fix_comments
    ## Check the log to see which commit fix_comments points to
    (master)$ git log --decorate
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

2.  Next, switch to your new branch so that you can work on it.

    ```
    (master)$ git checkout fix_comments
    ```

> No files will change as a result of this checkout because `fix_comments` and
master point to the same commit. But, the next time you commit something, the commit will be appended to
the `fix_comments` branch instead of `master`.


Let's make a change on our new branch.

3.  Remove the line "//throw new ArrayOutOfBoundsException..."
    from `src/review/ArrayExamples.java`

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

### Additional info
* [Guide to branches](https://www.atlassian.com/git/tutorials/using-branches).

## Dealing with some common mistakes

Sometimes you enter the wrong command in git by accident. Mistakes
happen to everyone all the time. Let's practice fixing them.

The main command to do this is `git reset`. _Be careful when using
git reset_: it's one of the only git commands that can potentially
obliterate your uncommitted work.

### Oops, I added the wrong thing to the staging area

1.  Modify `README.md` file and the `.gitignore`. It doesn't matter how;
    change anything you like.

2.  Stage all files in the dir: `git add .`

3.  Oh never mind, actually we only want to stage the README. So let's
    reset:

    ```
    (fix_comments)$ git reset
    Unstaged changes after reset:
    M    .gitignore        <---- The "M" means the file is modified.
    M    README.md
    ```

> `git reset` with no extra options removes everything from the staging area but doesn't change any files,
so your modifications are still there.

4. Use `git status` to confirm that your changes are still there.

5.  Now you can re-add just the README file: `git add README.md`

### Argh, all of these changes are bad, can I undo everything?

6.  Stage a tracked file for removal:

    ```
    (fix_comments)$ git rm src/geometry/Point.java
    rm 'src/geometry/Point.java'
    ```

> `git rm` deletes a tracked file and then adds that change to the staging area.

7.  Modify `.gitignore` again, however you like.

8.  `git status` should show:

    ```
    On branch fix_comments
    Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
        deleted:    src/geometry/Point.java

    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git  -- <file>..." to discard changes in working directory)

        modified:   .gitignore
    ```

9.  Actually, never mind, I want to throw away all these uncommitted changes
    _permanently_. Let's go back to the state of the most recent commit on the current
    branch:

    ```
    (fix_comments)$ git reset --hard
    HEAD is now at b20e208 Remove commented-out code
    ```

    Awesome, now all the tracked files are exactly as they were when you
    made the previous commit (removing commented code).

> The `--hard` option means: clear the staging area and undo all
changes to any tracked files.

### Oh noooo I committed the wrong thing! My commit message is terrible!

10. For this example, let's go back to master: `git checkout master`

11.  Modify README.md by added 3 empty lines to the top, then commit it with the
    message "Stuff".

    ```
    (master)$ git add README.md
    (master)$ git commit -m "Stuff"
    ```

How do we fix this terrible commit? Easy. Just make the modifications you need,
then commit again, but with the `--amend` option, which will replace the
most recent commit with your new changes.

12. Try the following:
    ```
    (master)$ vi README.md
    # Remove the 3 empty lines you added
    # Then add your first name to the last line of the file.
    (master)$ git add README.md
    (master)$ git commit --amend
    # An editor will open: change the message to "Add name to README" and save+quit
    ```

13. Use `git show` to check that the most-recent commit got replaced with
    what you just committed. Your old commit message is gone ("Stuff"), as
    well.

### Darn, I accidentally committed my changes on `master` instead of a feature branch :(

We just added a new commit to `master`, but we pretty much never, __never__ want to do that when working on
a team project because master should only contain changes that everyone approves.

So let's move our commit to a new branch.

14. Check the log to find the revision hash for the last published commit
    on master. __The hash you find may not necessarily match these instructions.__
    In the example below, the hash is: `83bc4472...`

    ```
    (master)$ git log --decorate
    commit b20e2088f04171021e1be12e9a5f5076da05a16f  (HEAD -> master) <-- our new commit
    Author: Ada Lovelace <ada@findingada.org>
    Date:   Fri Sep 1 01:08:16 2017 -0400

    Add name to README

    commit 83bc44725ae6ec90211cf0c3d85f37273914de8a (origin/master) <-- last published commit
    Author: Maja Frydrychowicz <mjzffr@gmail.com>
    Date:   Thu Aug 31 21:21:49 2017 -0400

    Override toString in Point
    ```

    The branches look like the diagram below. Notice that the `master` label has
    moved to a new commit (b20e2088...), which is marked with an `@` in the diagram.

    ```
               fix_comments o  @ master (b20e2088...)
                            | /
    o--o--o ... o--o--o--o--o  (83bc4472...)
                             \
                              o another_branch

    ```
15. Create a new branch based on the new commit that we want to remove from master.

    ```
    (master)$ git branch new_stuff
    ```

    Now `new_stuff` and `master` both point at the __new__ commit, `b20e2088f...`

16. Remove the new commit from `master` by resetting it to the last published commit
    whose hash you found earlier. In this example, it's 83bc4472 (you only need to
    write the first 7-8 characters), but it you local repo the hash might be different:

    ```
    (master)$ git reset --hard 83bc4472
    HEAD now at 83bc447
    ```

    Now `master` points at the last published commit, and `new_stuff` still points at the new commit. You can check this by looking at `git log --decorate` again.

    ```
               fix_comments o  @ new_stuff (b20e2088...)
                            | /
    o--o--o ... o--o--o--o--o master (83bc4472...)
                             \
                              o another_branch

    ```


### Additional info

There are lots of resources out there for fixing problems in git.

* [Undoing changes](https://www.atlassian.com/git/tutorials/undoing-changes)
* [git flight rules](https://github.com/k88hudson/git-flight-rules) is an amazing
troubleshooting guide for all sorts of disastrous scenarios.



## Collaboration: Show your branch to team members

After you've done some work, you probably want to publish it and request
feedback or code review from your team. We do that by pushing our branch
to the remote repo and then using the server's web UI to open a "Pull Request" or
"Merge Request" (the name varies depending on which repo hosting
service you are using).

"Pull Requests" or "Merge Requests" are used to ask a project owner or
teammate if they'd like to accept your changes by merging into the
master branch. In other words, it's a request for code review, discussion or feedback.

__In this part, you will upload a branch and request review from a classmate. When you're ready, pair up with a fellow student to review each other's changes.__

1.  Let's create a new branch based on `master` and switch to it:

```
$ git checkout master <-- if you're already on master this won't hurt
(master)$ git branch lab2_yourname
(master)$ git checkout lab2_yourname
```

2. Add a `Gender` called `NONE` to the `Gender` enum in `src/review/Person.java`.

3. Fix the spelling error in `src/geometry/PointUtils.java`              
   in the line `System.out.println("After srting");` -- should be "sorting"

5. Add and commit the two files.

4. Choose any one java file and rename any local variable of your choice.

5. Add and commit the file you changed.

6.  You can compare any two branches. Let's compare this branch to `master`:

    `git diff master lab2_yourname`

    This shows a summary of all changes on `lab2_yourname` compared
    to `master`

__To do this next step, you have to be added to the `origin` repository
as a member/collaborator by the owner of the repo.__ Otherwise, you will
get a "permission denied" error when pushing.

8.  Push your branch to origin:

     ```
     git push origin lab2_yourname
     ```

    This will create a new branch called `lab2_yourname` on the remote
    repo labelled `origin` and upload your commits to it from the current branch. _We
    are including your name in the branch name just to make it
    unique, otherwise everyone doing this tutorial would push to the same
    branch and overwrite each other's work._

9.  Now open the git-playground repo url in your browser again and sign into your account.

#### Creating the actual pull request/merge request: what to expect

When you fill out the web form to create a request (see steps below), you will see
information about the changes you are submitting. This is a chance for
you to check that your request consists of what you intended.

You can see:

*   ...whether there
    are any merge conflicts in the diff between your branch and master: a __merge
    conflict__ happens when the master code has changed (since you wrote
    your code) in a way that disagrees with your changes.
*   ...the __commit messages__ included in your request. This is
    another reason why commit message content and style is important: other
    developers need to easily read these message to get a good understanding of
    the changes.
*   ...the diff of the changes being submitted.


##### If you're on GitHub

Follow these rough instructions below. Details and screenshots on
[github docs](https://help.github.com/articles/creating-a-pull-request/#creating-the-pull-request)

*   Click on the "branches" link to see all the branches. Find your branch.
*   Click "New Pull Request" next to your branch.
*   That loads a form. In the Comment field, mention your teacher's username (`@username`)
    as well as a partner's username (pair up with a classmate).
    * "Could you review this please `@username`?"
*   At the top of the form, make sure the two branches are base: `master`
    and compare: `lab2_yourname` --    
    you want to compare your changes to what's on master.
*   Click "Create Pull Request"

#### If you're on GitLab

Follow these rough instructions below. Details and screenshots on
[gitlab docs](https://docs.gitlab.com/ee/gitlab-basics/add-merge-request.html)

*   Click on "Merge Requests" at the top of the project menu, then "New Merge Request"
*   Choose your branch as the source branch. The target branch should be master.
*   That loads a form. In the Comment field, mention your teacher's username (`@username`) as
    well as a partner's username (pair up with a classmate).
    * "Could you review this please `@username`?"
*   At the top of the form, make sure the two branches listed are `master` and
    `lab2_yourname` -- you want to compare your changes to what's on master.
*   Click "Submit Merge Request"

## Give feedback about a branch

In the request, people can look at your commits and write feedback
about specific lines of code or write general comments. These should always
consist of constructive feedback to make the code better.

10. Try it out by adding pretend comments to your partner's request:
    * Add one general comment (by filling the general comment form on the request summary    
      page)
    * Add one line comment (by clicking on a line of text in the diff shown in the request)
    * If you're not sure how to do this, see these docs:
        * GitHub: <https://help.github.com/articles/commenting-on-a-pull-request/>
        * GitLab: the UI is pretty similar to GitHub, see above.

11. You can also write comments on your own request -- for example, to add
extra clarification, or to reply to a question.

_Please don't "Accept" or "Merge" or "Close" any merge requests in this exercise. Thanks!_

> Pull Requests/Merge Requests are a concept that is specific to repo hosting services.
The service provides a UI to help you communicate with your teammates and exchange
feedback about different branches. git itself does not know anything about these requests.
