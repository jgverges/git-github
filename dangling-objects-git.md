

## Dangling objects in the repository

Dangling object - is the one that's not referenced by anything:

- Dangling blob = A change that made it to the staging area/index, but never got committed. 

- Dangling commit = A commit that isn't directly linked to by any child commit, branch, tag or other reference. 

You can delete or get them back.

### Verification of basic information about commits and dangling blobs.

```
git fsck
```

- If you gets this result you repo does not have dungling objects:

```
Checking object directories: 100% (256/256), done.
Checking objects: 100% (16396/16396), done.
```
 

- If you gets something like this you have to handle it:

```
dangling blob 91ff5fc4ad27e73cd1937dfda304708d31660956
dangling commit 96ffeaa7caefcf649f5b864b516e7314b24cbe8c
Verifying commits in commit graph: 100% (2603/2603), done.
```
## Get back a single object

Create a new branch form the SHA you want to recover (example: SHA 91ff5fc4)

```
$ git branch recover-branch 91ff5fc4
```

### Fixing All Dangling commits

https://stackoverflow.com/questions/67630383/git-gc-and-git-prune-warnings-when-git-fetch-origin-is-run

````
$ git fsck
$ git gc --prune=now
$ git prune  
$ git gc

````

 ### Warning: There are too many unreachable loose objects
 
 Sometimes using git  you obtain this message in console
 
````
warning: The last gc run reported the following. Please correct the root cause
and remove .git/gc.log
Automatic cleanup will not be performed until the file is removed.

warning: There are too many unreachable loose objects; run 'git prune' to remove them.

````
This means you have too many dangling commits for git to automatically clean up.

## Git — There are too many unreachable loose objects

https://medium.com/lynns-dev-blog/git-there-are-too-many-unreachable-loose-objects-c2df601b8001
