# SVN Migration

## Get Information about the repository:

Before you start you should gather some information about the svn repository that will help you through the process. Especially the age and revision numbers will give you an idea of how long the migration process or the commands will run. Expect > 10 Minutes for > 1000 Revisions.

* Does it use the standard directories for branches and tags?
  You can see all directories (aka) branches with `svn ls <url>`
* How many revisions, tags and branches are there and how old is the repository?
* Are there Externals used?

### Helpful commands

Count number of branches and tags:

	svn ls <url>/branches | wc -l
	svn ls <url>/tags | wc -l

"short-log" of revisions inside a directory:

	svn log <url> | grep ^r[0-9]

Number of revisions:

	svn log <url> | grep ^r[0-9] | wc -l

Estimate the size and number of files in a tree:

	svn list -vR <url> | awk '{if ($3 !="") sum+=$3; i++} END {print "\nsize: " sum/1024000" MB" "\nfiles: " i}'

Get a list of externals used:

	svn propget svn:externals -R <url>

Get date of first commit in a tree:

	svn log -r 1:HEAD --limit 1 <url> | grep r[0-9]

## Creating the Authors file

As commits in SVN store the username of the commiter, git also stores their e-mail adress. So we will have to add the email adresses to all commits when we migrate the history. We can create a list of all authors in the svn repo by using the `svn log` command:

	svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-mapping.txt

This will take some time. After the file was created, open it with an editor and modify the mappings so that the new authors all have an email address.

	vi authors-mapping.txt

	oldusername = newusername <email@host.tld>

This file will be used by `git svn` in the `--authors-file` parameter so that git knows which svn commit belongs to which git user.

## SVN GIT Clone

Single project svn repository

	git svn clone <clone-url> --no-metadata --authors-file=<authors-mapping-file> --stdlayout <target-directory>

Multiple projects in a single svn repository. The `clone-url` should point to the root of the project (the one with `trunk`, `branches` in it.):
	
	git svn clone <clone-url-project-root> --no-metadata --authors-file=<authors-mapping-file> --stdlayout <target-directory>

Different non-standard `trunk`, `branches` and `tags` directories:

	git svn clone <clone-url> --no-metadata -A <authors-mapping-file> -t tags -b branches -T trunk <target-directory>

If your repository contains tags for each build or even branches you don't want to include in the migration you can exclude them by adding them to the `ignore-paths` parameter when running `git svn clone`. The paths have to be absolute paths to the root of the repository. [Stackoverflow](http://stackoverflow.com/questions/7668752/git-svn-ignore-paths)

	--ignore-paths=/branches/foo/jars

## Tags

First create a list of all branches

	git branch -r

Convert all Tags into git tags and remove tag-branch from remote

	git tag <new-tag-name> <old-tag-name>
	git branch -r -d <old-tag-name>

You can automate the tags-to-branch-conversion with this simple shell script which iterates over all remote tags (coming from svn) and runs the two conversion commands on each:

	git for-each-ref --format='%(refname)' refs/remotes/tags |
	cut -d / -f 4 |
	while read ref
	do
	  git tag "$ref" "refs/remotes/tags/$ref";
	  git branch -D "tags/$ref";
	done

After you’ve converted all svn tag branches to real local tags you can check the list of finished tags with:

	git tag

### Push Branches to remote origin

The tags are not synced to the remote repository and should be pushed to it. Selective push of single tags to remote:

	git push origin <tag-name>

## Branches

Converting specific branches to local branches:
	
	git checkout -b <new-branch-name> remotes/<old-tag-name>

This task can also be automated by using the `git for-reach-ref` command and grep for the branches you want to convert and run the previous command in the loop:

	git for-each-ref --format='%(refname)' refs/remotes/ | grep "[0-9]-*"
	cut -d / -f 4 |
	while read ref
	do
	  git checkout -b "$ref" remote/$ref
	done

If you’re able to convert all remaining branches (you’ve deleted the `tag`-branches?) to local branches you can use a simple shortcut that basically changes the references of remote to local:

	cp -Rf .git/refs/remotes/* .git/refs/heads/
	rm -Rf .git/refs/remotes

After that you check if there are branches or tags missing that have to get converted too:

	git branch

## Rename "trunk" to "master"

SVN does not know anything about a "master"-branch and only knows it’s "trunk" so we’ll have to convert that too:

	git branch -m trunk master

## SVN ignore

SVN ignores can be specified in every directory in their svn-properties. In git there’s the `.gitignore`. So we’ll need to convert it using `git svn show-ignore` command which will also handle the resursion.

	git svn show-ignore > .gitignore
	git add .gitignore
	git commit -m 'Convert svn:ignore properties to .gitignore'

## Remove Passwords

If the SVN repository contains sensitive information like passwords or keys you **should change them all** and remove the files containing them completely from the git history after you migrated the repository.

	git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch [filename-to-delete]' --prune-empty --tag-name-filter cat -- --all

Don’t forget to set this file on `.gitignore` after deleting it from the history otherwise you may check in the file again!

## Commit-Hooks

All commit hooks can be used in git too. See the documentation on [Missing git hooks documentation](http://longair.net/blog/2011/04/09/missing-git-hooks-documentation/) and read [this tutorial](http://css.dzone.com/articles/all-git-hooks-you-need) or see this example: [PHPCS pre commit hook](https://github.com/s0enke/git-hooks/tree/master/phpcs-pre-commit).

## Submodules

Submodules must be replaced with git submodules or required by an other dependency management tool like composer or something similar.