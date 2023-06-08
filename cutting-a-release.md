How to cut a new release:
====

Make sure to cut the release when there is time to support it if something goes wrong. As a rule, don't do releases late in the afternoon or on Fridays.

Steps:
- [ ] Merge any outstanding PRs that are ready.
- [ ] Double check that the tests ran (and passed) on the merged commit(s).
- [ ] Pull down the branch that you'll cut the release from locally.
- [ ] Make sure the dev dependencies are installed in your virtual environment. It may also be necessary to manually install the dependencies listed under the `setup_requires` keyword argument.
- [ ] We use [Semantic Versioning](https://semver.org/) for all of our libraries. Decide which bump applies (major, minor, or patch) to the changes that will be relased. Here's a quick cheat sheet:
  - Use a Major version bump when you make incompatible API changes
  - Use a Minor version bump when you add functionality in a backwards-compatible manner
  - Use a Patch version bump when you make backwards-compatible bug fixes
- [ ] Make release notes before bumping the version number:
  - Check the Makefile to see if there is a `make notes` command
  - If there is a `make notes` command:
    - [ ] From the command line run: `$ make notes bump=$$version part to bump$$`
      - the "version part to bump" will usually be `major`, `minor`, or `patch`, but will sometimes be a devnum. Refer to the contributing guide in the docs for the library you're releasing to look at the syntax for
      a devnum bump. For an example, check out the [web3.py Contributing Docs](https://web3py.readthedocs.io/en/stable/contributing.html#which-version-part-to-bump)
    - [ ] Run `$ make docs` to view the release notes locally. Make sure everything looks like it should.
  - If there is not a `make notes` command, you'll have to write the release notes manually.
    - [ ] Look at the commits between the last version bump and the current one. Following the template already laid out in `docs/releases.rst`, write out what each unit of work does.
      - Here are some categories that the units of work might fall under:
        - Breaking Changes (only to be added in a major version bump)
        - Bugfixes
        - Deprecation
        - Docs
        - Features
        - Internal
        - Misc
        - Performance
        - Removal (only to be added in a major version bump)
    - [ ] Run `$ make docs` to view the release notes locally. Make sure everything looks like it should.
    - [ ] Once the docs look good, commit the changes.
    - [ ] Push the doc changes to master.
- [ ] Most repos have a section in either the README or the Contributing section of the docs that describes how to use the `make release` script. It looks something like [this](https://web3py.readthedocs.io/en/stable/contributing.html#releasing). Follow those instructions. Usually, it looks something like `$ make release bump=$$version part to bump$$`.
  - Troubleshooting:
    - If something goes wrong in the middle, you'll need to figure out what went wrong and fix that problem
    - Open up the Makefile, and run the failed command and all subsequent commands manually
    - If the problem persists, you may be able to skip the command and run the subsequent commands manually
- [ ] Test it!
  - Create and activate a new virtualenv
  - `$ pip install -U <released_project>`
  - Open shell and confirm that the module import works. Check a simple feature and/or new features
- [ ] Make sure the ReadTheDocs build succeeded. Visit the `https://<project name>.readthedocs.io/en/stable/releases.html` and make sure your changes are there.
- [ ] Announce to the Discord channel if it's a user-facing library. Typically this announcement includes a link to the release notes. :tada:
