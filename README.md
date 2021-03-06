# git-bisect-master-pr

First, run bisect only on commits created directly on the `master` branch:

That is a walk over the commits:
  * Meta PR "Merge commits"
  * PR "Squash & merge" commits
  * Direct commits to master
  
Then bisect inside the PR commits.

---

This proper solution allows to reconcile the "Squash & merge" and the "Merge commit" parties.

The proper clever walk that consists of two phases:
1. Treat all PRs as atomic commits and detect the `master` branch commit that directs towards the particular PR.
2. Bisect inside the PR using the "Merge commit" preserved history.

This yealds:
  * :heavy_check_mark: Overall less steps in the bisect, so bisect is more robust and to the point. Walking in Phase 1 & 2 avoids all the unrelated to the case commits internal to other PRs.
    * :heavy_check_mark: More effectiveness. Less bisect cycles. More casual bisect experience.
    * :heavy_check_mark: Yealding a higher probablitily to success in the bisect and find the causal PR.
  * :heavy_check_mark: The algorithm treats all merge practices as equal, but due to Meta information commits and the preservation of the original history and comments in "Create a merge commit" approach - enables the possibility of the Phase II, inside PR search.
  * :heavy_check_mark: Since Phase I gives a higher success bisect probability/effectiveness. In the small commit walk in Phase II, addressing only the causal PR, - the total bisect walk has a much higher probablitily to successed. And also allows to automatically find and directly point to the causal commit more robustly and more precicely.
  
  And allows to relax about the commit history and merge methods and preserve original author smaller diff commits with comments.

### Algorithm

  1. Phase 1 of the walk.

      ```sh
      git-bisect-master <badCommit> <goodCommit>
      ```
  
  2. Finish bisect phase clasically:

      ```sh
      git bisect run <test-command>
      # OR
      git bisect bad/good
      ```
      , to find the PR/commit.

  3. Then, phase 2:
  
      ```sh
      git-bisect-pr <commit>
      ```
  
  4. Finish bisect phase clasically:
  
      ```sh
      git bisect run <test-command>
      # OR
      git bisect bad/good
      ```

  5. Success!

---

Initial code taken from: [Quantic.edu blog](https://blog.quantic.edu/2015/02/03/git-bisect-debugging-with-feature-branches/).
