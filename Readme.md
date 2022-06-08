# Rebase Upstream Action

This Action is suitable if you:

* are maintaining a fork
* have changes that are not going to be merged into upstream
* want to keep changes based on the latest upstream

Basically this is doing `git rebase upstream master && git push -f`. If there are conflicts, it simply fails.

## Typical usage

```yml
# .github/workflows/sync.yml
name: Rebase Upstream
on:
  schedule:
  - cron: "0 0 * * 0"  # run once a week
  workflow_dispatch:   # run manually

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
      with:
        fetch-depth: 10  # greater than the number of commits you made
    - uses: imba-tjd/rebase-upstream-action@master
      # with:  # all args are optional
      #   upstream: <user>/<repo>
      #   branch:   master
```

## Comparison

* tgymnich/fork-sync and apps/pull: I don't want PRs. Besides there is not way to do a `git rebase` on GitHub website
* repo-sync/github-sync: It's not using rebase or merge. It completely mirrors the upstream so that it can't sync current branch
* wei/git-sync: Very complicated and have the same issue as github-sync. After all its aim is "syncing between two independent repo"
* aormsby/Fork-Sync-With-Upstream-action: If set `git_pull_rebase_config: true`, it's similar. But it tries to be configurable so that looks complex
* This one: Not widely tested. Use with caution
