# IMPORTANT

Since git 2.44 there is a
[`git replay`](https://github.com/git/git/blob/3c2a3fdc388747b9eaf4a4a4f2035c1c9ddb26d0/Documentation/RelNotes/2.44.0.txt#L19)
builtin and I do not want it to be confused with the script I am
providing here _therefore_ I think it makes sense that I rename
this project to something different so there is no confusion.

I haven't picked up a name yet but I am leaving a note here so you
know what my intent is at the moment (to be taken care of shortly).

Once I pick up a name and setup the project there, I will adjust
this `README` to point to the new location so people who used the
links to **this** project that have been provided here or there can
still find the new one.

Thanks for reading... and perhaps using the project.

Edmundo
March 2nd, 2024

# usage

This script can be used in cases when we want to _replay_
revisions on top of another branch that has the same tree
when the revisions we want to replay are not linear.
Look at this example:

```
$ git checkout v2.35.0
# create a revision that has the exact same tree as v2.35.0
$ git commit --amend --no-edit
# Replay all revisions between v2.35.0 and v2.36-rc0
$ git rebase --onto HEAD v2.35.0 v2.36.0-rc0 --rebase-merges
.
.
.
Could not apply 5d01301f2b... Sync-with-Git-2-35-1 # Sync with Git 2.35.1
$ git status
interactive rebase in progress; onto 9c7bc0e364
Last commands done (247 commands done):
   pick 90fb70e458 Name the next one 2.36 to prepare for 2.35.1
   merge -C 5d01301f2b865aa8dba1654d3f447ce9d21db0b5 Sync-with-Git-2-35-1 # Sync with Git 2.35.1
  (see more in file /home/antoranz/proyectos/git/git/.git/worktrees/master/rebase-merge/done)
Next commands to do (982 remaining commands):
   label branch-point
   pick 5e00514745 t1405: explictly delete reflogs for reftable
  (use "git rebase --edit-todo" to view and edit)
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)
Changes to be committed:
        new file:   Documentation/RelNotes/2.35.1.txt
Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   GIT-VERSION-GEN
        both modified:   RelNotes
```

Rebase is using the merge engine to replay all the revisions, for understandable reasons.
`git-replay.py` would instead recreate all the original revisions on top of the desired
point (doesn't have to be `HEAD`) without running any actual merge.

Technically speaking, the script will create new revisions using the same metadata from the
original revisions, except that it would change the parent IDs and the committer.

When you use this script, it won't move anything from the local repo, it will only
create commits as requested and, when it is finished, it will write the commit ID of the
tip of the resulting rebased/replayed branch, much the same way git-commit-tree behaves.

```
$ ./git-replay.py HEAD v2.35.0 v2.36.0-rc0
It might not look like it but...
I am working. Check CPU and disk usage.
f0b7663aa1b6e009e27c185b89ad88f683d773aa
```

# copyright/license

Copyright (c) 2022, 2023 Edmundo Carmona Antoranz

Released under the terms of GPLv2
