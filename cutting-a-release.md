When to cut a new release
====

Releases should be considered for each change that is merged to the main branch. Not all merges require a new version so it is reasonable to bundle several smaller changes together in a single release. All libraries should follow Continuous Delivery and Semver best practices. We typically release on Wednesdays and at minimum once per month (but could be longer for less prominent libraries).

> :warning: Make sure to cut the release when there is time to support it if something goes wrong. As a rule, don't do releases late in the afternoon or on Fridays.

The team should review any outstanding PRs and recent merges at the weekly sync. If there are any newsfragments in the main branch there is a good chance a release is needed.

See the Best Practices For Major Version Changes section below for more details on things to consider for a major version bump.

### Annually scheduled releases

Python is now maintaining a regular schedule of releasing a new version and deprecating
an old one every October. Adding support for a new version is considered a feature and
can be included with any release. We consider removing support for a version breaking,
thus should only be done in a major release.

Updates from the [project template](https://github.com/ethereum/ethereum-python-project-template)
can be saved up through the year and done in one update cycle over Q4 of each year.
This should include adding the new python version and can be done as soon as CI has an
image available.

Removal of a supported python version should be coordinated with the major release
of `web3.py`. In order to avoid breaking libraries downstream, remove it first in
`web3.py`, then work backwards up the dependency tree. Utilizing beta releases can
make this process less disruptive.

How to cut a new release
====

A release of a library requires a version bump, compiling release notes, building the package, and publishing to PyPI.

### Releasing with Semver

We use [Semantic Versioning](https://semver.org/) for all of our libraries. Decide which bump applies (major, minor, or patch) to the changes that will be released. Here's a quick cheat sheet:
  - Use a `major` version bump when you make incompatible API changes
  - Use a `minor` version bump when you add functionality in a backwards-compatible manner
  - Use a `patch` version bump when you make backwards-compatible bug fixes

Follow the sections below to release with the desired version.

### Requirements

  1. Changes are merged in the main or version branch
  1. The latest build on the main or version branch was successful
  1. Team buy-in: You'll need to get approval from at least two team members before a release. They'll want to know what lib you're releasing, what is in the release.

### Steps

  1. From your local copy of the lib's repository, check out and pull the latest from the main or version branch.
  1. Activate your virtual environment and run `python -m pip install -e ".[dev]"` to ensure the latest dev dependencies are installed.
      * It may be necessary to manually install dependencies listed under the `setup_requires` keyword.
  1. Verify documentation and release notes
      * Run `make docs` to preview and verify the docs pages.
      * Run `towncrier build --draft` to verify the release notes.
  1. If you are issuing a release for a major version, check out the [Best Practices for Major Version Changes](#major-release) below.
  1. Generate release notes with `make notes bump=<major,minor,patch>`.
  1. The remaining steps are run via `make release bump=<major,minor,patch>` command. This will typically build the docs, bump the version, push the tags to GitHub, and build and publish the package to PyPI.

If any of the commands fail, make sure the release is performed to completion. Once you have resolved the issue, run the remaining commands manually and verify the package is available.

### Final Verification

1. Make sure to install the new version and confirm everything is working.
    * Create and activate a new virtualenv, then run `$ pip install -U <released_project>`
1. Check the latest documentation is published on ReadTheDocs.
    * Also check the ReadTheDocs build succeeded at `https://<project name>.readthedocs.io/en/stable/releases.html`

### New Version Announcement

If the release was for a user-facing library, announce the latest version in the [web3py Discord channel](https://discord.com/channels/809793915578089484/817614427490615307). Typically this announcement includes a link to the release notes. :tada:

It is also helpful to put a blurb into the ``#pending-announcements`` channel in the Snake Charmers internal Discord to help advertise what got changed.


## Release FAQ

### **Q: The `make release` command failed with "`The user '<username>' isn't allowed to upload to project '<project name>'`"**

**A:** This occurs when you do not have permissions set on your PyPI account. Reach out to the team to get access.

### **Q: What can I do if there is not a `make notes` command?**

**A:** In this case, the release notes must be created manually. The following steps outline this process:

1. Modify the release notes in `docs/releases.rst` to add a section for the new version.
2. Create subsections to categorize each of the changes that were made.

    ### Subsections/Categories
    - Breaking Changes (only to be added in a major version bump)
    - Bugfixes
    - Deprecation
    - Docs
    - Features
    - Internal
    - Misc
    - Performance
    - Removal (only to be added in a major version bump)

3. Verify documentation and release notes
    * Run `make docs` to preview and verify the docs pages.
4. Commit the changes to the main branch and push to the upstream repo with `git push upstream`.

## Best Practices for Major Version Changes (or, Things Learned Through Experience) <a name='major-release'>🔗</a>

- Although it's not always possible, try to batch up breaking changes within a library. For example, if you're going to release a major version of a lib, try and get any other low-hanging fruit and/or prioritized breaking changes in at the same time.
- Utilize beta versions. If there is a big breaking change going in, or multiple big breaking changes going in, consider releasing a beta version or two.
  * To perform a release from a pre-release version, use `make notes bump=stage` and `make release bump=stage`
- If you know a change will be breaking in a downstream library, add an upper pin to the dependency requirement in said downstream library while the PR is open in the upstream library.
- We prefer to have breaking changes that cascade across libraries queued up at the same time so that we can release affected libraries in concert. This cuts down on thrash and dependency hell for both ourselves and users.
- Before moving from beta to stable, take one last look at the changes, and consider adding an upper pin of that dependency to any downstream libraries that may be affected.
