# sphinx-ext-dynaminc

A Sphinx extension that allows you to include the same content in multiple documents while avoiding duplicate label warnings. This is particularly useful when you need to reuse documentation sections across different contexts.

## How It Works

The extension provides a `.. dynamic-include::` directive that extends Sphinx's standard `..include::` functionality. When you include a file, specify a namespace to substitute in labels in the included content. You can include the same content multiple times with different namespaces, thus avoiding duplicate label warnings.

## Installation

```bash
pip install sphinx-ext-dynaminc
```

Or for manual installation:

1. Copy `dynamic_include.py` into your Sphinx project's `_extensions` directory
2. Add the extension to your `conf.py`:
   ```python
   extensions = [
       "dynamic_include",
       # ... your other extensions
   ]
   ```
## Usage

(This is the way I'm using this extension. It may work in other ways.)

### Example file structure

```
docs/
└── _include/
    └── shared_content.rst
└── dir_a/
    └── doc_a.rst
└── dir_b/
    └── doc_b.rst
```

### shared_content.rst

```rst
Title
-----

Some content.

_|namespace|_label:

Sub-title
^^^^^^^^^

More content
```

### doc_a

```rst
.. _doc_a:

Title
=====

.. dynamic-include:: _include/shared_content.rst
   :namespace: doc_a

```

### doc_b

```rst
.. _doc_b:

Title
=====

Some unique content.

.. dynamic-include:: _include/shared_content.rst
   :namespace: doc_b

```

### Updating conf.py

To prevent errors related to the `_include` subdirectory (or wherever you're keeping the shared files), add it to your exclude patterns in conf.py. Alternatively, you could choose to use a different file extension for shared content (like .inc).

```python
exclude_patterns = [
    '_include',
    '*.inc',
    # ... other exclude patterns
]
```

## Requirements

- Python 3.6+
- Sphinx 4.0+

## Future work

* Allow custom substitution word in case of conflicts with "namespace"
