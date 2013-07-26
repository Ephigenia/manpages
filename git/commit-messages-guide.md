# Git Commit Messages Guide

* written in present tence
* should be desriptive but not to verbose

## Example

	[Component:] short description of Commit (max 80 chars)

	Some more detailed description of the commit giving some hints on what has
	been changed. Note that these lines should not extend 80chars.

## DO

* Use Keywords like: "fix", "add", "change"
* Leave second line blank
* Line breaks should be included (limit lines to max 80 chars)

## Good Practice

Commit messages like the following are not very helpfull and should be more precise in what what was moved or which exact values where changed:

* "not needed"
* "Reverse to match values"
* "move to supported games"
* "One more file to fix"
* "This difference only applies to xy"

Always keep in mind that theses messages are often read without the according diffs or the ability to see which files where changed in the commit.

## Tips

If the commit message contains to many stuff and different themes it should be better split into single commits using the `git add -p` command.


## Sources:

*	[SCUMMVM Git Contribution Guide](http://wiki.scummvm.org/index.php/Commit_Guidelines)
*	[Google Angular Commit Guide](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit?pli=1)
*	http://www-cs-students.stanford.edu/~blynn/gitmagic/
*	[erlang/otp](https://github.com/erlang/otp/wiki/Writing-good-commit-messages)