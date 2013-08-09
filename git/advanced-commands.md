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

## Standup

Man kann sich über das Log Feature auch direkt die Sachen anzeigen lassen die man am letzten Tag so committet hat:

	git log --pretty=oneline --abbrev-commit --since yesterday --author ephigenia

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
