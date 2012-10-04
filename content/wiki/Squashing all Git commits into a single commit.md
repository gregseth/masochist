---
tags: git
cache_breaker: 1
---

# How

The simplest way seems to be the one [found here](http://stackoverflow.com/questions/1657017/git-squash-all-commits-into-a-single-commit); it consists of deleting the current `master` ref and then creating a new initial commit. Evidently the working tree is left untouched, and the current state of the index is used as a basis for the commit:

```shell
$ git update-ref -d refs/heads/master
$ git commit -s -m "Initial import"
```

# Why

Why would you want to do such a thing? Normally for minor tweaks of the history `git rebase` is your friend, but there is one use case where I want to squash the entire history of a repo into a single commit; when releasing a previously closed-source codebase as [open source](/wiki/open_source) for the first time.

You may not have the resources to audit the entire history of the project and ensure that you have legal right to publish all of it. It's much easier to audit just the current `HEAD` of the project. So you do this and decide you're good to go: you keep the "closed" version of the repo, but you start a new "open" version of the repo based on the current `HEAD` of the "closed" one.

You can either:

-   manually copy the worktree over into a new (empty) repository; or:
-   you can do a local `git clone` and squash the entire history as shown above

In both cases you'll need to set up your new remote (you don't want to push to the old remote, after all). Which of these is easier is up for debate.

# Alernatives

-   See "[Going public with previously proprietary code](/wiki/Going_public_with_previously_proprietary_code)"
