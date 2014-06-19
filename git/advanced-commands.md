# Advanced Git Commands

## Rebase

**describe git rebase here**

## Partial File Commit

Es ist in Git möglich nur Teile der Änderungen in einer Datei zu commiten. Angenommen man hat verschiedene Änderungen an einer Datei vorgenommen und will nur die erste und dritte Änderung commiten, die zweite aber noch nicht.

	git add -p [file]

Danach wird man bei jeder Änderung in der Datei gefragt ob man sie übernehmen will (y) oder es bleiben lassen möchte (n). Danach kann man die gestaged Änderungen bequem committen.

Die weiteren Abkürzungen haben laut [Stackoverflow](http://stackoverflow.com/questions/10605405/what-does-each-of-the-y-n-q-a-d-k-j-j-g-e-stand-for-in-context-of-git-p) folgende Bedeutungen:

	y - stage this hunk
	n - do not stage this hunk
	q - quit; do not stage this hunk nor any of the remaining ones
	a - stage this hunk and all later hunks in the file
	d - do not stage this hunk nor any of the later hunks in the file
	g - select a hunk to go to
	/ - search for a hunk matching the given regex
	j - leave this hunk undecided, see next undecided hunk
	J - leave this hunk undecided, see next hunk
	k - leave this hunk undecided, see previous undecided hunk
	K - leave this hunk undecided, see previous hunk
	s - split the current hunk into smaller hunks
	e - manually edit the current hunk
	? - print help

## Changelogs

Man kann wenn die Commit Messages ein einheitliches Format mit Headline und Description haben wunderbare Changelogs generieren:

	git log --format=%s --no-merges --since="14 days"

Zeigt das Log der letzten 2 Tage mit den Headlines welches sich hoffentlich dazu eigenen sie in ein Changelog einzutragen.

## Log over all time

Manchmal hat man Mist gebaut und muss gucken wo die Änderungen in einer Datei gelandet sind. Ein einfaches `git log <path>` ist nicht ausreichend weil die Änderung eventuell in einem Commit in der Zukunft gelandet ist. Dann muss man ein Log ausgeben können welches auf allen Branches schaut:

	git log --all -- <path>

## Standup

Man kann sich über das Log Feature auch direkt die Sachen anzeigen lassen die man am letzten Tag so committet hat:

	git log --pretty=oneline --abbrev-commit --since yesterday --author ephigenia

## Number of Commits

Sometimes you want to know how many commits you’ve done since a specific tag or hash:

	git rev-list hash..HEAD | wc -l

## Tree

Um sich den Commit-Tree anzuzeigen wie man ihn vielleicht aus verschiedenen GUIs für Git kennt kann man sich ebenfalls dem `log` Befehl zu Hilfe nehmen:

	git log --graph --decorate --pretty=oneline --abbrev-commit
	
## Use Same Wording Hook

Beim Eingeben der Commit-Message (`git commit`) kann man sich in dem Editor welcher geöffnet wird, die x-letzten Commit-Nachrichten anzeigen lassen:

	#!/bin/sh
	
	# .git/hooks/prepare-commit-msg
	echo "#"
	echo "# Last 5 commit messages" >> $1
	echo "# ----------------------" >> $1
	COMMITS=`git log --pretty=format:"# %h %s [%an]" -5`
	echo "${COMMITS}" >> $1

Source: http://codeinthehole.com/writing/enhancing-your-git-commit-editor/

## Change Stats

Sometimes you want to know how many lines you or someone else changed in a time-frame:

	git diff --shortstat "@{1 day ago}" --author "ephigenia"

## Tags

### Push single tag to remote

	git push [remote-name] [tagname]

### Push single tag to remote which allready exists

For overwriting a tag in a remote you must delete the remote tag first. You can archive that by pushing it with the `--delete` option:

	git push --delete [remote-name] [tagname]
	git push [remote-name] [tagname]

## Incoming / Outgoing Alias

From: [http://zacharyflower.com/my-two-favorite-git-aliases/](http://zacharyflower.com/my-two-favorite-git-aliases/)

## Show Files with Conflicts

Sometimes you need to get a list of files that have a conflict during a merge:

	git diff --name-only --diff-filter=U

## Show branch list with last commit date & author

This command will show a list of branches ordered descending by their last commit and will also show the date of the last commit together with the author:

	git for-each-ref --sort=-committerdate refs/heads/ --format='%(committerdate:short) %(authorname) %(refname:short)'

## git-extras

[git-extras](https://github.com/visionmedia/git-extras) is a collection of advanced commands that integrate into the `git` command on the shell. It provides many informative commands like user lists, efforts on files and other stuff. See the [list of commands](https://github.com/visionmedia/git-extras/wiki#commands) you get.
