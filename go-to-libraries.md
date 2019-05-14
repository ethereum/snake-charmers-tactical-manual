# Go-To Libraries

This is the location for opinionated write-ups of different python libraries to
accomplish certain tasks.


## Grammar Parsing

### [Parsimonious](https://github.com/erikrose/parsimonious)

@piper recommends

This is a good choice if you need to parse a simple grammar such as ABI type
strings.  For complex grammars it can be lacking in expressiveness.


### [PyParsing](https://pyparsing-docs.readthedocs.io/en/latest/)

@piper recommends

This is likely the most well established parsing library in the python
ecosystem.  It uses python classes for constructing the grammar which requires
learning your way around the library some, but also provides extreme
flexibility and expressiveness.

Beyond the grammar parsing component, it also lends itself well to functional
style programming as each grammar rule can be assigned a parse action which is
just a function to process the parsed result.


### [Lark](https://github.com/lark-parser/lark)

@piper **does not** recommend

I can't currently recommend Lark.  It's got a nice grammar and API but the
library doesn't appear to be very well maintained or documented and there are
places where the documentation doesn't match the library or the library doesn't
behave in documented ways.  Expect to end up reading the code some if you
choose this for anything very complex.

### [`cached-property`](https://pypi.org/project/cached-property/)

@piper recommends

For `@property` methods that are safe to cache this provides a clean, well tested, 
and highly performant mechanism.  Use of this however collides with the `__slots__` 
approach.

> NOTE: https://github.com/pydanny/cached-property/issues/69 demonstrates how to combine this with `__slots__`.
