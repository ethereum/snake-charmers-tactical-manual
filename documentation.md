# Documentation

Good documentation will lead to quicker adoption and happier users. It should
also lead to fewer requests for help and support. Documentation also serves to
draw a clear line between public and private APIs.

There is no one-size-fits-all approach to good documentation. Whether a documentation
is considered good or not still remains a highly subjective thing after all.

With that in mind, we still try to set up a rough guideline on how to create solid
documentation for the software that we produce.

## Tooling

We use [`Sphinx`](http://www.sphinx-doc.org/en/stable/) to create documentation for
all Python projects. Sphinx is the de facto documentation generator in the Python
eco-system because it provides lots of nice features specific to documenting Python
code.

We tend to host our documentation using the [ReadTheDocs](https://readthedocs.com/)
service.

The `README.md` file should always have a link to the hosted documentation.

### Robust examples

Documentation that contains broken examples is **very** annoying for the reader. Therefore it is
required to have a dedicated CI job that runs `doctest` to validate the correctness of the
documentation.

#### `doctest` code examples

Code examples can be written using `.. doctest::` code blocks to ensure they are executed as
regular code during the `doctest` CI run.

Examples that use this feature are effectively strucured as if they were written in an interactive
Python session.

Example:

```
.. doctest::

  >>> current_vm = chain.get_vm()
  >>> account_db = current_vm.state.account_db
  >>> account_db.get_balance(SOME_ADDRESS)
  10000000000000000000000
```

#### `literalinclude` code examples

Sometimes, writing code examples using only the `.. doctest::` directive can feel a bit limiting
and effecively clash with other desired properties of the example (e.g. order, brevity). There's
another effective way to guarantee examples in the documentation continue to work as the
underlying software evolves.

We can write examples as standalone code examples that are tested through unit tests and then use
the `.. literalinclude::` directive to take fine-grained control over the exposed parts that get
exposed in the documentation.

Example:

```
.. literalinclude:: ../../../trinity-external-plugins/examples/peer_count_reporter/peer_count_reporter_plugin/plugin.py
   :language: python
   :pyobject: PeerCountReporterPlugin
   :end-before: def configure_parser
```

## General structure

The following structure serves as a rough generalization on how to break down
the documentation in different sections. It also achieves a side navigation
that looks clean and makes it easy to navigate.

**General**
- Introduction
- Quickstart
- Release Notes

**Fundamentals**
- API
- Guides
- Cookbook (optional)

**Community**
- Contributing
- Code of Conduct


### General

#### Introduction

This is the main landing page. It should contain:

- Abstract (What is this?)
- Goals (What do we want?)
- Status (Where are we?)
- Further reading (Links)
  - Quickstart
  - Source Code
  - Communication channels
  - Getting involved

#### Quickstart

The Quickstart is a very simple hands-on guide, sketching a **happy path** scenario for
those who have just discovered the project. It demos how to install the software and
how to use it in the most simplest way possible.

It should also contain links to tell the reader what to explore next. Notice that the
Quickstart is really just a guide (see Guides section further down) that is referenced
in the side navigation as well as in the introduction to be exposed more prominently.

#### Release Notes

The release notes inform the reader about the progress of the project. They are continously
written as new releases are published.

### Fundamentals

#### API

API docs are the first level of documentation. They are the closest to actual source code and
simply document how an API works. They do not necessarily give a good perspective about the bigger
picture though.

##### Docstrings

Public APIs are expected to be annotated with docstrings as seen in the following example.

```python

def add_transaction(self,
                    transaction: BaseTransaction,
                    computation: BaseComputation,
                    block: BaseBlock) -> Tuple[Block, Dict[bytes, bytes]]:
        """
        Add a transaction to the given block and
        return `trie_data` to store the transaction data in chaindb in VM layer.

        Update the bloom_filter, transaction trie and receipt trie roots, bloom_filter,
        bloom, and used_gas of the block.

        :param transaction: the executed transaction
        :param computation: the Computation object with executed result
        :param block: the Block which the transaction is added in

        :return: the block and the trie_data
        """
```

Docstrings are written in reStructuredText and allow certain type of directives.

The `:param:` and `:return:` directives are being used to describe parameters as well as the return
value. Usage of `:type:` and `:rtype:` directives is discouraged as sphinx automatically reads
and displays the types from the source code type definitions making any further use of
`:type:` and `:rtype:` obsolete and unnecessarily verbose.


Whenever other APIs in the form of classes or methods are mentioned we should use the linking
syntax as it will allow the reader to jump to the corresponding API docs.

Example: `:class:`\`~eth_typing.misc.Address`

##### Grammar

We use imperative, present tense to describe APIs: “return” not “returns”

One way to test if we have it right is to complete the following sentence.

If we call this API it will: __________________________

If that sounds right, it probably is.

##### Doc generation

Sphinx can generated the API docs directly from the embedded docstrings in the source code. The
`rst` files for API docs are mostly concerned about how the order of the described types or short
intro paragraphs.

```
Chain
=====

BaseChain
---------

.. autoclass:: eth.chains.base.BaseChain
  :members:

Chain
-----

.. autoclass:: eth.chains.base.Chain
  :members:
```

#### Guides

While solid API documentation is very important for those who know what they are looking for,
it does not cover all the essential aspects of the entire documentation spectrum.

Often, a narrative style documentation that walks the user through specific use cases is more user
friendly. This is especially true for those who are not yet familiar with the software and
therefore lack a good feel for possibly existing APIs. 

##### Narrative Perspective

Each narrative is written from a particular point of view and it is important to remain consistent
in its use.

There are two main styles of writing that are generaly used for technical documentation and there
really is no *right* or *wrong* when it comes to picking one over the other.

Here are two very prominent examples:

*React choose to use Second-person "You" form*

![image](https://user-images.githubusercontent.com/521109/48718244-47f04180-ec1b-11e8-9978-85173b4e78e3.png)

*Rust choose to use First-person "We" form*

![image](https://user-images.githubusercontent.com/521109/48718325-70783b80-ec1b-11e8-8665-dd892edc1a51.png)

One might argue that the *Second-person "You" form* is more casual as it directly speaks to the
reader whereas the *First-person "We" form* is more formal and more popular among scientic writing.

We use the *First-person "We" form* only.

Examples:

**Correct**
> We can send the transaction after we signed it.

**Wrong**

> You can send the transaction after you signed it.

**Wrong**

> I can send the transaction after I signed it.

##### Writing problem driven

It's tempting to write guides that drop the user right within a solution that highlights exciting
features. While this may make a lot of sense for the writer, often readers will find themselves
wondering: "Wait!? **Why** are we doing this in the first place?"

Therefore, it's a good idea for guides to be written in a *problem-driven* manner whenever possible.
We want the the reader to first understand **why** they want to read the piece that they are about
to dive in. What is the problem that we will learn how to fix or what are the possible use cases
for the features that we are about to learn.


#### Cookbook

Between concise API docs and lengthy guides, there's often room for a third format of
documentation. A cookbook is a collection of small recipes that demo the use of APIs through short
examples. Things that do not need a full grown guide but also aren't entirely clear by just looking
at the API docs are perfect candidates to go into a cookbook.

Writing cookbooks takes less effort compared to writing guides and it's an effective tactic to
start collecting short examples in a cookbook and later turn some of them into more in-depth
guides.

### Community

#### Contributing

This section should describe how to get involved.

- Setting up a work environment
- Running the tests
- Sending PRs
- Developer communication channels

#### Code of Conduct

In the interest of fostering an open and welcoming environment, we as contributors and maintainers
pledge to making participation in our project and our community a harassment-free experience for
everyone. Therefore, every project documentation siteshould contain
[this](https://www.contributor-covenant.org/version/1/4/code-of-conduct.html) code of conduct.
