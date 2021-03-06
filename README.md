[![Build Status](https://travis-ci.com/mtumilowicz/git-merge-vs-rebase.svg?branch=master)](https://travis-ci.com/mtumilowicz/git-merge-vs-rebase)

# git-merge-vs-rebase
_Reference_: https://www.atlassian.com/git/tutorials/merging-vs-rebasing  
_Reference_: https://hackernoon.com/git-merge-vs-rebase-whats-the-diff-76413c117333  
_Reference_: https://www.atlassian.com/git/tutorials/merging-vs-rebasing  
_Reference_: https://git-scm.com/docs/git-rebase  
_Reference_: https://git-scm.com/docs/git-merge

# preface
The main goal of this project is to show difference between merge
and rebase and to describe squash.

## merge
**git-merge** - Join two or more development histories together.

All by itself, the merge commit represents every change that has 
occurred on feature since it branched from master.

	      A---B---C topic
	     /
    D---E---F---G master
    
	      A---B---C topic
	     /         \
    D---E---F---G---H master

* **pros**: Merging is nice because it’s a non-destructive operation. 
The existing branches are not changed in any way.
* **cons**: Merge commits can clutter up your Git logs, and make it much 
more difficult to understand the flow of your project’s history.


* Avoid branching and merging when only making minor tweaks or trivial 
bug fixes. Use merge for cases where you want a set of commits to 
stand out.
* Large refactors and major feature additions are good candidates 
for separate feature branches that can later be merged into master. 
As an added bonus, when merges are reserved for these major changes, 
the merge commits act as milestones that others can use to figure 
out when these major changes were incorporated into the project.

## rebase
**git-rebase** - Reapply commits on top of another base tip

At a high level, rebasing can be understood as "moving the base 
of a branch onto a different position".

          A---B---C topic
         /
    D---E---F---G master
    
                  A'--B'--C' topic
                 /
    D---E---F---G master

* The feature branch’s history has been completely rewritten.
* Although the changes on the newly rebased feature branch are 
  identical to what they were before, it is good to note that, 
  from Git’s perspective, these are new commits with new SHA’s 
  (the commits’ identifiers).
* https://www.atlassian.com/git/tutorials/merging-vs-rebasing#the-golden-rule-of-rebasing
* **The one-line summary**: don’t rebase a branch unless you are 
the only one who uses it.

## squash
Squash n commits into single one.

# project description
What we want to do:
1. modify README.md
1. base branch: a b
1. feature branch: 
    1. first commit: a **b** 
    1. we add to base branch `new_file1.txt`
    1. second commit: **a** **b**
1. we add to base branch `new_file2.txt`
1. we want to move changes from feature branch to base branch

|action   |base branch   |feature branch   |
|---|---|---|
|merge   |for-merge   |for-merge-feature   |
|merge + squash   |for-merge-squash   |for-merge-squash-feature   |
|rebase   |for-rebase   |for-rebase-feature   |

## history

* **merge**
    * base branch (`for-merge`)
        * Merge pull request ...
        * Create new_file2.txt
        * **a** **b**
        * Create new_file1.txt
        * a **b**
        * a b
    * feature branch (`for-merge-feature`)
        * **a** **b**
        * a **b**
        * a b
* **rebase**
    * base branch (`for-rebase`)
        * **a** **b**
        * a **b**
        * Create new_file2.txt
        * Create new_file1.txt
        * a b
    * feature branch (`for-rebase-feature`)
        * **a** **b**
        * a **b**
        * a b
* **merge + squash**
    * base branch (`for-merge-squash`)
        * For merge squash feature (#17) ...
        * Create new_file2.txt
        * Create new_file1.txt
        * a b        
    * feature branch (`for-merge-squash-feature`)
        * **a** **b**
        * a **b**
        * a b        

# summary
* Use merge in cases where you want a set of commits to be clearly 
grouped together in history
* Use rebase when you want to keep a linear commit history
* DON’T use rebase on a public/shared branch