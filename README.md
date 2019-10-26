# A strange magit / git-subline-merge Interaction

To reproduce:

* Install [git-subline-merge](https://github.com/paulaltin/git-subline-merge).
* Set [`GIT_SUBLINE_MERGE_NON_INTERACTIVE_MODE=1`](https://github.com/paulaltin/git-subline-merge#advanced-options) in your environment
* In `~/.gitconfig`, set:

    ```config
    [merge]
        conflictstyle = diff3

    [merge "git-subline-merge"]
        name = An interactive merge driver for resolving sub-line conflicts
        driver = git-subline-merge %O %A %B %L %P
        recursive = binary
    ```
* In this repository, check out `master`
* in a terminal, `git merge origin/A`.  Expect to see something like:

    ```
    git-subline-merge resolved conflict 1 of 1 in x.swift, resulting hunk was:
    <<<<<<<

                     print("some message")
                     let b = otherFunc(
    >>>>>>>
    Resolved 1 of 1 conflicts in x.swift
    Auto-merging x.swift
    hint: Waiting for your editor to close the file...
    Waiting for Emacs...
    Merge made by the 'recursive' strategy.
     x.swift | 2 +-
     1 file changed, 1 insertion(+), 1 deletion(-)
    ```

* Now roll that merge back: `git reset --hard origin/master`.
* In emacs/magit, do the same merge: `m m origin/A RET`
* Then `$` (or `M-x magit-process-buffer`) and expand the section
  for the last command
* Observe something like:

    ```
      0 git … merge --no-edit A



    ```

Where's the output from `git-subline-merge`?
