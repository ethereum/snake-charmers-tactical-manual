When to cut a new release
====

Releases should be considered for each change that is merged to the main branch. Not all merges require a new version so it is reasonable to bundle several smaller changes together in a single release. All libraries should follow Continuous Delivery and Semver best practices. We typically release on Wednesdays and at minimum once per month (but could be longer for less prominent libraries).

> :warning: Make sure to cut the release when there is time to support it if something goes wrong. As a rule, don't do releases late in the afternoon or on Fridays.

The team should review any outstanding PRs and recent merges at the weekly sync. If there are any newsfragments in the main branch there is a good chance a release is needed.

How to cut a new release
====

A release of a library requires a version bump, compiling release notes, and publishing to PyPI.

### Releasing with Semver

We use [Semantic Versioning](https://semver.org/) for all of our libraries. Decide which bump applies (major, minor, or patch) to the changes that will be released. Here's a quick cheat sheet:
  - Use a `major` version bump when you make incompatible API changes
  - Use a `minor` version bump when you add functionality in a backwards-compatible manner
  - Use a `patch` version bump when you make backwards-compatible bug fixes

Follow the sections below to release with the desired version.

### Requirements

  1. Changes are merged in the main branch
  2. The latest build on the main branch was successful

### Steps

  1. From your local copy of the lib's repository, check out and pull the latest from the main branch.
  2. Activate your virtual environment and run `python -m pip install -e ".[dev]"` to ensure the latest dev dependencies are installed.
      * It may be necessary to manually install dependencies listed under the `setup_requires` keyword.
  3. Verify documentation and release notes
      * Run `make docs` to preview and verify the docs pages.
      * Run `towncrier build --draft` to verify the release notes.
  4. Generate release notes with `make notes bump=<major,minor,patch>`.
  5. The remaining steps are run via `make release bump=<major,minor,patch>` command. This will typically build the docs, bump the version, push the tags to GitHub, and build and publish the package to PyPI.

If any of the commands fail, make sure the release is performed to completion. Once you have resolved the issue, run the remaining commands manually and verify the package is available.

### Final Verification

1. Make sure to install the new version and confirm everything is working.
    * Create and activate a new virtualenv, then run `$ pip install -U <released_project>`
2. Check the latest documentation is published on ReadTheDocs.
    * Also check the ReadTheDocs build succeeded at `https://<project name>.readthedocs.io/en/stable/releases.html`

### New Version Announcement

If the release was for a user-facing library, announce the latest version in the [web3py Discord channel](https://discord.com/channels/809793915578089484/817614427490615307). Typically this announcement includes a link to the release notes. :tada:



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
