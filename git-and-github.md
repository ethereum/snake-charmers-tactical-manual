# Git

We use `git` for version control and github for hosting of all of our code
repositories.


## Commit Signing

You **should** setup your local git to sign all of your commits.  This can be
done with `git config commit.gpgsign true`

You will also need to inform github of your private key so that it can *verify*
your commits.  See github's documentation on that
[here](https://help.github.com/articles/signing-commits-using-gpg/)


## Labels

We tend to use the following standard labels in our repositories.

- Ready to Merge
- Needs Review
- Work in Progress
- Bug
- Good for Bounty

Many repositories may implement extra labels as well.  You as the author of
pull requests are responsible for applying the appropriate labels to pull
requests, as well as removing labels when appropriate.


## Pull Requests

We are a distributed team.  The primary way we communicate about our code is
through github via ppull requests.

* When you start work on something you should have a pull request opened that
  same day.
* Mark unfinished pull requests with the "Work in Progress" label.
* Pull requests **should** always be reviewed by another member of the team
  prior to being merged.
    * Obvious exceptions include very small pull requests.
    * Less obvious examples include things like time-sensitive fixes.
* You should not expect feedback on a pull request which is not passing CI.
    * Obvious exceptions include soliciting high-level feedback on your approach.


Large pull requests (above 200-400 lines of code changed) cannot be effectively
reviewed.  If your pull request exceeds this threshold you **should** make
every effort to divide it into smaller pieces.

You as the person opening the pull request should assign a reviewer.


## Commit Hygiene

We do not have any stringent requirements on how you commit your work, however
you should work towards the following with your git habits.

### Logical Commits

This means that each commit contains one logical change to the code.  For example:

- commit `A` introduces new API
- commit `B` deprecates or removes the old API being replaced.
- commit `C` modifies the configuration for CI.

This approach is sometimes easier to do *after* all of the code has been
written.  Once things are complete, you can `git reset master` to unstage all
of the changes you've made, and then re-commit them in small chunks using `git
add -p`.

### Rebasing

You should be using `git rebase` when there are *upstream* changes that you
need in your branch.  You **should not** use `git merge` to pull in these
changes.


### Commit Messages

We don't care much about commit messages other than that they be sufficiently
descriptive of what is being done in the commit.

The *correct* phrasing of a commit message.

- `fix bug #1234` (correct)
- `fixes bug #1234` (wrong)
- `fixing bug #1234` (wrong)

One way to test if you have it write is to complete the following sentence.

> If you apply this commit it will ________________.


## Code Review

Every team member is responsible for reviewing code.

You should use the *"Changes Requested"* flag for your review **only** when
there is a change in your feedback that **must** be addressed.  Otherwise, even
when you've suggested changes, if everything is architecturally sound you
should mark things as *"Accepted"*.

It is also recommended to use the emoji responses to signal agreement or that
you've seen a comment and will address it rather than replying.  This reduces
github inbox spam.

Everyone is free to review any pull request.
