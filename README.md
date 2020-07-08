# merge-bisect

Run bisect only on commits created directly inside `master` branch.

That is a walk over the commits:
  * Meta PR "Merge commits"
  * PR "Squash & merge" commits
  * Direct commits to master

---

This proper solution allows to reconcile the "Squash & merge" and the "Merge commit" parties.

A proper clever walk that consists of two phases:
1. Treat all PRs as atomic commits and detect the `master` branch commit that directs towards the particular PR.
2. Bisect inside the PR using the "Merge commit" preserved history.

This yealds:
  * :heavy_check_mark: Overall less steps in the bisect. Walking in Phase I avoids all the unrelated to the bisect commits internal to the more atomical PRs.
    * :heavy_check_mark: More effectiveness. Less bisect cycles. More casual bisect experience.
    * :heavy_check_mark: Yealding a higher probablitily to success in the bisect and find the causal PR.
  * :heavy_check_mark: The algorithm treats all merge practices as equal, but due to preservation of the original history and comments in "Create a merge commit" approach - enables the possibility of the Phase II.
  * :heavy_check_mark: Since Phase I gives a higher success bisect probability/effectiveness. By addressing the causal PR in the small commit walk in Phase II, the result has a higher probablitily to success in the bisect. And allows to automatically find and directly point to the causal commit.

### Algorithm

  1. Take a range of the commits `<badCommit>` and `<goodCommit>`.
    
  2. Use the tool to start first phase of the walk.

      ```sh
      merge-bisect <badCommit> <goodCommit>`
      ```
  
  3. Then continue the classical:

      ```sh
      git bisect run <test-command>
      # OR
      git bisect bad/good
      ```
      , to find the PR.

  4. Case:
      
      a) A regular commit (or Squash & merge) - here you go, commit found.
      
      b) A Merge commit:

      ```sh
      git bisect reset
      git bisect start <commit>^1 <commit>^2
      ```
      ^ This starts a classical bisect inside a PR.

      ```sh
      git bisect run <test-command>
      # OR
      git bisect bad/good
      ```

  5. Success!

---

Initial code taken from: [Quantic.edu blog](https://blog.quantic.edu/2015/02/03/git-bisect-debugging-with-feature-branches/).
