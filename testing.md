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

### Mocks are to be avoided

Because:

- Mock based tests tend to test the implementation and not the feature functionality 
- It is very easy to have mock-based tests pass, when the actual code fails
- Mocking can be leaky if not done correctly: leaving objects monkey-patched after test cleanup, etc.

### Signal that your code is incorrectly structured

In many cases, if you find yourself needing to mock objects in order to write
your tests, this is a signal that your code may need to be refactored to make
functionality more modular and easier to test in isolation.

### When all else fails

Mocks **can** be ok when all other options are exhausted, or it is the pragmatic solution, or some other reason to ignore the bad things.

### But maybe don't mock things

A recurring pattern in our projects is a Backend Pattern, where you can swap in providers of functionality. Instead of mocking things that the code wasn't designed to do, try implementing the Backend Pattern, and swap in a dummy backend to stub out responses as fixtures. This provides most of the benefits of mocking, without sidestepping big swaths of code. You have more confidence that the majority of the feature was tested with the dummy backend. Then when you need to test a "live" backend, you can add use a few, targetted tests.
