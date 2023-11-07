# git-master-class
This is to learn about GIT commands and repositories


## What is repository?

## What is a Git repository?

## Architecture of repository?

## Credentials

git config user.name "auto.bee"
git config user.email "opra.student@gmail.com"



## Clone an existing repository
`git clone git@github.com:opsrace/git-master-class.git`

## Branches
List local branches

`git branch`
```
  feature/forgot-password-error
* master
```

List local as well as tracking branches, the branches that are tracking remote branches

`git branch -a`
```
  feature/forgot-password-error
* master
  remotes/origin/feature/forgot-password-error
  remotes/origin/feature1
  remotes/origin/master
```

## Git commit
First we need to stage the changes before we can commit the code
### Stage files
Add every thing to the staging area

* `git add .`


Add a very specific file to the staging area
* `git add index.html`

Add a partial change (git patch)

* `git add -p index.html`

### Git status
Tells the current sitaution of the local repository
1. Anything is changed or in untracked state
2. Anything that is in staging area
3. If everything is upto date
4. Current branch (moderen tools will show it with your prompt)

### Git commit
After staging the content next step is to make it part of the repository.

`git commit` this will popup a window to enter the message

    A commit must contain a concise title (subject) and a message that explain the commit.

Commit code with a message pre-populated
```
git commit -m "First line concise message here
Second line after the enter with a detailed message
How its different from before?
What caused this change? Why we are doing this?
Will it impact anything else or may be something for the reviewer?`
```

### Checkout
### Switch
### Add
### Add and switch
### Delete branch
#### Safe delete branch
`git branch -d branch-name`

The above command will warn you, if its pointing to some commit which is not reachable by any other pointer (branch)

error: The branch 'feature/some-feature' is not fully merged.
If you are sure you want to delete it, run 'git branch -D feature/some-feature'.

#### Forcefully delete branch
`git branch -D branch-name`
It will delete the branch but the node (change set) will be left dangling

Git will automatically garbage collect this (I am not sure if there is a way to particularly delete this node, but garabage collect can be forced)


`git gc`
Enumerating objects: 64, done.
Counting objects: 100% (64/64), done.
Delta compression using up to 12 threads
Compressing objects: 100% (51/51), done.
Writing objects: 100% (64/64), done.
Total 64 (delta 26), reused 0 (delta 0), pack-reused 0


f609665
### delete remote branch
Now delete remote branch
`git push origin -d feature1`

`git remote prune origin` 
Prunes tracking branches not on the remote.




### Push local branch
### Sync local branch

## Deatached state
`git checkout f609665`

Note: switching to 'f609665'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.


# Magic Force git branch to point something else
`git branch -f master HEAD~3`

* Master must not be current branch at the time of above command

This command will force main branch to point to leave current position and move 3 commits back (to the parent of third)

`C0 <- C1 <- C2 - C3 - C4 (master, bugfix *)`

`C0  <- c1(master) <- C2 <- C3 <- C4(bugfix *)`

git branch -f main C0


## Force master branch to specific commit
git checkout master
git reset --hard <old_commit_id>


C0 - C1 - C3 (main *)
`git reset HEAD~1`
C0 - C1 (main *)


`git rever HEAD`
C0 - C1 - C3 (main *)
C0 - C1 - C3 - C3`(main *)

`git revert -m 1 15e51dd`
A merged feeature branch changes will be remove, in this command its assume this branch is created from single parent.


## Cheery Pick
`git cherry-pick c1 c2`

## Tags
git tag v1 C1

* Cant commit in Dearached mode (or when checkedout a tag v1)

You bet there is! Git tags support this exact use case -- they (somewhat) permanently mark certain commits as "milestones" that you can then reference like a branch.
More importantly though, they never move as more commits are created. You can't "check out" a tag and then complete work on that tag -- tags exist as anchors in the commit tree that designate certain spots.
Let's see what tags look like in practice.

## Git Describe

`git describe`

it will describe current head

\<tag\>_\<total_commits\>_g\<hash\>

tag: is the closest tag to current position

total_commits: how far (no of commits) is that tag from current position

hash: hash of the commit being described


`git describe ref`

it will desciribe specified reference "ref"

```
C0 [v1] <- C1 <- C2 (master)
            ^
            |_ C3 [v2] <- C4 (queen *)

```

`git describe master`

v1_2_gC2

`git describe`

`git describe queen`

v2_1_gC4


## Rebase

* Rebase bugFix with main

`git checkout bugFix`

`git rebase main`

Commits in bugFix will be re-evalutaed on top of main's last commit and hence bugFix pointer will move to newly calculated commits.

#### Shortcut
`git rebase main bugFix`

Same as above


## Specifying Parents

Like the ~ modifier, the ^ modifier also accepts an optional number after it.

Rather than specifying the number of generations to go back (what ~ takes), the modifier on ^ specifies which parent reference to follow from a merge commit. Remember that merge commits have multiple parents, so the path to choose is ambiguous.

Git will normally follow the "first" parent upwards from a merge commit, but specifying a number with ^ changes this default behavior.

Enough talking, let's see it in action.


> ^ Which parent to follow

> ~ How many parents to go back


```
C0 <- C1 <- \ 
   \         \
    \<- C2 <- C3 (main *)
```

> First parent

git checkout HEAD^

```
C0 <- C1 (HEAD)<-\ 
   \              \
    <- C2 <------ C3 (main)
```

> Second parent (older)

`git checkout HEAD^2`

```
C0 <- C1 <--------\ 
   \               \
    <- C2 (HEAD) <- C3 (main)
```

Git checkout HEAD~^2~2

```
    C0
   --|—-
  /     \
C1      C3
 |       |
C2      C4
 |       |
 |      C5
 |______/
 |
C6
 |
C7 (main *)
```

```
Git checkout HEAD~;
Git checkout HEAD^2;
Git checkout HEAD~2;
```
and its impact

* Go one step back (C6)
* Select second parent (C5)
* Go two step back (C3)

HEAD now points to C3, main remains at C7

### Crazier
`git checkout~^2~2`

It will have same impact as above 3 commands

### create a new branch 'bugWork' a C3
`git branch bugWork HEAD~^2~2`
* Create branch bugWork at
* Go one step back
* Choose second parent
* Go 2 steps back


git branch -vv
which local branch pointing to remote branch


git checkout -b Maryam —track origin/marry




## Create a new branch from specific commit
git branch branch_name <commit-hash>



git log --graph --oneline --all
*   6ccb56b (HEAD -> master, origin/master, origin/HEAD) Merge pull request #5 from opsrace/feature/yello-color-added-to-pallet
|\
| * 8680196 Yellow file added
|/
* 0357e43 Transformers added to the system
* 9ac96dc Local change 1
* 5972da0 Update e.txt
* 668e256 Remote 1
* bad55d5 4th commit on master branch
* ab9f88b Third commit
* 5a8e701 Second commit
* 373ebf5 First commit



git log --graph --oneline --all
* c89ec7d (HEAD -> master) Green is added as well
*   6ccb56b (origin/master, origin/HEAD) Merge pull request #5 from opsrace/feature/yello-color-added-to-pallet
|\
| * 8680196 Yellow file added
|/
* 0357e43 Transformers added to the system
* 9ac96dc Local change 1
* 5972da0 Update e.txt
* 668e256 Remote 1
* bad55d5 4th commit on master branch
* ab9f88b Third commit
* 5a8e701 Second commit
* 373ebf5 First commit

git fetch --all

git log --graph --oneline --all
* c89ec7d (HEAD -> master) Green is added as well
| * c36afe1 (origin/master, origin/HEAD) Merge pull request #6 from opsrace/auto-bee-remote-change
|/|
| * e04b1c1 Change on Remote a.txt
|/
*   6ccb56b Merge pull request #5 from opsrace/feature/yello-color-added-to-pallet
|\
| * 8680196 Yellow file added
|/
* 0357e43 Transformers added to the system
* 9ac96dc Local change 1
* 5972da0 Update e.txt
* 668e256 Remote 1
* bad55d5 4th commit on master branch
* ab9f88b Third commit
* 5a8e701 Second commit
* 373ebf5 First commit


git remote show origin
* remote origin
  Fetch URL: git@github.com:opsrace/first-git.git
  Push  URL: git@github.com:opsrace/first-git.git
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (fast-forwardable)