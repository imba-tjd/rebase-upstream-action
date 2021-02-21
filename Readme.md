# Rebase Upstream Action

This Action is suitable if you:

* are maintaining a fork
* have changes that are not going to be merged into upstream
* want to keep changes based on the latest upstream

Basically this is doing `git rebase upstream master`. If there are conflicts, it simply fails.

## Typical usage

```yml
# .github/workflows/sync.yml
name: Rebase Upstream
on:
  schedule:
  - cron: "0 0 * * 0"  # run once a week
  workflow_dispach:    # run manually

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
      with:
        fetch-depth: 10  # greater than the number of commits you made
    - uses: imba-tjd/rebase-upstream-action@master
      # with:  # All args are optional
      #   upstream: <user>/<repo>
      #   branch:   master
```

Waiting for https://github.com/actions/runner/issues/646.

## Comparison

* tgymnich/fork-sync and apps/pull: I don't want PRs. Besides there is not way to do a `git rebase` on GitHub website
* repo-sync/github-sync: It's not using rebase or merge. It completely mirrors the upstream. And it can't sync current branch
* wei/git-sync: Very complicated and have the same issue as github-sync. After all its aim is "syncing between two independent repo"
* aormsby/Fork-Sync-With-Upstream-action: It's `git pull`. It trys to be configurable on many args so that may be buggy
* This one: Not widely tested. Use with caution
