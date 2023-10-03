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
through github via pull requests.

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

Every team member is responsible for reviewing code. The designations :speech_balloon:, :heavy_check_mark:, and :x: **should** be left by a reviewer as follows:

- :speech_balloon: (Comment) should be used when there is not yet an opinion on overall validity of complete PR, for example:
  - comments from a partial review
  - comments from a complete review on a Work in Progress PR
  - questions or non-specific concerns, where the answer might trigger an expected change before merging
- :heavy_check_mark: (Approve) should be used when the reviewer would consider it acceptable for the contributor to merge, *after addressing* all the comments. For example:
  - style nitpick comments
  - compliments or highlights of excellent patterns ("addressing" might be in the form of a reply that defines scenarios where the pattern could be used more in the code, or a simple :+1:)
  - a specific concern, where multiple reasonable solutions can adequately resolve the concern
  - a Work in Progress PR that is far enough along
- :x: (Request changes) should be used when the reviewer considers it unacceptable to merge without another review of changes that address the comments. For example:
  - a specific concern, without a satisfactory solution in mind
  - a specific concern with a satisfactory solution provided, but *alternative* solutions **may** be unacceptable
  - any concern with significant subtleties
  
Contributors **should** react to reviews as follows:
- :x: if *any* review is marked as "Request changes":
  - make changes and/or request clarification
  - **should not** merge until reviewer has reviewed again and changed the status
- (none) if there are no reviews, contributor should not merge.
- :speech_balloon: if *all* reviews are comments, then address the comments. Otherwise, treat as if no one has reviewed the PR.
- :heavy_check_mark: if *at least one* review is Approved, contributor **should** do these things before merging:
  - make requested changes
  - if any concern is unclear in any way, ask the reviewer for clarification before merging
  - solve a concern with suggested, or alternative, solution
  - if the reviewer's concern is clearly a misunderstanding, explain and merge. Contributor should be on the lookout for followup clarifications on the closed PR
  - if the contributor simply disagrees with the concern, it would be best to communicate with the reviewer before merging
  - if the PR is approved as a work-in-progress: consider reducing the scope of the PR to roughly the current state, and merging. (multiple smaller PRs is better than one big one)

It is also recommended to use the emoji responses to signal agreement or that
you've seen a comment and will address it rather than replying.  This reduces
github inbox spam.

Everyone is free to review any pull request.

Recommended Reading:

 - [How to Do Code Reviews Like a Human](https://mtlynch.io/human-code-reviews-1/)


## Merging

Once your pull request has been *Approved* it may be merged at your discretion.  In most cases responsibility for merging is left to the person who opened the pull request, however for simple pull requests it is fine for anyone to merge.

Please use `Squash and Merge` to keep the commit history clean on the main branch.

If substantive changes are made **after** the pull request has been marked *Approved* you should ask for an additional round of review.


## New Projects

Many projects follow a similar pattern, and there are
certain configurations and links that need to be repeated
in each repository. One suggested way to do that is to
clone [this template repo](https://github.com/carver/ethereum-python-project-template/)
and then push to a new repository once you've named your new project.

One benefit of working that way is that the template is
maintained as new practices are adopted. Changes to the
template are straightforward to merge in to projects that
are based on it, so global maintenance can be
propagated easily.


## Tips & Tricks

### Developing against locally modified dependencies

Let's assume we have a project A depending on project B. We can often find us in a situation where
we want to run project A with a modified version of project B. For instance, we might be working
on upcoming changes for project B and want to test drive them in project A. Or maybe we just want
to add some extra logging to project B in order to debug some related error that we are seeing in
project A.

Fortunately, `pip` allows us to add dependencies from any local directory without having actual
wheel package for it.

Let's assume we have project A and B sitting next to each other in the same directory.

```
├── project-a
└── project-b
```

We can install the local version of project B inside project A as follows:

```
cd project-a
pip install -e ../project-b
```

Internally, this creates a link to `../project-b` so that any changes made to it are immediately
reflected in project A.

### Pull Request with unreleased external dependencies

Consider we have project A depending on project B. It's a common scenario that we
are making changes to project B that will require project A to adapt to it (a "breaking change").
While the changes to project B may still be under review and therefore unreleased, it can be
valuable to open a PR against project A to show the migration path for these upcoming changes.

For us to be able to already use the new, unreleased version, we can set the dependency version of
package B to a temporary tag, commit or branch on GitHub. This ensures the PR can do a proper CI
run against the upcoming changes, allowing it to reveal potential problems that otherwise may
go unnoticed.

We can use the following syntax to specify a dependency directly from GitHub without creating an
actual package for it.

```
"package-name@git+https://github.com/owner/repository.git@tag"
```

Let's assume package B is called `lahja` and the `setup.py` file normally specifies the version in
the `install_requires` or `extras_requires` section as follows.

```
...
"web3==4.4.1",
"lahja==0.11.2",
"termcolor>=1.1.0,<2.0.0",
...
```

For the purpose of the PR, we can specify a temporary version such as.

```
...
"web3==4.4.1",
"lahja@git+https://github.com/cburgdorf/lahja-1.git@christoph/perf/filter-predicate"
"termcolor>=1.1.0,<2.0.0",
...
```

During the lifecycle of the PR the dependency version should be changed to the proper
version of the package as soon as a released version exists.

### Fetch Pull Requests without manually adding remotes

We often want or need to run code that someone proposes in a PR. Typically this involves adding the remote of the PR author locally and then fetching their branches.

Example:

```
git remote add someone https://github.com/someone/reponame.git
git fetch someone
git checkout someone/branch-name
```

With an increasing number of different contributors this workflow becomes tedious.
Luckily, there's a little trick that greatly improves the workflow as it lets us
pull down any PR without adding another remote.

To do this, we just have to add the following line in the `[remote "origin"]`
section of the `.git/config` file in our local repository.

```
fetch = +refs/pull/*/head:refs/remotes/origin/pr/*
```

Then, checking out a PR locally becomes as easy as:

```
git fetch origin
git checkout origin/pr/<number>
```

>Replace `origin` ☝ with the actual name (e.g. `upstream`) that we use for the
remote that we want to fetch PRs from.

Notice that fetching PRs this way is *read-only* which means that in case we do
want to contribute back to the PR (and the author has this enabled), we would
still need to add their remote explicitly.
