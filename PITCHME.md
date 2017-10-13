@title[Introduction]
## git and GitHub in the classroom

##### Maja, Tricia, Jaya

---
@title[Rationale]

![Image of comic](http://smutch.github.io/VersionControlTutorial/_images/vc-xkcd.jpg)

source : http://smutch.github.io/VersionControlTutorial

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

### *free* and *open source* <span style="color:blue">distributed</span> *version control* system

---

@title[distributed]

## What is git?

### <span style="color:grey">free and open source distributed version control system</span>

#### every collaborator has a <span style="color:blue">complete</span> copy of the repository

---

@title[distributed]

## What is git?

### <span style="color:grey">free and open source distributed version control system</span>

#### <span style="color:grey">every collaborator has a </span><span style="color:cornflowerblue">complete</span> <span style="color:grey">copy of the repository</span>

#### centralized server is not required, but greatly facilitates <span style="color:blue">collaboration</span>
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
   git clone url
```
   
---

@title[HLWorkflow2]

### High level workflow
   
#### 2. Make changes

* create files
* edit files

---

@title[HLWorkflow2b]

### High level workflow
   
#### 2. Make changes

* Use your favourite tools to create your content but note that git's revision features work with *text-based* files

* HTML and Markdown formats have editors and are easy to learn

---

@title[HLWorkflow3]

### High level workflow

#### 3. Decide which changes make up a new version, and add and commit them



A *version* in git-speak is a **commit**

---

@title[HLWorkflow3a]

### Commits

* A commit reflects the tracked changes in the repository
* Each commit has a unique identifier
* This facilitates:
	* backtracking/reverting
	* understanding the history of the project
	* branching

---

@title[HLWorkflow3b]

### High level workflow

#### 3. Decide which changes make up a new version, and add and commit them

Some changes belong together in a commit. These changes are first **added** to a staging area, then committed

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
* modified - changed from the version that is tracked

source : http://rogerdudler.github.io/git-guide
---
@title[HLWorkflow4]

#### 4. Upload or *push* them to the remote repository

* Remember when we said that git is *distributed*?
* the changes have been committed to the local repository

* If you started a new project (Hands-on tutorial 1 today), first create the remote repository (in GitHub) and associate it to your local repository

```
   git remote add origin url

   git push origin master

```

---

@title[HLWorkflow4]


#### 4. Upload or *push* them to the remote repository

* If you are contributing to an existing project (Hands-on tutorial 2 today)

```
   git push origin branchname
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

## Hands on Tutorial 1!

---
@title[Branch]

### Branches

* work-in-progress is by definition unstable
* use branches!
	* develop features in isolation
	* frequent commits as code is completed
* once the feature is tested, merge back into main branch

---
@title[Master]

### master Branch

* by default, you work in the master branch
	* notice command prompt ( `master` )
* but the master branch should reflect a <span style="color:blue">stable</span> load
* development should take place in branches

---
@title[Branches picture]

![Image of branches](https://cdn-images-1.medium.com/max/1600/1*coGkd6B_yXvZ1PKXtwra3w.jpeg)

source : https://hackernoon.com/in-defense-of-version-control-f8d8a0d7e23f
---
@title[nvie]

![Image of branching model](http://nvie.com/img/git-model@2x.png)

source: http://nvie.com/posts/a-successful-git-branching-model/

---
@title[protected]

### Protected Branch

##### branches can be protected to ensure that only <span style="color:blue">reviewed and approved</span> changes are merged
---
@title[protected]

### Protected Branch

* in our classes we are using protected branches to:
	* enforce an industry-standard workflow
	* introduce students to pull request documentation
	* ensure students review each others code
	* be alerted when assignments are submitted
	* provide feedback


