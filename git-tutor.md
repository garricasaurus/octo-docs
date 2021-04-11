# About

This is a `no-nonsense`, `follow-along` git tutorial for getting from beginner to intermediate level.

I assume the following:
   * you have basic computer literacy
   * if you see an unfamiliar command
      - you will try to understand it from the context
      - you will look it up on the internet
   * you are doing the steps, not just reading them 

Good luck! :)

# Create a sandbox

## Create a repository

Create a working directory:

```
mkdir git-tutor
cd git-tutor
```

Initialize an empty git repository:

```
git init
```

Configure your git username and email:

```
git config --global user.name "Your Name" 
git config --global user.email "email@example.com"
```

# Create your first commit


## Create a new file

```
echo "# Cat Database" > README.md
```

## Check `git status`

```
git status
```

```
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md

nothing added to commit but untracked files present (use "git add" to track)
```

`On branch master` means, that we are on the main branch of our repository. Each repository can have multiple branches containing different work/changes. More about this later.

## Stage the file

Files need to be staged before commit:

```
git add README.md
```

```
git status
```

```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md

```

## Make your first commit

```
git commit -m "first commit"
```

```
git status
```

```
On branch master
nothing to commit, working tree clean
```

## Make another commit

A commit can naturally contain multiple files. Let's add a couple of new files:

```
echo "hybrid of Egyptian Mau and Asian leopard cat" > bengal.md
echo "a large domestic cat with a distinctive physical appearance" > maine-coon.md
echo "docile and affectionate cat breed with a soft and silky coat and blue eyes" > ragdoll.md
```

```
git status
```

```
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        bengal.md
        maine-coon.md
        ragdoll.md
```

Stage all of them, and commit:

```
git add -- all
git commit -m "cat breeds"
```

```
 3 files changed, 3 insertions(+)
 create mode 100644 bengal.md
 create mode 100644 maine-coon.md
 create mode 100644 ragdoll.md
```

# Check the `git log`

The `git log` contains useful information about our repository. In its simplest form it will display the most recent commits ordered from newer to older.

```
git log
```

```
commit d1f01ee42d4aa2e9c089906a74895335d6992938 (HEAD -> master)
Author: ... 
Date:   Fri Apr 9 18:57:59 2021 +0200

    cat breeds

commit ad0e552fe1854a1a575f520cc2de8a8c4add6b50
Author: ... 
Date:   Fri Apr 9 18:56:27 2021 +0200

    first commit
```

Notable things:

   * commits are identified by a hash (`d1f01e`)
   * commits have an author, date and a message
   * commits can have different pointers pointing to them (`master`)

`HEAD` is a special pointer, which points to the current branch reference: `master` (which in turn points to the latest commit in the working directory).

The `git log` command can of course format the output in many different ways.

A few variants:

```
git log --pretty=oneline --author="..." --since="yesterday"
```

```
d1f01ee42d4aa2e9c089906a74895335d6992938 (HEAD -> master) cat breeds
3b78e664e96ecb8ad356927bb0e80408a6b70ff1 first commit
```

```
git log --pretty=format:'%h %ad | %s%d [%an]' --date=short
```

```
d1f01ee 2021-04-09 | cat breeds (HEAD -> master) [...]
ad0e552 2021-04-09 | first commit [...]
```

```
git log --graph
```

```
* commit d1f01ee42d4aa2e9c089906a74895335d6992938 (HEAD -> master)
| Author: ... 
| Date:   Fri Apr 9 18:57:59 2021 +0200
|
|     cat breeds
|
* commit ad0e552fe1854a1a575f520cc2de8a8c4add6b50
  Author: ... 
  Date:   Fri Apr 9 18:56:27 2021 +0200

      first commit
```

# Undo changes in an unstaged file

Let's modify one of the files:

```
sed -i 's/blue/green/g' ragdoll.md
```

```
git status
```

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   ragdoll.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Note, that the change is not staged yet. So let's undo the change:

```
git checkout ragdoll.md
```

```
Updated 1 path from the index
```

```
git status
```

```
On branch master
nothing to commit, working tree clean
```

# Compare unstaged changes

Let's modify some files:

```
sed -i 's/cat/dog/g' *.md
```

```
git status
```

```
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   bengal.md
        modified:   maine-coon.md
        modified:   ragdoll.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Now let's compare what changed:

```
git diff *.md
```

```
diff --git a/bengal.md b/bengal.md
index d4312cc..4b03a51 100644
--- a/bengal.md
+++ b/bengal.md
@@ -1 +1 @@
-hybrid of Egyptian Mau and Asian leopard cat
+hybrid of Egyptian Mau and Asian leopard dog
diff --git a/maine-coon.md b/maine-coon.md
index 7569f85..1b19f68 100644
--- a/maine-coon.md
+++ b/maine-coon.md
@@ -1 +1 @@
-a large domestic cat with a distinctive physical appearance
+a large domestic dog with a distinctive physical appearance
diff --git a/ragdoll.md b/ragdoll.md
index 59a6bfc..e0d7fca 100644
--- a/ragdoll.md
+++ b/ragdoll.md
@@ -1 +1 @@
-docile and affectionate cat breed with a soft and silky coat and blue eyes
+docile and affectionate dog breed with a soft and silky coat and blue eyes
```

`git diff` compares the files line-by-line and displays changes in a special syntax, which contains both the old version and the new version of the line. (In some terminal emulators you will also see colored output.)


# Undo staged changes

Let's stage all changes:

```
git add -all
git status
```

```
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   bengal.md
        modified:   maine-coon.md
        modified:   ragdoll.md
```

Now let's revert the state of the working tree to what it was before we started replacing cats with dogs:

```
git reset --hard
```

```
HEAD is now at d1f01ee cat breeds
```

The `git reset` command can move the different pointers, like `HEAD`. Not specifying the pointer will move `HEAD` by default.

A hard reset will not only move the `HEAD` pointer, but also restore the state of the directory tree. More on different types of `reset` later.

# Interacting with previous commits

## Prepare the scene

Let's rename one of the files in our workspace:

```
git mv bengal.md kitty.md
```

Commit the change:

```
git commit -m "rename bengal"
```

```
[master e6be47c] rename bengal
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename bengal.md => kitty.md (100%)
```

Check the log:

```
commit e6be47c7014722a442ee0e15182caa7926858f91 (HEAD -> master)
Author: ... 
Date:   Fri Apr 9 19:18:57 2021 +0200

    rename bengal

commit d1f01ee42d4aa2e9c089906a74895335d6992938
Author: ... 
Date:   Fri Apr 9 18:57:59 2021 +0200

    cat breeds

commit ad0e552fe1854a1a575f520cc2de8a8c4add6b50
Author: ... 
Date:   Fri Apr 9 18:56:27 2021 +0200

    first commit
```

## Checkout a previous commit

```
git checkout d1f01ee
```

```
Note: switching to 'd1f01ee'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at d1f01ee cat breeds
```

The output of the `checkout` command explains the situation pretty well.

If you look around the workspace, you will see that it reflects the earlier state:

```
ls
```

```
README.md  bengal.md  maine-coon.md  ragdoll.md
```

## Tagging

Tagging a commit simply adds a new label to it. This can be a version number, or an arbitrary label we can use to refer to it.

```
git tag v1.0.0
```

Let's verify the tag in the logs:

```
git log -1
```

```
commit d1f01ee42d4aa2e9c089906a74895335d6992938 (HEAD, tag: v1.0.0)
Author: ...
Date:   Fri Apr 9 18:57:59 2021 +0200

    cat breeds
```

To switch back:

```
git checkout master
```

Checking out the tag again is pretty easy:

```
git checkout v1.0.0
```

You can checkout a commit _relative_ to a tag. For example to checkout the parent commit of the `v1.0.0` tag use the `~1` notation:

```
git checkout v1.0.0~1
```

```
HEAD is now at ad0e552 first commit
```

Multiple tags can be added to the same commit:

```
git tag v0.9.9
git tag first
```


All tags can be listed with the `git tag` command:

```
git tag
```

```
first
v0.9.9
v1.0.0
```

Switch back to master after you are done tagging:

```
git checkout master
git log --oneline
```

```
e6be47c (HEAD -> master) rename bengal
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

## Reverting commited changes

You can use the `git revert` command to undo a specific commit:

```
git revert e6be47c --no-edit
```

```
[master 08da7f2] Revert "rename bengal"
 Date: Fri Apr 9 19:30:41 2021 +0200
 1 file changed, 0 insertions(+), 0 deletions(-)
 rename kitty.md => bengal.md (100%)
```

```
git log --oneline
```

```
08da7f2 (HEAD -> master) Revert "rename bengal"
e6be47c rename bengal
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Note how `revert` just generated a new commit, that removed the change introduced by the reverted commit `e6be47c`.

Instead of using the commit hash, you can also use `HEAD` or a specific tag name when issuing the `git revert`  command.

## Removing commits from the log

__Removing commits alters history.__ Always keep this in mind when removing commits. Altering history can cause nasty problems when your repository has already been shared with others in its previous state (for example you performed `git push`, and someone `git clone`d it and made some commits locally).

__You have been warned.__

Now let's clean up our cat repository, and remove the most recent 2 commits. We will use the `reset` command and the `v1.0.0` tag we created in the previous section:

```
git reset --hard v1.0.0
```

```
git log --oneline
```

```
d1f01ee (HEAD -> master, tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Are the two commits `e6be47c` and `08da7f2` truly gone though? Not yet. Until the next git garbage collection, they can still be referenced with their hashes:

```
git checkout 08da7f2
```

At this point we may decide to `tag` the commit to prevent it from being garbage collected:

```
git tag dead-end 
```

```
git log --oneline --all
```

```
08da7f2 (HEAD, tag: dead-end) Revert "rename bengal"
e6be47c rename bengal
d1f01ee (tag: v1.0.0, master) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Note how `master` is now _behind_ `HEAD`.

Finally let's remove the `dead-end` tag:

```
git tag -d dead-end
```

```
Deleted tag 'dead-end' (was 08da7f2)
```

```
git checkout master
```

```
Warning: you are leaving 2 commits behind, not connected to
any of your branches:

  08da7f2 Revert "rename bengal"
  e6be47c rename bengal

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> 08da7f2

Switched to branch 'master'
```

We got a warning: if don't tag or create the commits, soon they will be lost forever.

## Amending commits

Let's add a new cat breed:

```
echo -n "the pedigreed version of the traditional British domestic cat" > british-shorthair.md
```

```
git add british-shorthair.md
git commit -m "add british shorthair"
git log --oneline
```

```
3c73bd6 (HEAD -> master) add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

_Oops_. We already commited the change, but let's say we forgot to add something to the description. In this case we can `amend` the last commit:

Let's append the missing part of the description:

```
echo ", with a distinctively stocky body, dense coat, and broad face" >> british-shorthair.md
git diff british-shorthair.md
```

```
diff --git a/british-shorthair.md b/british-shorthair.md
index 40da3e6..12585d1 100644
--- a/british-shorthair.md
+++ b/british-shorthair.md
@@ -1 +1 @@
-the pedigreed version of the traditional British domestic cat
\ No newline at end of file
+the pedigreed version of the traditional British domestic cat, with a distinctively stocky body, dense coat, and broad face
```

Now `amend` the previous commit:

```
git add british-shorthair.md
git commit --amend --no-edit
```

The `--no-edit` flag means we don't want to change the commit message. (Otherwise we could use the usual `-m` flag with a message.)

```
git log --oneline
```

```
848e66f (HEAD -> master) add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

The log is nice and clean, and no extra commit was created for our fix.

# Peek into the `.git` directory

`git` stores its internals in the `.git` directory:

```
ls -C .git
```

```
branches        config       HEAD   index  logs     ORIG_HEAD    refs
COMMIT_EDITMSG  description  hooks  info   objects  packed-refs
```

## Config

This file contains our repository specific config (as opposed to the `--global` config). Repository specific config will override the global config.

```
cat .git/config
```

```
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
```

## Tags, branches

```
ls -C .git/refs/heads
```

```
master
```

```
ls -C .git/refs/tags
```

```
first  v0.9.9  v1.0.0
```

```
cat .git/refs/tags/v1.0.0
```

```
d1f01ee42d4aa2e9c089906a74895335d6992938
```

The is a simple text file, which contains the hash `d1f01ee` the tag points to. For branches it is very similar, but we only have a single branch `master` for now.

## HEAD

```
cat .git/HEAD
```

```
ref: refs/heads/master
```

`HEAD` points to a specific reference at this point (which is `refs/heads/master`). As we have seen in earlier examples, this can be a specific commit as well.

## Object store

```
ls -C .git/objects
```

```
08  13  3c  59  84  ad  d1  e6  f7    pack
12  14  40  75  85  c0  d4  f0  info
```

Each directory contains compressed, binary objects.

To check the most recent commit `848e66f`:

```
git cat-file -t 848e66f
```

```
commit
```

```
git cat-file -p 848e66f
```

```
tree f7e0e09096a41c827388508ae25e26615b02abe0
parent d1f01ee42d4aa2e9c089906a74895335d6992938
author ... 
committer ... 

add british shorthair
```

The commit consists of a `parent`, `author` and a `tree` object.

We can poke into the tree object:

```
git cat-file -p f7e0e0
```

```
100644 blob 85a20e47efcabc127203eec9ae159b81f1bab95b	README.md
100644 blob d4312cc6f6b89205e236a1da76fa219b630a6469	bengal.md
100644 blob 12585d174e7c6a52bcdc732a4102136024a92463	british-shorthair.md
100644 blob 7569f85c39b5b7fd3a93b9a6e535c2c7a06f6276	maine-coon.md
100644 blob 59a6bfc5095207c9d9c8ea39cd81459ca0c132ab	ragdoll.md
```

And now the actual content of an object:

```
git cat-file -p d4312c
```

```
hybrid of Egyptian Mau and Asian leopard cat
```

# Branching

## Create a branch

One of the signature features of `git` is of course branching. Think about a branch as a split in the commit graph.

We decided to modify each cat file in the cat database to start with a title, which is set to the file name. For this task we create a new branch:

```
git checkout -b cat-title
```

`git checkout -b` is a shortcut for `git branch <name>` followed by a `git checkout <name>`.

```
git status
```

```
On branch cat-title
nothing to commit, working tree clean
```

Now we perform the file changes for our task (instead of using the below command you can edit and commit each file one-by-one of you like):

```
for f in !(README).md ; do sed -i "1s;^;# $f\n\n;" "$f" && git add "$f" && git commit -m "add title to $f" ; done
git log --oneline
```

```
88ad6cf (HEAD -> cat-title) add title to ragdoll.md
ef63f0b add title to maine-coon.md
eb12b84 add title to british-shorthair.md
c92edd5 add title to bengal.md
848e66f (master) add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Note, that the commit graph is still a single line. The `master` ref is now behind, while the `HEAD` and `cat-title` refs are pointing to the most recent commit `88ad6cf`.

## Merge the branch into `master`

Switch back to the `master` branch:

```
git checkout master
```

Now we merge `cat-title` into `master`:

```
git merge cat-title
```

```
Updating 6f0efd6..bce6cad
Updating files: 100% (4/4), done.
Fast-forward
 bengal.md            | 2 ++
 british-shorthair.md | 2 ++
 maine-coon.md        | 2 ++
 ragdoll.md           | 2 ++
 4 files changed, 8 insertions(+)
```

Since there were no (diverging) changes on `master`, the merge operation in this case was a trivial effort: git only had to make the `master` ref to point to the same commit. This is called a `fast-forward`.

## More merging

Let's undo our previous `fast-forward` merge on `master`:

```
git reset --hard 848e66f
git log --oneline
```

```
848e66f (master) add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Modify and commit the `README.md` file:

```
printf %b '\nCats love databases' >> README.md
git commit -a -m "updated README.md"
```

```
[master ef98780] updated README.md
 1 file changed, 2 insertions(+)
```

Let's inspect the `git log`:

```
git log --all --oneline --graph
```

```
* ef98780 (HEAD -> master) updated README.md
| * 88ad6cf (cat-title) add title to ragdoll.md
| * ef63f0b add title to maine-coon.md
| * eb12b84 add title to british-shorthair.md
| * c92edd5 add title to bengal.md
|/
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

To interpret the output, imagine that the `*` symbols are nodes, while the `|` and `/` symbols are branches of the tree.

Now we merge `master` into our feature branch:

```
git checkout cat-title
git merge master --no-edit
```

```
Merge made by the 'recursive' strategy.
 README.md | 2 ++
 1 file changed, 2 insertions(+)
```

```
git log --all --oneline --graph
```

```
*   02b96ac (HEAD -> cat-title) Merge branch 'master' into cat-title
|\
| * ef98780 (master) updated README.md
* | 88ad6cf add title to ragdoll.md
* | ef63f0b add title to maine-coon.md
* | eb12b84 add title to british-shorthair.md
* | c92edd5 add title to bengal.md
|/
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

This time `merge` created a new commit `02b96ac` on branch `cat-title` which contains all the changes that have been made on `master` since we diverged.

It is __important__ to remember this workflow as this is something you will frequently do when working on a feature-branch and you want to periodically update the state of your branch with the changes others do on the `master` branch.

## Even more merging - with conflicts

Let's roll back our previous merge on `cat-title`:

```
git reset --hard 88ad6cf
```

```
HEAD is now at 88ad6cf add title to ragdoll.md
```

Create a conflicting change in `README.md`:

```
echo "a database of cats" >> README.md
git commit -a -m "more to read"
git log --all --oneline --graph
```

```
* 3f82b00 (HEAD -> cat-title) more to read
* 88ad6cf add title to ragdoll.md
* ef63f0b add title to maine-coon.md
* eb12b84 add title to british-shorthair.md
* c92edd5 add title to bengal.md
| * ef98780 (master) updated README.md
|/
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

Time to merge `master` into `cat-title`:

```
git merge master
```

```
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
Automatic merge failed; fix conflicts and then commit the result.
```

```
git status
```

```
On branch cat-title
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

It tells us what are options are. So let's fix the conflicts:

```
cat README.md
```

```
# Cat Database
<<<<<<< HEAD
a database of cats
=======

Cats love databases
>>>>>>> master
```

This special notation tells us, that the current version on `HEAD` contains "a database of cats", while the incoming change on `master` contains a new line and "Cats love databases".

For more complex changes you will use a text editor to fix the conflict manually. For now the following will be sufficient:

```
sed -i '2,4d;$d' README.md
```

Commit the conflict resolution:

```
git add README.md
git commit -m "Merge 'master' into cat-title, fix conflict"
git log --oneline --graph --all
```

```
*   6dcd0fa (HEAD -> cat-title) Merge 'master' into cat-title, fix conflict
|\
| * ef98780 (master) updated README.md
* | 3f82b00 more to read
* | 88ad6cf add title to ragdoll.md
* | ef63f0b add title to maine-coon.md
* | eb12b84 add title to british-shorthair.md
* | c92edd5 add title to bengal.md
|/
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

# Customizing your git experience

## Define aliases

One way to define aliases is to use the `git config` command:

```
git config --global alias.st 'status -sb'
```

An alternative way is to simply edit `$HOME/.gitconfig` with `git config --global -e`.


Here is a recommended set of commonly used git aliases:

```
[alias]
   st = status -sb
   ll = log --oneline
   lg = log --all --oneline --graph
   last = log -1 HEAD --stat
   cm = commit -m
   co = checkout
   cb = checkout -b
   br = branch
   dv = 'difftool -t vimdiff -y'
   p = push
   up = pull --rebase --prune
   rv = remote -v
   ec = config --global -e
   am = commit -a --amend
```

## Command Autocorrect

```
git config --global help.autocorrect 20
```

```
git stats
```

```
WARNING: You called a Git command named 'stats', which does not exist.
Continuing in 2.0 seconds, assuming that you meant 'status'.

...
```

# Rebasing

Make sure you are on branch `cat-title` and reset the branch to its pre-merge state again:

```
git reset --hard 88ad6cf
```

```
git lg
```

```
* ef98780 (master) updated README.md
| * 88ad6cf (HEAD -> cat-title) add title to ragdoll.md
| * ef63f0b add title to maine-coon.md
| * eb12b84 add title to british-shorthair.md
| * c92edd5 add title to bengal.md
|/
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

Now we will use `rebase` to synchronize the commit in `master` into `cat-title`.

```
git rebase master
```

```
Successfully rebased and updated refs/heads/cat-title.
```

```
git lg
```

```
* dc9ee3f (HEAD -> cat-title) add title to ragdoll.md
* 9ffbed0 add title to maine-coon.md
* 28f0d5c add title to british-shorthair.md
* 85e1f66 add title to bengal.md
* ef98780 (master) updated README.md
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

Note, that `rebase` resulted in a flat graph. The commit history is identical to what we would get if the commit to `README.md` in branch `master` happened _before_ we branched `cat-title` and added our title related commits to it.

Merging back `cat-title` to `master` now will result in a `fast-forward` merge. This simply moves the branch refs and avoids creating a merge commit.

```
git co master
git merge cat-title
```

```
Updating ef98780..dc9ee3f
Fast-forward
 bengal.md            | 2 ++
 british-shorthair.md | 2 ++
 maine-coon.md        | 2 ++
 ragdoll.md           | 2 ++
 4 files changed, 8 insertions(+)
```

The `master` and `cat-title` branches are now identical, so we can safely delete the feature branch:

```
git branch -d cat-title
```

__Warning__: `rebase` changes history. While having a flat commit graph is generally desirable, do not `rebase` a branch that has already been shared with others.

As a general rule of thumb you can use `rebase` for short-lived, local branches and `merge` for branches in a shared repository.

# Repo-repo collaboration

`git` has built-in support for creating `clone`s (copies) of repositories and synchronizing changes between them. Copies are usually accessed through the network (but can be local as well).

Before proceeding, create a new branch in `git-tutor` with a commit:

```
git cb persian
```

```
echo "originally from Persia, this breed has an impressive long silky coat. Introduced in Europe in the 1500s as highly valued items of trade." > persian.md
```

```
git add persian.md
git cm "add persian"
```

```
0c49c5d (HEAD -> persian) add persian
dc9ee3f (master) add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Switch back to `master`:

```
git co master
```

## Clone the `git-tutor` repository

Use the `git clone` command to create a copy. Execute the command from the _parent directory_ of `git-tutor`:

```
cd ..
git clone git-tutor git-tutor-clone
```

```
Cloning into 'git-tutor-clone'...
done.
```

Investigate the git log of the newly created clone:

```
cd git-tutor-clone
git lg
```

```
* 0c49c5d (origin/persian) add persian
* dc9ee3f (HEAD -> master, origin/master, origin/HEAD) add title to ragdoll.md
* 9ffbed0 add title to maine-coon.md
* 28f0d5c add title to british-shorthair.md
* 85e1f66 add title to bengal.md
* ef98780 updated README.md
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

The cloned repository has additional refs `origin/master`, `origin/persian` and `origin/HEAD`, which indicate the state of these pointers in the original repository.

## Remotes

The cloned repository is linked to the original repository.

Use the `git remote` command to show such links:

```
git remote -v
```

```
origin  /.../git-tutor (fetch)
origin  /.../git-tutor (push)
```

```
git remote show origin
```

```
* remote origin
  Fetch URL: /.../git-tutor
  Push  URL: /.../git-tutor
  HEAD branch: master
  Remote branch:
    master tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

This tells git exactly where to synchronize changes from/to, and which branches are mapped to which ones on the original repository.

## Remote branches

To see _all_ branches, including remote branches use the `-a` flag:

```
git branch -a
```

```
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/persian
```

In the cloned repository currently only `master` is available locally.

## Fetch

First go back to the original `git-tutor` repository and add a change:

```
cd ../git-tutor
echo "...except when they don't" >> README.md
git cm "fixed README" -a
```

Now compare the log of the original and the clone repositories:

```
git ll
```

```
073f03a (HEAD -> master) fixed README
dc9ee3f add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

```
cd ../git-tutor-clone
git ll
```

```
dc9ee3f (HEAD -> master, origin/master, origin/HEAD) add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

The cloned repository is not yet aware of the newly added commit.

In order to obtain the new commit, we need to use `fetch`:

```
git fetch
```

```
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 309 bytes | 309.00 KiB/s, done.
From /home/garric/projects/git-tutor
   dc9ee3f..073f03a  master     -> origin/master
```

```
git ll --all
```

```
073f03a (origin/master, origin/HEAD) fixed README
0c49c5d (origin/persian) add persian
dc9ee3f (HEAD -> master) add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

The cloned repository now has the latest commits, but the local `master` ref is behind `origin/master`.

We have encountered this scenario earlier, when looking at merging (this is a `fast-forward` merge):

```
git merge origin/master
```

```
Updating dc9ee3f..073f03a
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```
git ll
```

```
073f03a (HEAD -> master, origin/master, origin/HEAD) fixed README
dc9ee3f add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

## Pull

`git fetch` followed by `git merge` is such a common sequence of operations, that we have a separate command `git pull` for that.

To try it out, reset the cloned repository to its previous state:

```
git reset --hard dc9ee3f
```

Also remove and re-add the remote to delete the tracking refs:

```
git remote remove origin
git remote add origin ../git-tutor
git branch --set-upstream-to=origin/master master
```

Now our cloned repository should be in the same state as before we performed the `fetch` + `merge`:

```
git ll --all
```

```
073f03a (origin/master) fixed README
0c49c5d (origin/persian) add persian
dc9ee3f (HEAD -> master) add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Now perform the `pull`:

```
git pull
```

```
Updating dc9ee3f..073f03a
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

```
073f03a (HEAD -> master, origin/master) fixed README
0c49c5d (origin/persian) add persian
dc9ee3f add title to ragdoll.md
9ffbed0 add title to maine-coon.md
28f0d5c add title to british-shorthair.md
85e1f66 add title to bengal.md
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

## Tracking branches

In the previous section we got a glimpse of _tracking branches_ when we have restored the cloned repository state to before `fetch` and `merge`.

Add a local branch `persian` that tracks the remote branch `origin/persian`:

```
git branch --track persian origin/persian
```

```
Branch 'persian' set up to track remote branch 'persian' from 'origin'.
```

```
git branch -a
```

```
* master
  persian
  remotes/origin/master
  remotes/origin/persian
```

```
git lg
```

```
* 073f03a (HEAD -> master, origin/master) fixed README
| * 0c49c5d (origin/persian, persian) add persian
|/
* dc9ee3f add title to ragdoll.md
* 9ffbed0 add title to maine-coon.md
* 28f0d5c add title to british-shorthair.md
* 85e1f66 add title to bengal.md
* ef98780 updated README.md
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

# Bare repositories

A `bare` repository has no working directory, and only includes the `.git` metadata. They are mostly used for repository to repository collaboration as a _central_ repository.

Convert the original `git-tutor` repository to a bare repository:

```
cd ..
git clone --bare git-tutor git-tutor.git
rm -rf git-tutor
```

Bare repositories end with `.git` by convention.

```
cd git-tutor.git
ls -C
```

```
branches  config  description  HEAD  hooks  info  objects  packed-refs  refs
```

# Setting remotes

Now set the remote `origin` of `git-tutor-clone` to the newly created bare repository:

```
cd ../git-tutor-clone
git remote remove origin
git remote add origin ../git-tutor.git
```

# Push

`push` distributes changes from a branch in a repository to another repository.

Merge the `persian` branch into `master` and push the change (you need to also set the `origin/master` as upstream branch to `master` since we deleted and readded the remote in the previous section):

```
git push --set-upstream origin master
```

```
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 12 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 294 bytes | 294.00 KiB/s, done.
Total 2 (delta 1), reused 0 (delta 0), pack-reused 0
To ../git-tutor.git
   073f03a..d436400  master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

# Pull with rebase

Create another clone of the `git-tutor.git` bare repository:

```
cd ..
git clone git-tutor.git git-tutor-clone2
cd git-tutor-clone2
```

Make some commits:

```
echo "a breed of domestic short-haired cat with a distinctive 'ticked' tabby coat, in which individual hairs are banded with different colors" >> abyssinian.md
git add abyssinian.md
git cm "add abyssinian"
```

```
sed -i "1s;^;# abyssinian.md\n\n;" abyssinian.md
git cm "add title to abyssinian.md" -a
```

```
git ll
```

```
d9ab67e (HEAD -> master) add title to abyssinian.md
8c7148d add abyssinian
d436400 (origin/master, origin/HEAD) Merge branch 'persian'
073f03a fixed README
<...>
```

Push the commits for abyssinian to `git-tutor`:

```
git push
```

Now make a commit in the `git-tutor-clone` repository:

```
cd ../git-tutor-clone
```

```
sed -i "1s;^;# persian.md\n\n;" persian.md
git cm "add title to persian.md" -a
```

```
git ll
```

```
a967376 (HEAD -> master) add title to persian.md
d436400 (origin/master) Merge branch 'persian'
073f03a fixed README
<...>
```

Now `pull` the changes with `rebase`:

```
git pull --rebase
```

```
remote: Enumerating objects: 7, done.
remote: Counting objects: 100% (7/7), done.
remote: Compressing objects: 100% (6/6), done.
remote: Total 6 (delta 3), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (6/6), 622 bytes | 622.00 KiB/s, done.
From ../git-tutor
   d436400..d9ab67e  master     -> origin/master
Successfully rebased and updated refs/heads/master.
```

Now inspect the history. Note, how the commits made to `git-tutor-clone2` were rebased into the history, appearing like they were made _before_ the commit which adds abyssinian.

```
git ll
```

```
ef95b8f (HEAD -> master) add title to persian.md
d9ab67e (origin/master) add title to abyssinian.md
8c7148d add abyssinian
d436400 Merge branch 'persian'
073f03a fixed README
<...>
```

# Delete remote branch

Now delete the `persian` branch in the remote repository:

```
git push origin --delete persian
```

```
To ../git-tutor.git
 - [deleted]         persian
```

This will not delete the local branch though. To delete the local branch as well:

```
git branch --delete persian
```

```
git branch --all
```

```
* master
  remotes/origin/master
```

# Tampering with history with interactive rebase

__Warning:__ heed the usual consequences of changing history in a shared repository.

We will now review what is possible with interactive rebase. Let's rebase the `master` branch using an earlier commit from the history:

```
git rebase -i ef98780
```

At this point `git` will open our defult editor, and we can make changes to the _command_ verb before each commit.

```
pick 85e1f66 add title to bengal.md
pick 28f0d5c add title to british-shorthair.md
pick 9ffbed0 add title to maine-coon.md
pick dc9ee3f add title to ragdoll.md
pick 073f03a fixed README
pick 0c49c5d add persian
pick 8c7148d add abyssinian
pick d9ab67e add title to abyssinian.md
pick ef95b8f add title to persian.md

# Rebase ef98780..ef95b8f onto ef98780 (9 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
```

Some commonly used commands are:
   * `pick`: leave commit as it is
   * `squash`: combine multiple commits
   * `reword`: change the commit message
   
   
Let's edit the buffer to look like this:

```
pick 85e1f66 add title to bengal.md
s 28f0d5c add title to british-shorthair.md
s 9ffbed0 add title to maine-coon.md
s dc9ee3f add title to ragdoll.md
d 073f03a fixed README
pick 0c49c5d add persian
s 8c7148d add abyssinian
s d9ab67e add title to abyssinian.md
s ef95b8f add title to persian.md
```

Save and exit the editor. The editor will be opened again, with a buffuer to edit the squashed commit message for the first 4 commits. Alter it to look like this:

```
add titles
```

Save, exit, and repeat for the next 4 commits:

```
add 2 more cats
```

If you were successful, you will see the rebase completed message:

```
[detached HEAD 947de88] add titles
 Date: Fri Apr 9 20:06:08 2021 +0200
 4 files changed, 8 insertions(+)
[detached HEAD ef72f03] add 2 more cats
 Date: Sun Apr 11 12:03:19 2021 +0200
 2 files changed, 6 insertions(+)
 create mode 100644 abyssinian.md
 create mode 100644 persian.md
Successfully rebased and updated refs/heads/master.
```

Check the history after rebasing:

```
git ll
```

```
ef72f03 (HEAD -> master) add 2 more cats
947de88 add titles
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

The procedure is practically the same for using the other commands (`reword`, `drop`, ...)
If you are stuck or unsure what happened, you can always abort an in-progress rebase and return to the original state with:

```
git rebase --abort
```

Pushing after running a rebase which changed history will normally be rejected:

```
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to '../git-tutor.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

To force the remote to accept our version of the history and overwrite its own:

```
git push --force
```

```
Enumerating objects: 15, done.
Counting objects: 100% (15/15), done.
Delta compression using up to 12 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (10/10), 1.13 KiB | 1.13 MiB/s, done.
Total 10 (delta 2), reused 0 (delta 0), pack-reused 0
To ../git-tutor.git
 + d9ab67e...ef72f03 master -> master (forced update)
```

```
git ll
```

```
ef72f03 (HEAD -> master, origin/master) add 2 more cats
947de88 add titles
ef98780 updated README.md
848e66f add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

# Cherry-picking

Cherry-picking allows arbitrary commits to be picked and appended to the current working `HEAD`.

Assume we create the `release` branch in the `git-tutor-clone2` repository as follows:

```
cd ../git-tutor-clone2
git lg
```

```
* d9ab67e (HEAD -> master, origin/master, origin/HEAD) add title to abyssinian.md
* 8c7148d add abyssinian
*   d436400 Merge branch 'persian'
|\
| * 0c49c5d (origin/persian) add persian
* | 073f03a fixed README
|/
* dc9ee3f add title to ragdoll.md
* 9ffbed0 add title to maine-coon.md
* 28f0d5c add title to british-shorthair.md
* 85e1f66 add title to bengal.md
* ef98780 updated README.md
* 848e66f add british shorthair
* d1f01ee (tag: v1.0.0) cat breeds
* ad0e552 (tag: v0.9.9, tag: first) first commit
```

```
git checkout v1.0.0
git switch -c work
git ll
```

```
d1f01ee (HEAD -> work, tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Now cherry pick a few commits to `work`:

```
git cherry-pick 848e66f 0c49c5d 8c7148d
```

```
[work 54da427] add british shorthair
 Date: Fri Apr 9 19:44:52 2021 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 british-shorthair.md
[work 23efb85] add persian
 Date: Sun Apr 11 12:03:19 2021 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 persian.md
[work d0c13d2] add abyssinian
 Date: Sun Apr 11 14:10:37 2021 +0200
 1 file changed, 1 insertion(+)
 create mode 100644 abyssinian.md
```

```
git tag v1.1.0
git ll
```

```
d0c13d2 (HEAD -> work, tag: v1.1.0) add abyssinian
23efb85 add persian
54da427 add british shorthair
d1f01ee (tag: v1.0.0) cat breeds
ad0e552 (tag: v0.9.9, tag: first) first commit
```

Note, that the cherry-picked commits have different hashes - they are technically new commits, just with the same message and content.
