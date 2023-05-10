# git-github
Git and Github informations, Tests and docs


## Errors git

https://www.youtube.com/watch?v=9sK_is8ufbk

https://pabpereza.dev/docs/programacion/git/solucionar_errores/

## Limpiar las ramas obsoletas

git fetch --prune

es la mejor utilidad para limpiar ramas obsoletas. Se conecta a un repositorio remoto compartido y recupera todas las referencias a ramas remotas. A continuación, elimina las referencias remotas que ya no se usan en el repositorio remoto.

## Gitignore not working

https://stackoverflow.com/questions/25436312/gitignore-not-working

The files/folder in your version control will not just delete themselves just because you added them to the .gitignore. They are already in the repository and you have to remove them. You can just do that with this:

**Remember to commit everything you've changed before you do this!**

````
git rm -rf --cached .
git add .
````

This removes all files from the repository and adds them back (this time respecting the rules in your .gitignore).

## How to have a different .gitignore for each branch (or an alternative solution)?

https://stackoverflow.com/questions/56390952/how-to-have-a-different-gitignore-for-each-branch-or-an-alternative-solution

## Git : using .gitignore with different branches

https://stackoverflow.com/questions/6057419/git-using-gitignore-with-different-branches

## git merge and different gitignore for each branch

https://stackoverflow.com/questions/24649584/git-merge-and-different-gitignore-for-each-branch

## Separate git branches with different .gitignores for production and development

https://stackoverflow.com/questions/52891650/separate-git-branches-with-different-gitignores-for-production-and-development

