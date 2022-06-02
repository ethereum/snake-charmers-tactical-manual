# Repository Template

Many of our projects are based on a [repository template](https://github.com/ethereum/ethereum-python-project-template/).

A key benefit of using the template is that we can easily propagate best practices to a bunch of repos.

## Using template to create a repo

Basically, just clone the template repo and start any new repo with that clone.

*NOTE: we do **not** squash the history when cloning. In order to easily pull in later updates from the repo, we need the full template history.*

TODO: step-by-step instructions

## Upgrading a repo to use the latest templated features

### Confirm template is remote

Check that you have the remote:

```sh
git remote -v
```

#### What it looks like if you DO have the template remote

```
$ git remote -v
origin	git@github.com:carver/eth-typing.git (fetch)
origin	git@github.com:carver/eth-typing.git (push)
template-host	git@github.com:ethereum/ethereum-python-project-template.git (fetch)
template-host	git@github.com:ethereum/ethereum-python-project-template.git (push)
upstream	git@github.com:ethereum/eth-typing.git (fetch)
upstream	git@github.com:ethereum/eth-typing.git (push)
```

#### What it looks like if you do NOT have the template remote

```
$ git remote -v
origin	git@github.com:carver/eth-typing.git (fetch)
origin	git@github.com:carver/eth-typing.git (push)
upstream	git@github.com:ethereum/eth-typing.git (fetch)
upstream	git@github.com:ethereum/eth-typing.git (push)
```

In this case, add the template as a remote this way:

```sh
git remote add template-host git@github.com:ethereum/ethereum-python-project-template.git
```

Then check out the master of the template repo as a local branch called `template`:

```sh
git fetch template-host
git checkout --track -b template template-host/master
```

### Pull in the latest template changes

#### Get the latest template changes into your template branch

```sh
git checkout template
git pull
```

#### Create a template upgrade feature branch

```sh
git checkout master
git pull
git checkout -b upgrade-template
```

#### Merge the latest template changes into your feature branch

```sh
git merge template
```

#### Resolve conflicts

As usual. Feel free to **leave in** any template variables like `<MODULE_NAME>` or `<PYPI_NAME>`,
because they will be filled in again, next.

Go ahead and commit with the template variables unfilled. This makes it easy to undo any mistakes
while filling in template variables.

### Fill any added template variables

#### Check if you've run a template fill before

If you have filled them in, then you'll see something like:
```
$ cat .project-template/template_vars.txt
eth_typing
eth-typing
eth-typing
eth-typing
eth-typing
Common type annotations for ethereum python packages
```

If you haven't filled them in, then you'll see something like:
```
$ cat .project-template/template_vars.txt
<MODULE_NAME>
<PYPI_NAME>
<REPO_NAME>
<RTD_NAME>
<PROJECT_NAME>
<SHORT_DESCRIPTION>
```

#### Fill variables

If the variables are already fully filled, then run
```sh
.project-template/refill_template_vars.sh
```

If not, run:
```sh
.project-template/fill_template_vars.sh
```

Answer any questions that you're prompted with.

#### Commit filled variables

Be sure to double-check all of the fills, by using `git add -p`.

### Push up the changes for PR

```sh
git push --set-upstream origin upgrade-template
```

Open the PR, resolve and issues, and merge. **Then you're done!**

## Configure an existing repo to use the template

Sometimes we have existing repos that are close to the template, but were never actually derived from the same git history. These are the steps to merge the git histories. This provides the benefit that it's much easier to pull in all the upgrades in the template at a later time.

### Fetch the template repo

```sh
git remote add tmpl git@github.com:ethereum/ethereum-python-project-template.git
get fetch tmpl
```

Check out the template on a local branch:
```sh
git checkout -b template tmpl/master
```

### Merge in the template

We want to force a merge, even though the repositories have no shared history.
```sh
git checkout -b join-to-template master
git merge --allow-unrelated-histories template
```

### Resolve conflicts

Yeah, no way around it. This will probably take a while. There's no handbook to follow here, you just have to figure it out patch by patch.

After committing the merge, don't forget to re-fill the template variables. See the section above called "Fill any added template variables"
