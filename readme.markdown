This is a fsstore, an easy way to use dict-like objects for file I/O. 

fsstore _isn't_ intended to be a replacement for databases. You can't easily (efficiently) search for objects with x attribute. There are only two types of objects -- strings and dicts of strings, which map to files and directories.

Instead, it's supposed to be an alternative to full-fledged databases for when you only need key-value pairs with a tiny api.

It's pretty simple: 

````python
# -*- coding: utf-8 -*-

from fsstore import Store


# initialize a Store in a directory called "testing"
s = Store("testing")

# save a string to a file
s["first_file"] = "Hello, fsstore."

# create an empty directory
s["first_dir"] = {}

# create a non-empty directory
s["second_dir"] = {
    "one": "1, I, i, one",
    "two": "2, II, ii, two",
}

# update a directory
s["first_dir"].update({
    "first_dir_file": "hello, world."
})

# get the path of a file or directory (it doesn't necessarily have to exist yet)
s.get_path("first_file")
# >> "testing/first_file"

# get a file object, in case you want it for iterating over
with s.get_file("first_file", "r") as f:
    for line in f:
        print line

# or if you want to use a function that requires a file-like object
import json
with s.get_file("second_file", "w") as f:
    json.dump([1, 2, 3], f, indent=4)

# a word of caution -- always encode `unicode` objects before storing them
s["unicode"] = {
    u"α".encode("utf-8"): "alpha",
    u"β".encode("utf-8"): "beta",
    u"γ".encode("utf-8"): "gamma",
}
````

And here's what the directory tree looks like after all of that:

````
testing
├── first_dir
│   └── first_dir_file
├── first_file
├── second_dir
│   ├── one
│   └── two
├── second_file
└── unicode
    ├── α
    ├── β
    └── γ
````

## Installation

You can install fsstore from PyPI:

````sh
pip install fsstore
````

or directly from this git repository:

````sh
pip install git+https://startling@github.com/startling/fsstore.git
````
