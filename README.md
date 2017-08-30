# Intro to git

## What is this?

Hi! You have found a series of tutorials about git.

If you're reading this text in a web browser, you're probably at the URL for a 
_GitHub/GitLab/BitBucket/..._ project page. (From now on, we'll write GitBlarg to mean any one 
of _GitHub/GitLab/BitBucket/..._). This tutorial is stored in a git repository hosted on 
_gitblarg_.com, and _gitblarg_.com provides a web-based user interface for viewing the content of 
the repository, which is what you're looking at right now.

## What is git?

git was created in 2005 by Linus Torvalds. It is a widely used tool for version control:
 in addition to storing files, a
git repository stores detailed information about the _history_ of its files.
This makes it easy to:

* enable many people to combine the changes they make to the same
file(s).
* switch between different versions of files made by you or your collaborators.
* go back to earlier versions of the files if ever you need to recover old work
to fix something that has gone wrong.

You can track any kind of file in a git repository (images, etc.), but it's best suited for
_plain-text_ files like software source code.

## How do I use git?

We'll go through this step-by-step in the actual tutorials, but here's an overview.

### Use the `git` command

You'll mostly interact with git at the command-line. If you're not sure that it's installed,
type `git --version` in a shell. It should be installed by default on
Linux and Mac. If you need to install it
on Windows, install it from <https://git-for-windows.github.io/>, which provides you with a bash
terminal called _git-bash_. See also: [Installation instructions for Mac/Win/Linux](https://www.atlassian.com/git/tutorials/install-git)

### Keep a local repo

The simplest way to use git is to maintain a repository locally. You can have many
repositories, one for each project you're working on. You don't have to collaborate
with anyone; you can just use git to track version history to organize your own work.

### "Push" your repo to a remote server

If you wish, you upload a copy of your local repo to a remote repository server. This is called
_pushing to a remote_. Use can use this to back up your work, or your can also it to collaborate
with others.

### Collaborate

If many people all push to the same remote repo, they can share work that way. git provide ways
to keep your changes distinct from your collaborators' changes. When you're ready, you can merge
those changes together to make progress on your team project. _We'll learn more about how this
works in future labs._

## Let's get started!

In the first lab, you'll create a repository on your computer and use it to learn some git
commands. Then you'll also publish your repository to a remote server like gitlab.com and get a
glimpse of how git is used for collaboration.

[Start the lab here](01_basics.md)


## Resources

For working on this repo:

-   [markdown cheat sheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)
-   [original main on mediawiki](http://wiki.pcampbell.profweb.ca/index.php/Using\_git)
-   [original lab 1 on mediawiki](http://wiki.pcampbell.profweb.ca/index.php/Introductory\_Git\_Lab)
-   [differences between github/gitlab/bitbucket](https://about.gitlab.com/2016/01/27/comparing-terms-gitlab-github-bitbucket/)

<img src="https://imgs.xkcd.com/comics/git_2x.png" width=300 alt="xkcd on git"/>
