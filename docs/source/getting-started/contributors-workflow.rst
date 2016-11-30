
Contributors workflow
=====================

A normal git development workflow should be used with some
considerations:

1. **Always** develop on a feature branch.
2. **Never** commit, rebase or merge changes into the master. Your
   forked **master** branch should just be a mirror of the official one and must be pulled
   frequently to be up to date with latest features and bugfixes.

If you don't follow the previous suggestions your pull request probably will be rejected. 

As a recommendation use meaningful branch names because they are going to be public.

How to contribute
#################

Freedomotic follows the `fork &
pull <https://help.github.com/articles/using-pull-requests>`__ process
for collecting and quality-checking contributions from the development
community.

This process works as follows:

1. You can start by `forking our main git
   repository <https://github.com/freedomotic/freedomotic/fork>`__. This
   will create a Freedomotic fork named
   *YOUR-GITHUB-USERNAME/freedomotic.git*
2. Clone your fork locally by doing

.. code::

    git clone https://github.com/YOUR-GITHUB-USERNAME/freedomotic.git

3. On your local repository clone, create a branch with a meaningful
   name (e.g. new-feature-name). In case you are working to solve one of
   the `known
   issues <http://freedomotic.myjetbrains.com/youtrack/issues>`__,
   please include the issue in the branch name (e.g. *fixing-Core-413*).
   
   **Again, always develop on a branch.**
4. When your proposed modifications are complete, you can generate a
   pull request, e.g. asking to merge *fixing-Core-413* into
   **freedomotic/master**. This can be done by publishing your new local
   branch online in your repository fork

.. code:: 

    git push origin BRANCHNAME

To generate a pull-request just click the **Create pull request** button
on GitHub 6. Your pull request will be reviewed by Freedomotic
Development Team that will merge it into the main repository or ask for
further revisions.

More Info
#########

If you are clueless the procedure described above is covered in full
details with screenshots
https://help.github.com/articles/using-pull-requests/#initiating-the-pull-request
