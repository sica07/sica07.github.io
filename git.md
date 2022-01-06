# Git
[<TIL](Programming.md)

## Fixing mistakes
1. Spelled last commit message wrong
    `git commit --amend`

2. Spelling mistake on branch name

    `git branch -m feture-brunck feature-branch`

    If you have already pushed this branch, there are a couple of extra steps
    required. We need to delete the old branch from the remote and push up the new one:

    `git push origin --delete feature-brunch`
    `git push origin feature-branch`

3. Accidentally committed all the changes to the master branch
   Make sure your commit or stash your changes first, or all will be lost!

   `git branch feature-branch`
   `git reset HEAD~ --hard`
   `git checkout feature-branch`

4. Forgot to add a file to that last commit
   `git add missed-file.txt`
   `git commit --amend --no-edit`

   `--no-edit` means that the commit message does not change.

5. Added wrong file in the repo
   If all you did was stage the file and you haven’t committed it yet, it’s as simple as resetting that staged file:

   `git reset /assets/img/misty-and-pepper.jpg`

   If you have gone as far as committing that change, no need to worry. You just need to run an extra step before:
   ```
   git reset --soft HEAD~1
   git reset /assets/img/misty-and-pepper.jpg
   rm /assets/img/misty-and-pepper.jpg
   git commit
   ```
   This will undo the commit, remove the image, then add a new commit in its place.

6. Last resort (when nothing works)
   ```
   git reflog
   git reset HEAD@{index}
   ```
[source] (https://medium.com/@i_AnkurBiswas/common-git-mistakes-and-how-to-fix-them-10184cd5fa77)

## Tagging versions
Are you still not git tagging version?
If so, you should automatically and for the logs you could just run
"git log `git describe --tags --abbrev=0`..HEAD --oneline" which gives you
all the change between now & the last version / tag and pipe that to your clipboard


## Delete all untracked files
cleaning up (read: removing) untracked files from a local copy of a repository.
`$ git clean -f -d`

[source](https://github.com/jbranchaud/til/blob/master/git/delete-all-untracked-files.md)

## Untrack a file or a directory without deleting it
If you want to untrack a file (remove it from the index), but still have it available locally (in the working tree), then you are going to want to use the --cached flag.
`$ git rm --cached <filename>`
or for a directory
`$ git rm --cached -r <directory>`
**Note:** If you do this, you may also consider adding that file to the .gitignore file.

[source](https://github.com/jbranchaud/til/blob/master/git/untrack-a-file-without-deleting-it.md)


## Diffing with Patience algorithm
You can set this as the default algorithm by adding the following lines to your ~/.gitconfig file:
```
[diff]
    algorithm = patience
```

or it can be set from the command line with:

`$ git config --global diff.algorithm patience`

Comparison between the classic algorithm and the patience one
https://gist.github.com/roryokane/6f9061d3a60c1ba41237

[source](https://github.com/jbranchaud/til/blob/master/git/diffing-with-patience.md)


## Check to see all the changes in a commit
`$ git whatchanged`

## List all commits that changed a specific file
`$ git log --follow -- filename`
`--follow` accounts for renames
[src](https://stackoverflow.com/a/8808453)

## Git philosophy
### Goals
Git was designed to support a more distributed model with no need for a central repository
(though you can certainly use one if you like). Also git was designed so that the client and
the "server" don't need to be online at the same time. Git was designed so that people on an
unreliable link could exchange code via email, even. It is possible to work completely
disconnected and burn a CD to exchange code via git.

### Implementation
In order to support this model git maintains:
- a _local repository_ with **your code**
- a _local repository_ that **mirrors the state of the remote repository**.

By keeping a copy of the remote repository locally, git can figure out the changes needed
even when the remote repository is not reachable. Later when you need to send the changes
to someone else, git can transfer them as a set of changes from a point in time known to the
remote repository.

### Conclusion
The take away is to keep in mind that there are often at least three copies of a project
on your workstation.
One copy is your working copy where you are editing and building. (**_/home/you/workingtree_**)
The second copy is your own repository with your own commit history. (**_/refs/heads_**)
The third copy is your local "cached" copy of a remote repository. (**_refs/remots/<remote>/_**)

[source](https://stackoverflow.com/a/7104747/174320)

## Fetch vs. Pull

Git documentation – `git pull`:
> In its default mode, `git pull` is shorthand for `git fetch` followed by `git merge FETCH_HEAD`.

* `git fetch` is the command that says "bring my local copy of the remote repository up to date."
  Git gathers any commits from the target branch that do not exist in your current branch and
  stores them in your local repository. However, it does not merge them with your current branch.
  This is particularly useful if you need to keep your repository up to date,
  but are working on something that might break if you update your files.

* `git pull` says "bring the changes in the remote repository to where I keep my own code."
  Normally `git pull` does this by doing a `git fetch` to bring the local copy of the remote
  repository up to date, and then merging the changes into your own code repository
  and possibly your working copy. It is _context sensitive_, so Git will automatically merge
  any pulled commits into the branch _you are currently working_ in.

[source](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch)
