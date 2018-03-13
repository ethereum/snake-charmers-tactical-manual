# Testing

Testing is **essential** to the production of software with minimal flaws.  The
default should always be writing tests for the code you produce.

## Continuous Integration

Tests **must** always be automated using a service like Travis-CI or Circle-CI.

Testing also introduces overhead into our workflow.  If a test suite takes a
long time to run, it slows down our iteration cycle.  This means finding a
*pragmatic* balance between thorough testing, and the speed of our test suite,
as well as always iterating on our testing infrastructure.


## Testing Framework

[`py.test`](https://docs.pytest.org/en/latest/) is the default framework we use
for testing.


## Using Mocks

The use of mock objects such as those provided by the
`pytest-mock`](https://pypi.python.org/pypi/pytest-mock) library should be
considered an anti-pattern.

- Mock based testing promotes testing of the implementation rather than the
  functionality.
- Mock based testing is prone to the tests passing despite the code failing in
  production.

In many cases, if you find yourself needing to mock objects in order to write
your tests, this is a signal that your code may need to be refactored to make
functionality more modular and easier to test in isolation.
