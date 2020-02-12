# Mutability

We have taken a strong stance against mutability and thus have established
multiple conventions that should be followed to avoid the use of mutability.

## Why go to such lengths

Here is a common example where use of a `list` is often the norm.

```python
class User:
    def get_recent_events(self):
        events = []
        for event in self.all_events:
            if event.is_recent():
                events.append(event)
        return event
```

This example is quite benign and the mutation of the list object doesn't pose
any real problems.  There are no race conditions, or shared state.  This raises
the question *why* do we avoid this pattern if it is using mutability in a safe
way?

The answer has two parts.

First, *internally* by being disciplined, we can avoid entire classes of bugs that *can* arrise inadvertantly due to the use of mutability.  While the example above doesn't have any immediate issues related to mutability, it isn't unreasonable to imagine a refactor to the function above which ends up introducing a bug due to the use of mutability.

Second, *externally* we want to set an easy to follow example.  We have plenty
of 3rd party contributors and they take cues on how to write new code based on
existing code.  Rather than try to define rules on what constitutes
*acceptable* use of mutability, it is much simpler for the rule to be
"immutable* by default unless there is an explicit reason to use mutability.


### Caching

Another benefit of immutable objects is that they play nicely with caching as
well as sharing them across processes.


## When can I use mutability

Anytime you have an explicit reason to do so.  Some good reasons are:

- Performance critical code
- Readability


## Types of multability and how to avoid it.


### Programatic generation of various collection data structures

The use of lists

```python
# list
def get_things(all_items):
    things = []
    for item in all_items:
        things.append(item)
    return things
```

We use `tuple` in place of lists and generators or comprehensions in place of
`list.append` for programatic consprogramatic.


```python
# using a comprehension
def get_things(all_items):
    return tuple(item for item in all_items)

# using a generator
def get_things(all_items):
    for item in all_items:
        yield item
```

The `get_things` function above will produce a generator which can be a
*foot-gun*. eg~ if you try to iterate the result twice, it will be empty on the 2nd round.
You can either use the
[`eth_utils.to_tuple`](https://eth-utils.readthedocs.io/en/latest/utilities.html#to-tuple-callable-callable-tuple)
decorator or a thin wrapper pattern to force the generator to evaluate like this.


```python
@to_tuple
def get_things(all_items):
    for item in all_items:
        yield item


# or a thin wrapper
def get_things(all_items):
    return tuple(_get_things(all_items))

def _get_things(all_items):
    for item in all_items:
        yield item
```


> This approach for `list` objects also translates to `set` objects.


The same applies to dictionaries.  While python doesn't have an immutable
dictionary, we tend to avoid mutating dictionaries.


```python
# avoid this pattern
def get_result(all_items):
    result = {}
    for key, value in all_items:
        result[key] = value
```

Avoiding mutation can be done either using a comprehension, the generator
pattern, or using the [`toolz` library
APIs](https://toolz.readthedocs.io/en/latest/api.html#dicttoolz) for doing
dictionary mergers or key/value filtering/mapping.


```python
# comprehension
def get_result(all_items):
    return {key: value for key, value in all_items}

# generator (using either thin wrapper)
def get_result(all_items):
    return dict(_get_result(all_items))

    def _get_result(all_items):
    for key, value in all_items:
        yield key, value

# generator (using `eth_utils.to_dict`)
@to_dict
def get_result(all_items):
    for key, value in all_items:
        yield key, value
```
