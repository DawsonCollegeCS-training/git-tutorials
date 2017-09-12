# Protected Branch Workflow

There are several popular git workflows used in industry. A git workflow is a set of
steps and conventions that all project contributors adhere to while working in a
shared repository. A workflow describes
how a single developer's experimental code is eventually integrated into the main
project codebase once it is deemed to be of sufficient quality.

The main idea in the _protected branch workflow_ is that one or more branches
is only ever modified when the whole team agrees that specific commits should be added.
Protected branches usually represent a more stable, higher-quality snapshot of the project or
a version that might be released to users, for example.

* The master branch is always protected.
* Additional agreed-upon branches may also be protected. In the section below, we
  recommend one protected "staging" branch to represent a shared milestone that will
  eventually be merged with master.

# Recommended Team Workflow

We describe a workflow for a team, but a very similar process can be used by an individual
working in a repository without any collaborators.

Let's assume that `origin` points to a repository shared by several students working on
a team assignment.

### Initial setup: Start a "staging" branch

This is the branch on which record your ready-to-submit work. When your work is done, you will make a PR of staging against master. Let's say you're working on Assignment 5, then you can call your staging branch "Assignment5".

Make sure that you're on master branch, master branch up-to-date, start new branch:

```
git checkout master
git pull origin master
git checkout -b Assignment5
git push origin Assignment5
```

Other teammates will have to fetch the staging branch after the first clone the team repo:

```
git clone url/to/project
git fetch origin Assignment5:Assignment5
```

### Individual member starts work on a feature

Work on your assignment in logical pieces, with each piece developed on a separate branch. If it's a small/simple individual assignment, maybe you only need one feature branch for all your work.

1. Make sure that you're on staging branch, staging branch up-to-date, start new branch:

```
git checkout Assignment5
# in case new code from teammates has been merged on your staging branch
git pull origin Assignment5
git checkout -b featureA
```

Never commit or push to the master branch: in class assignments, your submission will be merged into master after it's graded. Never commit or push directly to the staging branch unless you're preparing the very final version of your assignment submission -- only team-approved, working code can land there by merging feature branches via merge/pull requests. If you're a team of one, you will decide when to merge feature branches.

2. Work work work

```
git add somefiles
git commit
...
```

3. You can push your branch to the remote whenever your like to back up your work or make it visible to your teammates.

```
git push origin featureA
```

4. You can pull your branch from the remote if you switch to a different computer

```
# first time on new computer
git clone url/to/submission/repo
cd path/to/submission/repo
git fetch origin featureA:featureA
# subsequent times just do
git pull origin featureA
```

### Get feedback from teammates

5. When you want feedback on your work, open a PR for your work against the staging branch. Do this when you want early feedback from teammates (__f?__), or when you want official code review (__r?__) from teammates so that your work can be merged into the staging branch.

```
# in case teammates have landed new work on staging branch, bring it up to date
git checkout Assignment5
git pull origin Assignment5
git checkout featureA
# replay your changes onto latest Assignment5 and fix any conflicts
git rebase Assignment5
git push origin featureA
```

Then use gitblarg UI online to open a PR against the Assignment5 and flag a teammate for review or discussion.

> #### Review flags
> Use a common shorthand to signal specific requests and official responses/decisions in your
PR:
>
> * `f?` - Please give me some informal feedback on this work-in-progress.
> * `f-` - I disagree with the general direction of this code. A different approach is needed.
> * `f+` - I think you're on the right track.
> * `r?` - Please review this finished code so that is can be merged.
> * `r-` - This code needs additional changes before it can be merged.
> * `r+` - This code is correct and complete, let's merge it!

6. When you get feedback (__f-, r-__) on your PR and need to change things, just make new commits on your feature branch and push again.

```
git add; git commit
git push origin featureA
```

The new commits will appear on the PR, and you can ask your teammates for feedback again.

7. (Optional) Once your feature is approved in the PR, you can clean up your commits and commit messages if needed. Modify your commit message(s) to indicate the author(s) and reviewer(s) of each commit.

```
git rebase -i HEAD~n
git push -f origin featureA
```

##### If you are working alone

Instead of opening a PR, you can just merge your feature branch into your staging branch yourself at the command-line whenever you are happy with your feature.

```
git checkout Assignment5
git merge featureA
```

##### When your feature is approved by the team

8. When your PR is approved (__r+__ from teammate) __Merge__ it in the gitblarg UI: now featureA gets merged into the staging branch (Assignemnt5).

Pull the new commits from the remote. The commits you made for featureA are now on Assignment5.

```
git checkout Assignment5
git pull origin Assignment5
```

(Optional) Now you can also safely delete your featureA branch locally and on the remote because it's been merged. To delete it on the remote, find the delete button in the web UI. To delete it from your local repo:

```
git branch -d featureA
```

9. Keep working on more parts of the assignment on new branches. (Repeat the above process as needed. You can alternate working on several features at once; you just need to keep the different branches up to date.)

```
git checkout Assignment5
git checkout -b featureB
```


##### When your are ready to submit your work

You submit your work by making a PR of your staging branch (Assignment5).  Only one teammate needs to do this:

```
# pull in all the latest changes on the staging branch (to make sure you have all your teammates' work)
git checkout Assignment5
git pull origin Assignment5
# check that the log makes sense, test the code, check that the commits look good
git log -p
etc...
# then make the final push
git push origin Assignment5
```

Note: Never force-push (git push -f) to the staging branch, because this rewrites history that you share with your teammates! If you force push, your teammates will have trouble pulling and will have to waste their time fixing their repo!

Next open the PR in gitblarg UI  of Assignment5 against __master__ and request review from your teacher.

##### When you want to incorporate feedback from teacher

This is the same as incorporating feedback from teammates. After grading, teacher might require you to make some changes before approving your PR and merging into master.

Add more commits to your Assignment5 branch and push.
