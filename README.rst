=================================
Find downstream patches in Gerrit
=================================

When dealing with open-source projects in a enterprise world,
it is unfortunately a quite common scenario to have downstream patches
that might be needed to carry over from release to release.

This script is what helps to find those.
It compares two branches from project (possibly hosted on different
Gerrit instances) and finds commits that are missing by comparing
``Change-Id`` inserted into the commit message by the Gerrit git hook.

It also can filter out commits that, although not present in the
new branch/release, are definitely not be carried over
(defaults are targeting peculiarities of OpenStack projects
and development process).

Installation
============

In a virtual env using ``pip install git+https://github.com/pshchelo/gerrit-scripts``.

Usage
=====

In the virtualenv::

    $ custom-patches --help
    usage: custom-patches [-h] [--workdir WORKDIR] [--gerrit GERRIT]
                          [--gerrit-username GERRIT_USERNAME]
                          [--gerrit-password GERRIT_PASSWORD] [--project PROJECT]
                          [--new-gerrit NEW_GERRIT]
                          [--new-gerrit-username NEW_GERRIT_USERNAME]
                          [--new-gerrit-password NEW_GERRIT_PASSWORD]
                          [--new-project NEW_PROJECT]
                          [--project-prefix PROJECT_PREFIX]
                          [--old-branch OLD_BRANCH] [--new-branch NEW_BRANCH]
                          [--long] [--json JSON] [--regex REGEX]
                          [--mcp-packages-file MCP_PACKAGES_FILE]

    Using Geriit Change-Id, report patches in the history leading to <old branch>
    which are missing in the history of <new branch>.

    optional arguments:
      -h, --help            show this help message and exit
      --workdir WORKDIR     Working directory
      --gerrit GERRIT       Gerrit location (full HTTP(S) URL). Defaults to
                            CUSTOM_PATCHES_GERRIT_LOC shell var
      --gerrit-username GERRIT_USERNAME
                            Gerrit HTTP user name to access Gerrit HTTP API/repos.
                            Defaults to CUSTOM_PATCHES_GERRIT_USERNAME shell var
      --gerrit-password GERRIT_PASSWORD
                            Gerrit HTTP password. Defaults to
                            CUSTOM_PATCHES_GERRIT_HTTP_PASSWORD shell var.
      --project PROJECT     Gerrit project name. Defaults to
                            CUSTOM_PATCHES_GERRIT_PROJECT shell var.
      --new-gerrit NEW_GERRIT
                            New Gerrit location (full HTTP(S) URL).Defaults to
                            CUSTOM_PATCHES_NEW_GERRIT_LOC shell var. If empty,
                            falls back to Gerrit location.
      --new-gerrit-username NEW_GERRIT_USERNAME
                            New Gerrit HTTP user name to access Gerrit HTTP
                            API/repos.Defaults to
                            CUSTOM_PATCHES_NEW_GERRIT_USERNAME shell var. If
                            empty, falls back to Gerrit username.
      --new-gerrit-password NEW_GERRIT_PASSWORD
                            Gerrit HTTP password. Defaults to
                            CUSTOM_PATCHES_GERRIT_HTTP_PASSWORD shell var.
      --new-project NEW_PROJECT
                            New Gerrit project name. Defaults to
                            CUSTOM_PATCHES_NEW_GERRIT_PROJECT shell var. If empty,
                            falls back to Gerrit project name.
      --project-prefix PROJECT_PREFIX
                            Gerrit project prefix, to fetch all projects starting
                            with it. Defaults to
                            CUSTOM_PATCHES_GERRIT_PROJECT_PREFIX shell var.
      --old-branch OLD_BRANCH
                            Old branch (typically, previous release). If
                            resembling a full-length SHA, will be considered as
                            commit SHA instead of a branch name. Defaults to
                            CUSTOM_PATCHES_OLD_BRANCH shell var
      --new-branch NEW_BRANCH
                            New branch (typically, current release). If resembling
                            a full-length SHA, will be considered as commit SHA
                            instead of a branch name. Defaults to
                            CUSTOM_PATCHES_NEW_BRANCH shell var
      --long                Print full commit messages
      --json JSON           Path to JSON output file. Default is not to generate
                            JSON output.
      --regex REGEX         Output only commits with title matching this regular
                            expression. Default "^(?!(Updated from global
                            requirements|Imported Translations from Zanata|Update
                            UPPER_CONSTRAINTS_FILE for stable|Update .gitreview
                            for stable|import zuul job settings from project-
                            config))" is mostly suitable for OpenStack projects
                            and their stable branches. To output all missing
                            commits, set it to '.*'.
      --mcp-packages-file MCP_PACKAGES_FILE
                            Path to debian Packages file to parse, overrides
                            old_branch and project / project-prefix. Can be used
                            to find commit diff between code in MCP debian package
                            repo and Gerrit. WARNING: Very MCP specific as it
                            relies on private package metadata fields added by MCP
                            package build procedure to find commits from which
                            packages were built.
