# Patching Around

## E-Mail Patches

Sometimes your connection to the remote is broken or lost and you want to deliver your changes including your author names and commits to a buddy who has access to it.

Git offers a nice feature which will create a single file containing a all commits done between two revisions which then can be applied to the repository.

### Creating E-Mail Patches

First you need to know from which commit to which commit you want to have your patchfile created.

The following example will create a `.patch` file right where you are which contains patches for two commits:

	git format-patch HEAD~2..HEAD

### Validating E-Mail Patches

	git apply --check <patchfile>

### Applying E-Mail Patches

	git am --signoff <patchfile>
