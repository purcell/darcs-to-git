darcs-to-git
============

Converts a Darcs repository into a Git repository.  Supports
incremental updates, i.e., you can pull new patches from the source
repository or import a large repository in steps.

Usage
-----

(Use `darcs-to-git --help` to display the latest usage instructions.)

1. Create an *empty* directory that will become the new git repository
2. From inside that directory, run this program, passing the location
   of the local source darcs repo as a parameter

The program will git-init the empty directory, and migrate all patches
in the source darcs repo into commits in that repository.

Thereafter, incremental patch conversion from the same source repo is
possible by repeating step 2.

Options
-------

 * `--patches N`: only import `N` patches.

 * `--email-address ADDRESS`: `darcs-to-git` tries to reconstruct the
   email address from the darcs patch.  In cases this is not possible,
   a default will be picked by Git.  This is usually the one in
   `~/.gitconfig`.  This option allows you to specify another default
   (without having to to modify `~/.gitconfig.`)

 * `--list-authors`: Outputs a list of authors in the source
   repository and how they will appear in the git repository and
   quits.  The output will be lines like this:

   ```
   Jane@example.com: Jane <Jane@example.com>
   ```

   This means that the darcs author `Jane@example.com` will be
   translated to git-author `Jane` with email address
   `Jane@example.com`.  You can use the output of this command as a
   starting point for the input for `--author-map`.

 * `--author-map FILENAME`: Allows translations from darcs committer
   name to Git committer name.  The input is a YAML map.  For an
   example see the output of `--list-authors`.  The author map will be
   stored in the repository and will be re-used for future imports.


Known issues
------------

When `darcs-to-git` pulls a conflicting patch it will revert the state
of the repository to the state before the conflict. **This will also
remove any local changes to your repository, including git commits!**
You should therefore not commit to the branch you import to, but
instead work in a different branch.  You can rename your master branch
after import using:

    $ git branch -m darcs_import

`darcs-to-git` creates a full copy of the original repository in addition to the Git repository;
this can lead to considerable space usage. You can save space by treating the copied Darcs
repository [as a branch](http://wiki.darcs.net/BestPractices#how-to-create-a-branch) by
running

    $ darcs optimize --relink --sibling /old-repo/dir

inside the new repository.

Acknowledgements
----------------

Written and maintained by Steve Purcell, with some improvements by
Thomas Schilling, Jonathon Mah and others.


<hr>

[![](http://api.coderwall.com/purcell/endorsecount.png)](http://coderwall.com/purcell)

[![](http://www.linkedin.com/img/webpromo/btn_liprofile_blue_80x15.png)](http://uk.linkedin.com/in/stevepurcell)

[Steve Purcell's blog](http://www.sanityinc.com/) // [@sanityinc on Twitter](https://twitter.com/sanityinc)

