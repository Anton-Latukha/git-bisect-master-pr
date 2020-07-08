# merge-bisect

Run bisect only on commits created directly inside `master` branch. That is the commits to master, squash & merge commits, and the "Merge commits" that are meta commits for direct PR merges.

This allows to reconcile the "Squash & merge" and the "Merge commit" parties with proper solution.

What we then have is a proper clever bisect walk.

`merge-bisect` allows to find the PR that caused an issue. And use of the "Merge commit" (preserving history) approach would allow further to do `git bisect` in the PR commits itself, yealding highest probablitily to automatically find and directly pointing the causal change.


Initial code taken from: [Quantic.edu blog](https://blog.quantic.edu/2015/02/03/git-bisect-debugging-with-feature-branches/).
