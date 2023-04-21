## What is “dangling commit” & “dangling blob” ?

Dangling objects that exist but that are never directly used.

### Dangling blob

A change that made it to the staging area/index but never got committed.

### Dangling commit

A commit that isn’t directly linked to by any child commit, branch, tag or other reference.

## gc reporting

 $  **git fetch**
 
````

Auto packing the repository in background for optimum performance.
See "git help gc" for manual housekeeping.
warning: The last gc run reported the following. Please correct the root cause
and remove .git/gc.log
Automatic cleanup will not be performed until the file is removed.

warning: There are too many unreachable loose objects; run 'git prune' to remove them.

````

### Solution

https://stackoverflow.com/questions/67630383/git-gc-and-git-prune-warnings-when-git-fetch-origin-is-run

````
$ git fsck
$ git gc --prune=now
$ git prune  
$ git gc

````
## Git — There are too many unreachable loose objects

https://medium.com/lynns-dev-blog/git-there-are-too-many-unreachable-loose-objects-c2df601b8001
