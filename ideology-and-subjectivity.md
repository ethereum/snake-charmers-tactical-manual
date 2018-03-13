# Ideology and Subjectivity

This section contains various ideological and subjective things about workflow,
development style, etc.


## Breaking Changes and Deprecation Policies

Libraries and APIs which are experimental should be clearly marked as such in
the documentation.  These APIs/libraries may be subject to arbitrary breaking
changes.

However, anything that is documented should be subject to a deprecation period
before introducing **any** breaking API changes.  Our users need to trust they
can use our libraries as foundational components to their products and that
they won't be surprised with sudden breaking changes.

Exceptions to this would be things like security issues.


## Delivering Results

As you develop features you should balance the timely delivery of shippable
code with well designed architecture.

New feature work can take multiple weeks to develop, but you should never go
multiple weeks without merging code.  There are always ways that large changes
can be broken down into iterative smaller changes.

It is ok to develop a large feature branch with broad sweeping API changes, but
it will need to be broken down into smaller pieces that maintain backwards
compatability in order to be merged.


## Community Support

Our users interact with us primarily through Github issues and the Gitter chat
channels.  You should make an effort to respond when a user needs help.  We are
often the most knowledgeable people when it comes to our libraries and thus are
often in the best position to provide help.


## The Zen of the Python Ethereum Team

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


## Friendly Code Review

When reviewing people's code consider the following two comments.

> I don't like the name of this function.

vs.

> What do you think about changing the name of this function to ....

Your feedback will often be better received if you pose it in the form of a
question.
