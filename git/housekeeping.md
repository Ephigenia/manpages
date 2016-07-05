From time to time you’ll need to delete old unused, allready merged branches.

### List merged branches

    git branch --merged
    
In most cases you can delete all the merged branches with `git branch -d <branchname>`
    
### List unmerged branches

    git branch --no-merged

Sometimes there are several unmerged branches for different reasons. They may be of experimental nature not pushed to the remote yet. You’ll need to decide on your own what to do with them.
    
### Remove Old Remote Branches

The remote branches will remain on their remote location untill you delete them one by one with `git branch push --delete origin <branch-name>` or just remove all which are not available locally:

    git remote prune origin
    
You can iterate over all branches and remove them programatically:

    for branch in $(git branch --merged); do if [ "$branch" != "*" ] && [ "$branch" != "develop" ]; then git branch -d $branch; elif; done;
    
An also on remote:

    git branch -r --merged | grep -v visono | grep -v upstream | grep -v HEAD | sed 's/origin\///' | xargs -n 1 git push --delete origin
