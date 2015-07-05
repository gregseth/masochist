---
tags: git github
---

# Dealing with a pull request from a deleted repo

**Scenario:** Somebody submits a pull request and then deletes their fork.

You can't add their fork as a remote and pull their code that way. The simplest way to get the commit (if the PR consists of a single commit) is:

```shell
$ git checkout -b gh/pull/48
$ curl -L https://github.com/wincent/Command-T/pull/48.patch | git am
```

Note the use of `-L`, which causes `curl` to follow redirects.

# See also

-   Assortment of tips/comments for working with pull requests from the [command line](/wiki/command_line): <https://gist.github.com/piscisaureus/3342247>
