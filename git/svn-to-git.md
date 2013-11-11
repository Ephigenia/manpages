# SVN Migration

## Get Information about the repository:

Before you start you should gather some information about the svn repository that will help you through the process.

* Does it use the standard directories for branches and tags?
* How many revisions, tags and branches are there?
* Are there Externals used?

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

## Branches and Tags

First create a list of all branches

	git branch -r

Convert all Tags into git tags and remove tag-branch from remote

	git tag <new-tag-name> <old-tag-name>
	git branch -r -d <old-tag-name>

You can automate the tags-to-branch-conversion with this simple shell script:

	git for-each-ref --format='%(refname)' refs/heads/tags |
	cut -d / -f 4 |
	while read ref
	do
	  git tag "$ref" "refs/heads/tags/$ref";
	  git branch -D "tags/$ref";
	done

Convert all remaining remote svn-branches to local branches

	cp -Rf .git/refs/remotes/* .git/refs/heads/
	rm -Rf .git/refs/remotes

After that you check if there are branches or tags missing that have to get converted too:

	git tag
	git branch -a

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

	http://stackoverflow.com/questions/872565/remove-sensitive-files-and-their-commits-from-git-history

	git filter-branch --force --index-filter 'git rm --cached --ignore-unmatch [filename-to-delete]' --prune-empty --tag-name-filter cat -- --all

Don’t forget to set this file on `.gitignore` after deleting it from the history otherwise you may check in the file again!

## Commit-Hooks

All commit hooks can be used in git too. See the documentation on [Missing git hooks documentation](http://longair.net/blog/2011/04/09/missing-git-hooks-documentation/) and read [this tutorial](http://css.dzone.com/articles/all-git-hooks-you-need) or see this example: [PHPCS pre commit hook](https://github.com/s0enke/git-hooks/tree/master/phpcs-pre-commit).

## Submodules

Submodules must be replaced with git submodules or required by an other dependency management tool like composer or something similar.