## kpt pkg update

Apply upstream package updates

<link rel="stylesheet" type="text/css" href="/kpt/gifs/asciinema-player.css" />
<asciinema-player src="/kpt/gifs/pkg-update.cast" speed="1" theme="solarized-dark" cols="100" rows="26" font-size="medium" idle-time-limit="1"></asciinema-player>
<script src="/kpt/gifs/asciinema-player.js"></script>

    # run the tutorial from the cli
    kpt tutorial pkg update

[tutorial-script]

### Synopsis

    kpt pkg update LOCAL_PKG_DIR[@VERSION] [flags]


  **Note:** all changes must be committed to git before running update

  LOCAL_PKG_DIR:

    Local package to update.  Directory must exist and contain a Kptfile to be updated.

  VERSION:

  	A git tag, branch, ref or commit.  Specified after the local_package with @ -- pkg@version.
    Defaults the local package version that was last fetched.

	Version types:

    * branch: update the local contents to the tip of the remote branch
    * tag: update the local contents to the remote tag
    * commit: update the local contents to the remote commit

  --strategy:

    Controls how changes to the local package are handled.  Defaults to fast-forward.

    * resource-merge: perform a structural comparison of the original / updated Resources, and merge
	  the changes into the local package.  See `kpt help apis merge3` for details on merge.
    * fast-forward: fail without updating if the local package was modified since it was fetched.
    * alpha-git-patch: use 'git format-patch' and 'git am' to apply a patch of the
      changes between the source version and destination version.
      **REQUIRES THE LOCAL PACKAGE TO HAVE BEEN COMMITTED TO A LOCAL GIT REPO.**
    * force-delete-replace: THIS WILL WIPE ALL LOCAL CHANGES TO
      THE PACKAGE.  DELETE the local package at local_pkg_dir/ and replace it
      with the remote version.

  -r, --repo string

    Git repo url for updating contents.  Defaults to the repo the package was fetched from.

  --dry-run

    Print the 'alpha-git-patch' strategy patch rather than merging it.

#### Env Vars

  KPT_CACHE_DIR:

    Controls where to cache remote packages when fetching them to update local packages.
    Defaults to ~/.kpt/repos/

### Examples

    # update my-package-dir/
    git add . && git commit -m 'some message'
    kpt pkg update my-package-dir/

    # update my-package-dir/ to match the v1.3 branch or tag
    git add . && git commit -m 'some message'
    kpt pkg update my-package-dir/@v1.3

    # update applying a git patch
    git add . && git commit -m "package updates"
    kpt pkg  update my-package-dir/@master --strategy alpha-git-patch

###

[tutorial-script]: ../gifs/pkg-update.sh
