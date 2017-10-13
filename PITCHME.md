@title[Introduction]
## git and GitHub in the classroom

##### Maja, Tricia, Jaya

---
@title[Rationale]

![Image of comic](http://smutch.github.io/VersionControlTutorial/_images/vc-xkcd.jpg)

##### *source : http://smutch.github.io/VersionControlTutorial*
---
@title[Version Control1]

## Version Control?

#### Record and manage <span style="color:red">changes</span>


---
@title[Version Control2]

## Version Control?

#### <span style="color:grey"> Record and manage </span><span style="color:thistle">changes</span>

#### Facilitate sharing and <span style="color:green">collaboration</span>

---

@title[git]

## What is git?

### free and open source distributed version control system

---

@title[distributed]

## What is git?

### *free* and *open source* <span style="color:blue">distributed</span> *version control* system

#### every collaborator has a <span style="color:blue">complete</span> copy of the repository

---

@title[distributed]

## What is git?

### *free* and *open source* <span style="color:blue">distributed</span> *version control* system

#### every collaborator has a <span style="color:blue">complete</span> copy of the repository

#### centralized server is not required, but greatly facilitates collaboration workflow
---

@title[GitBlargh]

## Centralized Git Servers

### GitHub

### GitLab

### BitBucket

### ...
---

@title[HLWorkflow1]

### High level workflow

#### 1. Create or clone a repository (local or remote)

**Repository?** Project files as they change over time, version history, and other metadata

   
---

@title[HLWorkflow1a]


#### 1. Create or clone a repository (local or remote)

* either start a project from scratch (Hands-on tutorial 1 today)

```
   git init 
```

* or contribute to an existing project (Hands-on tutorial 2 today)

```
   git clone
```
   
---

@title[HLWorkflow2]

### High level workflow
   
#### 2. Make changes

* create files
* edit files

##### You can use you favourite tools to create your content but note that git's revision features work with *text-based* files


* HTML and Markdown formats have easy editors and are easy to learn

* This presentation is made in markdown, ugliness is due to presenter, not the tool!

---

@title[HLWorkflow3]

### High level workflow

#### 3. Decide which changes make up a new version, and add and commit them

##### A *version* in git-speak is a *commit*

* A commit reflects the changes in the repository
* Each commit is given a unique identifier
* This facilitates:
	* understanding history of the project
	* backtracking/reverting
	* branching

---

@title[HLWorkflow3b]

### High level workflow

#### 3. Decide which changes make up a new version, and add and commit them

Some changes belong together in a commit. These changes are first **added** to a staging area

```
	git add filename.md

	git commit -m "explain the changes that make up this commit"

```
---

@title[HLWorkflow3b]

![Image of add and commit](http://rogerdudler.github.io/git-guide/img/trees.png)

* untracked - new file, not yet in staging area
* staged - should be included in the next commit
* tracked - committed
* modified - file is changed from the version that git last tracked

##### *source : http://rogerdudler.github.io/git-guide*
---
@title[HLWorkflow4]

### Remember when we said that git is *distributed*?

#### 4. Upload or *push* them to the remote repository

* the changes have been committed to the local repository

* If you started a project from scratch (Hands-on tutorial 1 today), you need to create the remote repository (in GitHub) and associate it to your local repository

```
   git remote add origin url

   git push origin master

```

---

@title[HLWorkflow4]

#### Remember when we said that git is *distributed*?

#### 4. Upload or *push* them to the remote repository

* If you are contributing to an existing project (Hands-on tutorial 2 today)

```
   git push origin master
```

---
@title[HLWorkflow4]

### High level workflow

#### 5. Collaborate with others

* you may not be the only one making changes to the remote repository
* you need to make sure you are working on the latest version


```
   git pull origin master


```

---
@title[HLWorkflow4b]

### High level workflow summary

#### 1. Create a repository (local or remote)
   
#### 2. Make changes

#### 3. Decide which changes make up a new version, and add and commit them

#### 4. Upload or *push* them to the remote repository

#### 5. Collaborate with others

---

## Let's do it!