# merge-bisect

Run bisect only on commits created directly inside `master` branch.

That is a walk over the commits:
  * Meta PR "Merge commits"
  * PR "Squash & merge" commits
  * Direct commits to master

This allows to reconcile the "Squash & merge" and the "Merge commit" parties by giving them a proper solution.

What we then have as a result - is a proper clever and faster bisect walk that consists of two phases:
1. Treat all PRs as atomic commits and detect the `master` branch commit that directs towards PR.
2. Bisect inside the PR using the "Merge commit" preserved history.

This yealds a faster bisect (by having fewer steps in the bisect), and yealding highest probablitily to automatically find and directly pointing the causal change, because first phase skips over the rough edges inside PRs and treats them atomically.

---

Initial code taken from: [Quantic.edu blog](https://blog.quantic.edu/2015/02/03/git-bisect-debugging-with-feature-branches/).
