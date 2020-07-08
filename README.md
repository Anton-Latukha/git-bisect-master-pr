# merge-bisect

Run bisect only on commits created directly inside `master` branch.

This allows to have to reconcile the "Squash & merge" and the "Merge commit" parties. By treating "Merge commits" just as "Squash & merge" commits.

What we then have is a proper clever bisect walk.

This results in `merge-bisect` allows to find the PR that caused an issue, and use of the "Merge commit" (preserving history) approach allows then to `git bisect` the PR itself with high possibility to automatically directly find and pointing the causal change.


Initial code taken from: [Quantic.edu blog](https://blog.quantic.edu/2015/02/03/git-bisect-debugging-with-feature-branches/).
