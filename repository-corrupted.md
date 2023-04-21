# Fix Reporitory

## How can I fix a corrupted Git repository?

https://stackoverflow.com/questions/18678853/how-can-i-fix-a-corrupted-git-repository

Nearly all answers assume one can simply re-clone from some uncorruptible remote origin. Here's the problem...
What if you are the origin, and you're corrupted? Right. So, here: 

`git-repair` 

https://manpages.ubuntu.com/manpages/focal/man1/git-repair.1.html

http://git-repair.branchable.com/

is a program that will run

`git fsck` 

and try pretty hard to fix any problems it encounters.

[ git-repair.branchable.com](https://git-repair.branchable.com/) 

It seems quite capable, and though you might end up having to copy (if you can!) objects from a backup (you have a backup, right?), it should save you a lot of time by salvaging whatever it can and leaving you the real work, not lots of automatable tasks. No affiliation, etc. – 
underscore_d


## git filter-branch

https://git-scm.com/docs/git-filter-branch

  
Ideas for `git-fix` project
===========================

https://gist.github.com/mhagger/01bf5485470bc5202623


We would like to have a way to fix up repositories' history; e.g., to remove corruption that may have happened some time in the past.

The standard tool for mass-rewriting of Git history is `git filter-branch`.  But it is awkward to use and has a number of limitations:

* It is a shell script, and correspondingly slow
* It cannot deal with some kinds of corruption, because it tries to check out all historical revisions into the index and/or working tree
* It can make "grafts" and "replace references" permanent, but only at the commit level.  It cannot make "replace references" for trees or blobs permanent.

Fixing repository corruption has three steps: detection and characterization of the corruption, creation of replacement objects, and history rewriting to make the repairs permanent.  Here is some brainstorming about how each of these stages could be improved:

1.  Detection of problems
    * Improve `git fsck` output (or maybe build this into another program, like `git rev-list --objects`).
      * Support a traversal-based check, whose range can be defined (ideally, also at object granularity).
      * Display the paths that led to broken objects.
    * Add new validity tests (to the extent that they don't already exist):
      * Prohibit empty filenames in trees.
      * Require filenames in trees to be distinct and sorted correctly.
      * Optionally require tree filenames to be unique even modulo Unicode normalization.
      * Optionally require tree filenames to be valid UTF-8 and normalized in form NFC.
    * `git fsck --interactive`: allow objects to be fixed while `git fsck` is running, to avoid having to restart all of the time (see below).
2.  Creation of replacement objects
    * Make `git hash-object` stricter by default.  It should run all of the object tests that `git fsck` would usually run.  Add a --no-strict mode for backwards compatibility.
    * Add `git hash-object --overwrite` command, which writes a new loose object regardless of whether an object with that name already exists.  (This could be used for replacing corrupt objects.)
    * Add `git hash-object --batch`, for efficiency.  Its input should be similar to the output of `git cat-file --batch`.
    * Use `git ls-tree -z` and `git mktree -z` for trees instead of `git cat-file` and `git hash-object`.
    * Add `git ls-tree --batch`, for efficiency.
3.  `git-fix`: Rewrite history, incorporating replace objects
    * Allow fixing all of the history reachable from a single tip
    * Allow references to be fixed
    * Handle fixing of only part of the history (e.g., via `<rev-list options>` like those of `git filter-branch`).
    * Handle tags.  Replace signed tags with unsigned tags where necessary.


## `git fsck --interactive`

Another idea was to implement `git fsck --interactive`, in which every time it found a problem, it would report the bad object in some machine-readable format, and wait for commands on stdin.  The commands could be things like

    replace SP <SHA-1> LF

to use as a replacement for the bad object (fsck would create a replace record but not do healing up the tree).

    add SP <type> SP <size> LF
    <contents> LF

to add a new object to the DB.

    replace SP <type> SP <size> LF
    <contents> LF

to add a new object to the DB and use it as a replacement for the bad object.

    continue LF

to just go on without repairing the broken object.

The point of all this would be to avoid the "fsck; fix reported problem; repeat" loop that is so expensive when there is a chain of bad objects.
