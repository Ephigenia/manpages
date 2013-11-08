# SVN Migration

## Get Information about the repository:

* Does it use the standard directories for branches and tags?
* How many revisions, tags and branches are there?
* Are there Externals used?

## Creating the Authors file

Commits in git don’t only store the username of the commiter but the email in addition. In order to have a nice-looking git history with real usernames and email addresses we need to tell `git svn` a mapping for each user who ever commited to the svn repo. Most of the `git svn` commands accept the `--authors-file=` parameter with a mapping of authos in it.

	svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-mapping.txt

After the file is created you need to modify it. You can skip this step but the result will be a git repo with usernames that won’t properly fit to your git usernames.

	vi authors-mapping.txt

	oldusername = newusername <email@host.tld>

## SVN GIT Clone

Single project svn repository

	git svn clone <clone-url> --no-metadata -A <authors-mapping-file> --stdlayout <target-directory>

Multiple projects in a single svn repository. The `clone-url` should point to the root of the project (the one with `trunk`, `branches` in it.):
	
	git svn clone <clone-url-project-root> --no-metadata --authors-file=<authors-mapping-file> --stdlayout <target-directory>

Different non-standard `trunk`, `branches` and `tags` directories:

	git svn clone <clone-url> --no-metadata -A <authors-mapping-file> -t tags -b branches -T trunk <target-directory>

If your repository contains tags for each build or even branches you don't want to include in the migration you can exclude them by adding them to the `ignore-paths` parameter when running `git svn clone`. The paths have to be absolute paths to the root of the repository. [Stackoverflow](http://stackoverflow.com/questions/7668752/git-svn-ignore-paths)

	--ignore-paths=/branches/foo/jars

## Branches and Tags

First create a list of all branches

	git branch -r

Convert all Tags into git tags and remove tag-branch from remote

	git tag <new-tag-name> <old-tag-name>
	git branch -r -d <old-tag-name>

## Shorter version

	git for-each-ref --format='%(refname)' refs/heads/tags |
	cut -d / -f 4 |
	while read ref
	do
	  git tag "$ref" "refs/heads/tags/$ref";
	  git branch -D "tags/$ref";
	done

Convert all remaining branches to local branches

	cp -Rf .git/refs/remotes/* .git/refs/heads/
	rm -Rf .git/refs/remotes

Check branches and tags

	git tag
	git branch -a

## Rename "trunk" to "master"

	git branch -m trunk master

## SVN ignore

	git svn show-ignore > .gitignore
	git add .gitignore
	git commit -m 'Convert svn:ignore properties to .gitignore'

## Remove Passwords

If the SVN repository contains sensitive information like passwords or keys you **should change them all** and remove the files containing them completely from the git history after you migrated the repository.

	http://stackoverflow.com/questions/872565/remove-sensitive-files-and-their-commits-from-git-history

	git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch [filename-to-delete]' --prune-empty --tag-name-filter cat -- --all

Don’t forget to set this file on `.gitignore` after deleting it from the history otherwise you may check in the file again!

## Commit-Hooks

How to handle git commit hoocks?

## Submodules



[1] http://john.albin.net/git/convert-subversion-to-git