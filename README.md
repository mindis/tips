Command-line arguments in Python show up in `sys.argv` as a list of strings (so you'll need to import the `sys` module).

For example, if you want to print all passed command-line arguments:

```python
import sys
print(sys.argv)  # Note the first argument is always the script filename.
```

Command-line options are sometimes passed by position (e.g. `myprogram foo bar`) and sometimes by using a "-name value" pair (e.g. `myprogram -a foo -b bar`).

Here's a simple way to parse command-line pair arguments. It scans the `argv` list looking for `-optionname optionvalue` word pairs and places them in a dictionary for easy retrieval. The code is heavily commented to help Python newcomers.

```python
"""Collect command-line options in a dictionary"""

def getopts(argv):
    opts = {}  # Empty dictionary to store key-value pairs.
    while argv:  # While there are arguments left to parse...
        if argv[0][0] == '-':  # Found a "-name value" pair.
            opts[argv[0]] = argv[1]  # Add key and value to the dictionary.
        argv = argv[1:]  # Reduce the argument list by copying it starting from index 1.
    return opts

if __name__ == '__main__':
    from sys import argv
    myargs = getopts(argv)
    if '-i' in myargs:  # Example usage.
        print(myargs['-i'])
    print(myargs)
```

Running this script:

```shell
$ python main.py -i input.txt -o output.txt
input.txt
{'-o': 'output.txt', '-i': 'input.txt'}
```

Simple solution, but not very robust; it doesn't handle error checking and the like. So don't use this in production code! There are more complex alternatives available. Some modules to consider are:

* `getopt`
* `optparse` (deprecated since Python 2.7)
* `argparse` (recommended if you want something in the standard library)
* [`docopt`](http://docopt.org/) (recommended if you're willing to use something not in the standard library)

This post was inspired by the book "Programming Python" by Mark Lutz.
