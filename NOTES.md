# Notes

Some command-line invocations that do certain things, tips, etc.

## Get all commits on a pull request

Run this for a repository to pull all pull request refs:

    $ git config --add remote.origin.fetch '+refs/pull/*/head:refs/remotes/origin/  pr/*'


Running these commands will show that #4552 hasn't been merged into master:

    tavish@temeraire:~/code/ipython (master)$ git log -1 `git merge-base --independent master origin/pr/4552`

    commit b321a7fda415c5efb73829a4ad137b91b9859055
    Author: MinRK <benjaminrk@gmail.com>
    Date:   Sat Nov 16 16:05:35 2013 -0800

        add use_dill note to what's new

    tavish@temeraire:~/code/ipython (master)$ git log -1 origin/pr/4552

    commit b321a7fda415c5efb73829a4ad137b91b9859055
    Author: MinRK <benjaminrk@gmail.com>
    Date:   Sat Nov 16 16:05:35 2013 -0800

        add use_dill note to what's new


Run this to get the point where #4552 diverges from master:

    $ git merge-base --all master origin/pr/4552

To see all the commits:

    $ git log `git merge-base --all master origin/pr/4552`..origin/pr/4552

If the pull request has been merged in, you can get all the commits like so.
Note that the merge-commit shown by the first command has two parents. Since it's a fast-forward merge, one of the parents the common ancestor. You can then take the range from that common ancestor to the tip of the pull request and get the log of that.

    tavish@temeraire:~/code/ipython (master)$ git log -1 `git merge-base --independent master origin/pr/4543`

    commit 85b169fa3afc3d374973295c7f1409ededddbaca
    Merge: f0efabf 6fcd7bd
    Author: Paul Ivanov <pivanov314@gmail.com>
    Date:   Fri Nov 15 11:59:41 2013 -0800

        Merge pull request #4543 from minrk/whos-startup
        
        make hiding of initial namespace optional


    # Look for where origin/pr/4543 diverged from master.
    tavish@temeraire:~/code/ipython (master)$ git merge-base --all f0efabf 6fcd7bd

    f0efabf5ad16e5552f9a086d90064c51f7575e69

    # It is the first parent of the merge commit. This could have been a FF merge.


    tavish@temeraire:~/code/ipython (master)$ git log --oneline f0efabf..origin/pr/4543

    6fcd7bd mention %whos instead of %who
    06fdc18 add whatsnew/pr for initial hidden-ns changes
    bc42d0b make `ipython -i script.py` match `%run script.py`
    680d854 make hiding of initial namespace optional
