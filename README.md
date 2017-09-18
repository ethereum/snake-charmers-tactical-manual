# Development Tactical Manual

This document lays out guidelines and expectations for the Ethereum Foundation
Python development team.


### Testing

Testing is **essential** to the production of software with minimal flaws.  The
default should always be writing tests for the code you produce.

Tests **must** always be automated using a service like Travis-CI.

Testing also introduces overhead into our workflow.  If a test suite takes a
long time to run, it slows down our iteration cycle.  This means finding a
*pragmatic* balance between thorough testing, and the speed of our test suite,
as well as always iterating on our testing infrastructure.


### Pull Requests

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


### Code Style

When we see code written in a style that does not conform to our own style it
is more difficult to read.  For this reason, we **should** strive to write code
which conforms to a common style.  The exact style guide we choose is less
important than the act of picking one.

This style guide will represent the consensus of the python team for how we
write code.  It is intentionally opinionated and specific.

* [Style Guide](./style-guide.md)

We also acknowledge that there will be many cases where there are good reasons
to deviate from this guide.


### Documentation

Good documentation will lead to quicker adoption and happier users.  It should
also lead to fewer requests for help and support.  Documentation also serves to
draw a clear line between public and private APIs.

Programatically generated API documentation is generally not sufficiently user
friendly to serve as **good** documentation.  For this reason, we will strive
to produce *narative* style documentation which walks the user through the high
level concepts as well as low level details they may need to use the library.

Inclusion of examples within the documentation is also key for assiting users
to understand how the library can and should be used.


### Breaking Changes and Deprecation Policies

Libraries and APIs which are experimental should be clearly marked as such in
the documentation.  These APIs/libraries may be subject to arbitrary breaking
changes.

However, anything that is documented should be subject to a deprecation period
before introducing **any** breaking API changes.  Our users need to trust they
can use our libraries as foundational components to their products and that
they won't be surprised with sudden breaking changes.

Exceptions to this would be things like security issues.


### Delivering Results

As you develop features you should balance the timely delivery of shippable
code with well designed architecture.

New feature work can take multiple weeks to develop, but you should never go
multiple weeks without merging code.  There are always ways that large changes
can be broken down into iterative smaller changes.

It is ok to develop a large feature branch with broad sweeping API changes, but
it will need to be broken down into smaller pieces that maintain backwards
compatability in order to be merged.


### Community Support

Our users interact with us primarily through Github issues and the Gitter chat
channels.  You should make an effort to respond when a user needs help.  We are
often the most knowledgeable people when it comes to our libraries and thus are
often in the best position to provide help.


### The Zen of the Python Ethereum Team

Here's a collection of things.

* Write fewer classes.
* Write more functions.
* Write even more utility functions.
* Hard to test code is probably wrong
* Less statefulness
* More pure functions
* Avoid mutability at *almost* any cost.
* The [toolz](http://toolz.readthedocs.io/) library is your friend.
* One concept per line of code.
* Don't be clever.

