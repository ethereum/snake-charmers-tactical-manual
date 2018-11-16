How to cut a new release:
====

Make sure to cut the release when there is time to support it if something goes wrong. As a rule, don't do releases late in the afternoon or on Fridays.

Steps:
1. Merge any outstanding PRs that are ready.
1. Double check that the tests ran (and passed) on the merged commit(s).
1. Pull down the branch that you'll cut the release from locally.
1. Make release notes before bumping the version number:
  - Look at the commits between the last version bump and the current one. Following the template already laid out in `docs/releases.rst`, write out what each unit of work does.
  - There are three categories that the units of work might fall under:
    - Bugfixes
    - Features
    - Misc
1. run `$ make docs` to view the release notes locally. Make sure everything looks like it should.
1. Once the docs look good, commit the changes.
1. Most repos have a section in the README that describes how to use the `make release` script. It looks something like [this](https://github.com/ethereum/web3.py#release-setup). Follow those instructions.
  - If something goes wrong in the middle, you'll need to: 
    - Figure out what went wrong and fix that problem
    - Open up the Makefile, and run all commands after the one that failed manually 
1. Test it!
1. Announce to the Gitter channel. Typically this announcement includes a link to the release docs. :tada:
