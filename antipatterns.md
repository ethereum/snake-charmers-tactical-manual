# Antipatterns

This is a collection of [antipatterns](https://en.wikipedia.org/wiki/Anti-pattern) which we actively avoid.


## Boolean checks on objects as functions.


We try to avoid implementing boolean checks on objects as functions, instead preferring `@property` methods when appropriate.


```python
class Thing:
    def is_active(self):
        ...


def get_active_things(all_things):
    return tuple(thing for thing in all_things if thing.is_active)
```

Can you spot the bug in the code above?  It should be:



```python
def get_active_things(all_things):
    return tuple(thing for thing in all_things if thing.is_active())
```

Note that we needed to call `thing.is_active()`.  The nefarious part of this is
that the first code will not throw any errors, but will just return the
entirety of the original data set.  This is due to the python semantics that
function objects evalute truthy.

For this reason, when implementing methods like this, we prefer to make them
`@property` methods.


```python
class Thing:
    @property
    def is_active(self):
        ...

# correct
thing.is_active

# incorrect: throws a `TypeError`
thing.is_active()
```

This pattern ensures that incorrect usage of the API throws an exception rather
than silently doing the wrong thing.


## Return value checking


We prefer raising exceptions over return value checking.  Here is an example that requires return value checking.


```python
def get_user(user_id):
    if orm.user_exists(user_id):
        return orm.get_user(user_id)
    else:
        return None


# some "view" function in a web application 
def user_profile_view(request, user_id):
    user = get_user(user_id)
    return template.render({'user': user})
```

The `get_user` function above can return either a user, or `None` if no user
was found.  This pattern requires all consumers of this API to check the return
value, an action which is easy to forget to do.  This often results in
unexpected errors happening some distance away from the call to `get_user` when
a `None` value is used for something that expects a user object.  These types
of bugs can be difficult and confusing to diagnose and can often remain hidden
in an application until the right situation arises to trigger it.  It is also
easy for the *checking* of the return value to accidentally be removed since
those checks are often only loosely coupled.

Instead of returning `None` we should instead raise an exception.  This changes
the requirement for API consumers to include exception handling instead of
return value checking.  In the case that a call site forgets to add the proper
exception handling, the error is raised at the call site.



```python
def get_user(user_id):
    if orm.user_exists(user_id):
        return orm.get_user(user_id)
    else:
        raise Exception("User not found")
```


## Validation functions should probably not have a return value

This is an extension of the "no return value checking" antipattern.



```python
# bad: requires return value checking
def validate_age(user):
    if user.age >= 18:
        return True
    else:
        return False

# good
def validate_age(user):
    if user.age < 18:
        raise Exception("User must be 18 or older")
```

In the case where you need both *validation* and a check, they should be
implemented separately, or in combination like follows.


```python
def check_is_old_enough(user):
    return user.age >= 18

def validate_age(user):
    if not check_is_old_enough(user):
        raise Exception("user must be 18 or older")
```

## Assertions in runtime code.

For Reference: https://stackoverflow.com/questions/1273211/disable-assertions-in-python


```python
class User:
    def __init__(self, name, age):
        assert len(name) > 1
        assert age >= 18
        ...
```

A common pattern for doing validation is to use `assert` statements.  We choose
to use explicit exception raising due to how easy it is to run the python
interpreter with assertions disabled.


```python
class User:
    def __init__(self, name, age):
        if len(name) > 1:
            raise AssertionError(...)
        if age < 18:
            raise AssertionError(...)
        ...
```

Replacing `assert` statements with `raise AssertionError(...)` (or whatever
exception class you prefer) ensures that these checks cannot be trivially
disabled.
