---
tags: git
---

The notion of [rebasing](/wiki/rebasing) isn't one that I had heard of prior to starting with [Git](/wiki/Git). I found [this](http://jbowes.dangerouslyinc.com/2007/01/26/git-rebase-keeping-your-branches-current/) [weblog](/wiki/weblog) post which explains it very clearly:

> There is some upstream project that you wish to work on. You clone this upstream project when it is in state **A**, and make some changes. Your personal branch is now in state **Ab**, that is, **A** plus some set of changes **b**.
>
>     upstream ==========A
>
>     you                +=====Ab
>
> Now, while you’ve been writing **b**, more changes have occurred upstream. These changes may or may not also be contained in **b**. Upstream is now in state **A’**
>
>     upstream ==========A==========A'
>
>     you                +=====Ab
>
> Now, how do you get the differences between **A** and **A’** into your branch? With many distributed scms, you would perform a merge. The merge will take the differences between **A** and **A’** and apply them on top of **Ab** (this is a greatly simplified explaination, of course). Over time, you end up with a history in your branch that interleaves changes from upstream with your own changes. Merge is an option with git, but you can also perform a REBASE.
>
> With a rebase, the changes between **A** and **Ab** are taken and reapplied at **A’**:
>
>     upstream ==========A==========A'
>
>     you                           +=====Ab
>
> So your own changes are always the most recent. In practice, I find this to be a very elegant approach. git-rebase makes it easy to see and manipulate your own set of changes against the upstream codebase.

The [official documentation](http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html) goes into much more detail and describes considerably more complex scenarios in which `git rebase` can be employed.

# When not to use `rebase`

You should only use `git rebase` on your local-only branches. Its purpose is to keep your local, invisible changes up-to-date so that when you publish them they'll be more relevant and easy to understand for others.

If you are working on a public branch then you can't use `git rebase`. The first time I ever used `git rebase` I made this mistake. The remote server had two branches, `master` and `antlr`. My local machine had a clone of this repository with those same two branches. Changes were made to the `master` branch and I mistakenly used `git rebase` to get them replicated on the `antlr` branch.

This "worked" in a way (the `antlr` branch was updated) but it had undesirable side effects.

    First, rewinding head to replay your work on top of it...

    HEAD is now at c82b2ea... Split Wopen3 into separate repository

    Applying Add ANTLR README file

    Wrote tree 2b130d87be17e4386aa29c22dcfdff8660e29032
    Committed: 16d8ebefcabf455e691e366ff24fbfc2b8b6d457

The first sign that something was amiss was when I performed a `git push --all`:

    error: remote 'refs/heads/antlr' is not a strict subset of local ref 'refs/heads/antlr'. maybe you are not up-to-date and need to pull first?
    updating 'refs/heads/master'
      from 963908b833e9f0976fdfea00484c87d19e81850f
      to   c82b2eaa740520c43743dca0275eca5b278fb714
    updating 'refs/remotes/origin/HEAD'
      from 0000000000000000000000000000000000000000
      to   963908b833e9f0976fdfea00484c87d19e81850f
    updating 'refs/remotes/origin/antlr'
      from 0000000000000000000000000000000000000000
      to   963908b833e9f0976fdfea00484c87d19e81850f
    updating 'refs/remotes/origin/master'
      from 0000000000000000000000000000000000000000
      to   963908b833e9f0976fdfea00484c87d19e81850f
    Generating pack...
    Done counting 1 objects.
    Deltifying 1 objects...
     100% (1/1) done
    Writing 1 objects...
     100% (1/1) done
    Total 1 (delta 0), reused 1 (delta 0)
    refs/heads/master: 963908b833e9f0976fdfea00484c87d19e81850f -> c82b2eaa740520c43743dca0275eca5b278fb714
    refs/remotes/origin/HEAD: 0000000000000000000000000000000000000000 -> 963908b833e9f0976fdfea00484c87d19e81850f
    refs/remotes/origin/antlr: 0000000000000000000000000000000000000000 -> 963908b833e9f0976fdfea00484c87d19e81850f
    refs/remotes/origin/master: 0000000000000000000000000000000000000000 -> 963908b833e9f0976fdfea00484c87d19e81850f

Upon investigation, I saw that I had inadvertently created a new `origin` entry on the remote server:

    # in $GIT_DIR/refs/remotes/origin
    $ ls
    antlr  HEAD  master
    $ cat *
    963908b833e9f0976fdfea00484c87d19e81850f
    963908b833e9f0976fdfea00484c87d19e81850f
    963908b833e9f0976fdfea00484c87d19e81850f

I was sure that nobody else had cloned the repository in this state so I decided to blow away the changes immediately.

    # blow away the branch
    $ sudo -u git -H git branch -d antlr

    # the -d switch won't be enough in this case; will warn us as follows:
    #  error: The branch 'antlr' is not a strict subset of your current HEAD.
    #  If you are sure you want to delete it, run 'git branch -D antlr'.
    $ sudo -u git -H git branch -D antlr
    Deleted branch antlr.
    $ sudo -H -u git git prune

    # blow away the unwanted remotes
    $ cd ../..
    $ sudo rm -r remotes

    # create the branch again
    $ sudo -H -u git git branch antlr

Then, back on the local machine I cloned the repository again and reapplied the previously applied changes to the `antlr` branch:

    $ git clone git.example.com:/full_path_to_repo src
    $ cd src

    # create local branch to track remote branch
    $ git branch --track antlr
    $ git co antlr

    # apply changes...
    # then git add..., git ci..., git push...

So the moral of the story is:

> Remember that you should only use `git rebase` on your private, local-only branches. When working with publicly visible branches you will have no choice but to use `git merge` instead.

# See also

-   [Using git rebase](/wiki/Using_git_rebase)
