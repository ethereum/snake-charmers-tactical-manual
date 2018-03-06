# Documentation

Good documentation will lead to quicker adoption and happier users.  It should
also lead to fewer requests for help and support.  Documentation also serves to
draw a clear line between public and private APIs.

Programatically generated API documentation is generally not sufficiently user
friendly to serve as **good** documentation.  For this reason, we will strive
to produce *narative* style documentation which walks the user through the high
level concepts as well as low level details they may need to use the library.

Inclusion of examples within the documentation is also key for assiting users
to understand how the library can and should be used.


## Framework

For simple libraries with a very small API surface area, documentation can be
part of the `README.md` in the repository.

For all other things we default to using
[`Sphinx`](http://www.sphinx-doc.org/en/stable/)

We tend to host our documentation using the
[ReadTheDocs](https://readthedocs.com/) service.
