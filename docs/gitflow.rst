**************************************************
Git-related Conventions for appsembler-mediathread
**************************************************

Branch Naming
-------------

The branch should be named as ``<trello-card-#>-<branch-name>``.

**e.g. 15-new-form, 57-landing-page**


Git flow
--------

refer to `A successful Git branching model <http://nvie.com/posts/a-successful-git-branching-model/>`_.

Installation
++++++++++++

Using your Package Manager to install package which is called `git-flow`.

Linux
    ``sudo apt-get install git-flow``

Mac OS X
    ``brew install git-flow``


Configuration
+++++++++++++

+ branches
    - production branch: appsembler_master
    - "next release" branch: appsembler_develop
+ prefix
    - feature: feature/ (Default)
    - release: release/ (Default)
    - hotfix: hotfix/ (Default)
    - support: support/ (Default)
    - version tag: None (Default)


Commands
++++++++

initialization
    ``git flow init``
    and answer the prompt by the order of following configuration

start a flow
    ``git flow <prefix> start <branch name>``
    this will create a new branch and checkout it

finish a flow
    ``git flow <prefix> finish <branch name>``
    this will commit your change in that branch and merge it back to "the next release" branch

Setup
+++++

For the first time you need to run ``git flow init`` to initialize git-flow for specified repo, or you can direct modify git config file.

::

    [gitflow "branch"]
        master = appsembler_master
        develop = appsembler_develop
    [gitflow "prefix"]
        feature = feature/
        release = release/
        hotfix = hotfix/
        support = support/
        versiontag =

::

You can also copy and paste above text into your ``path/to/mediathread/.git/config`` (replace original one or append in the bottom)
