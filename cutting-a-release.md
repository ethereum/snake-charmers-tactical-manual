How to cut a new release:
====

Make sure to cut the release when there is time to support it if something goes wrong. As a rule, don't do releases late in the afternoon or on Fridays.

Steps:
- [ ] Merge any outstanding PRs that are ready.
- [ ] Double check that the tests ran (and passed) on the merged commit(s).
- [ ] Pull down the branch that you'll cut the release from locally.
- [ ] Make release notes before bumping the version number:
  - Look at the commits between the last version bump and the current one. Following the template already laid out in `docs/releases.rst`, write out what each unit of work does.
  - There are three categories that the units of work might fall under:
    - Bugfixes
    - Features
    - Misc
- [ ] Run `$ make docs` to view the release notes locally. Make sure everything looks like it should.
- [ ] Once the docs look good, commit the changes.
- [ ] We use [Semantic Versioning](https://semver.org/) for all of our libraries. Decide which bump applies (major, minor, or patch) to the changes that will be relased. Here's a quick cheat sheet:
  - Use a Major version bump when you make incompatible API changes
  - Use a Minor version bump when you add functionality in a backwards-compatible manner
  - Use a Patch version bump when you make backwards-compatible bug fixes
- [ ] Most repos have a section in the README that describes how to use the `make release` script. It looks something like [this](https://github.com/ethereum/web3.py#release-setup). Follow those instructions.
  - If something goes wrong in the middle, you'll need to: 
    - Figure out what went wrong and fix that problem
    - Open up the Makefile, and run the failed command and all subsequent commands manually
- [ ] Test it!
  - Create and activate a new virtualenv
  - `$ pip install -U <released_project>`
  - Open shell and confirm that the module import works. Check a simple feature and/or new features
- [ ] Make sure the Read The Docs build succeeded. Visit the [web3.py Release Notes](https://web3py.readthedocs.io/en/stable/releases.html) and make sure your changes are there.
- [ ] Announce to the Gitter channel. Typically this announcement includes a link to the release docs. :tada:
