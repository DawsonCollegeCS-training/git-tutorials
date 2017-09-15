# Git Tutorial: Mistakes, Branches (Setup For Teachers)

## Protected Branch

The intended audience of this document is teachers who
are setting up this git tutorial for their class.

To goal is to set up a public or private repo (your choice) where you will collect PRs/MRs from your class, and students use the "protected branch" workflow.

These steps refer to either of these two public repos, choose one:

* <https://github.com/DawsonCollegeCompSci/git-playground>
* <https://gitlab.com/dawsoncollege/git-playground>

Whichever you choose, let's call it `upstream`. We will manually set up a fork of `upstream` in a namespace for our class. (Manual fork setup should work regardless of the privacy of the repos involved.)

### GitHub

1.  Create an organization for your course section for this semester. This
    isn't essential, but it's tidy to keep all
    instances of these git exercises organized per semester.

2.  Create an empty repo called `git-playground` in your chosen namespace
    (e.g. the org for your class).

3.  Under the repo Settings > Collaborators & Teams, add individual
    students as [collaborators][github-collaborators]. Give them __Write__
    access. You can do this in advance or at the beginning of the lab.
    (This step is what enables the protected-branch workflow.)

[github-collaborators]: https://help.github.com/articles/adding-outside-collaborators-to-repositories-in-your-organization/
[github-restrict-branch]: https://help.github.com/articles/enabling-branch-restrictions/
[github-protect-branch]: https://help.github.com/articles/configuring-protected-branches/


### GitLab

1.  Create a [group or subgroup][gitlab-group] for your course section for
    this semester. This isn't essential, but it's tidy to keep all
    instances of these git exercises organized per semester.

2.  Create an empty repo called `git-playground` in your chosen namespace
    (e.g. the group for your class).

3.  Under Settings > Members, add individual students as
    [members][gitlab-members] with the __Developer__ role. You can do this
    in advance or at the beginning of the lab. (This step is what enables
    the protected-branch workflow.)

[gitlab-members]:https://docs.gitlab.com/ce/user/project/members/index.html
[gitlab-group]:https://docs.gitlab.com/ee/user/group/index.html#create-a-new-group

## Both

So now you should have an empty repo at `url/to/mynamespace/git-playground`.

Locally:

```bash
$ mkdir class-git-playground; cd class-git-playground
$ git init
$ git remote add origin url/to/mynamespace/git-playground
$ git remote add upstream url/to/upstream/git-playground
$ git pull upstream master
$ git fetch upstream another_branch:another_branch
```

`git branch` should now show two local branches: `master` and `another_branch`

```
$ git checkout master; git push origin master
$ git checkout another_branch; git push origin another_branch
```

## Optional Steps  

### GitHub

Go back to the git-playground repo you created on GitHub,
go to repo Settings > Branches, make `master` a [protected branch][github-protect-branch].
Also check the option to [restrict who can push this branch][github-restrict-branch]

### GitLab

Branches in GitLab are protected by default, but if you're not sure
you can check under Settings > Repository.

## That's it!

Now give students the link to your `origin` (`url/to/mynamespace/git-playground`) so they can clone it and make a PR against master.
