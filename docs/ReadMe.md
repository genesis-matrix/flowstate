
# Table of Contents

1.  [Purpose](#org093a68d)
2.  [Usage](#orgcfa1856)
3.  [Installation](#org9d2b623)
    1.  [dot](#org9f1523d)
4.  [Running tests](#orgf7705d9)
5.  [Acknowledgements](#orgd16acd8)



<a id="org093a68d"></a>

# Purpose

FlowState is a tool for visualising the runtime dependency graph of a [Salt](https://github.com/saltstack/salt) highstate.

It takes the json output of `state.show_highstate` or `state.show_sls`
from Salt and produces a program written in [dot](http://www.graphviz.org/doc/info/lang.html), which is a tool from
Graphviz for defining an acyclic graph.

States are nodes and dependencies are edges. `require` and `require_in` are blue; `watch` and `watch_in` are red.

Some examples, with the dot programs rendered as png:

![img](http://i.imgur.com/wETR0WG.png)

![img](http://i.imgur.com/LJ6ckzr.png)


<a id="orgcfa1856"></a>

# Usage

The tool expects the JSON output of `show_highstate` or `show_sls` over stdin.
It outputs `dot` code representing the dependency graph. The dot code defines a
single digraph called "states".

For example, if you have a salt master running:

    salt 'minion1' state.show_highstate --out json | flowstate

You can write the dot output to a file, or pipe it further into the \`dot\` tool
from GraphViz. In this example, we produce a png called \`output.png\`:

    salt 'minion*' state.show_highstate --out json \
          | flowstate \
          | dot -Tpng -o output.png

The tool currently only supports rendering the output from one minion. If you
give it output with two minions, it'll report an error.


<a id="org9d2b623"></a>

# Installation

Install directly from PyPi:

    pip install flowstate

Or clone this repository and use `setup.py`:

    python setup.py install

Unfortunately, the version of the `pydot` package in PyPi doesn't work in
Python 2.7+. We're working on getting this up to date. In the meantime, please
use this fork to install the \`pydot\` package first:

<https://github.com/nlhepler/pydot>


<a id="org9f1523d"></a>

## dot

Most operating systems have a package for it, usually as part of GraphViz.

-   **Debian / Ubuntu**

        apt-get install graphviz

-   **OSX with homebrew**

        brew install graphviz


<a id="orgf7705d9"></a>

# Running tests

The test runner is [tox](https://tox.readthedocs.org/en/latest/). Run it from the root of the project. It will
run tests for a number of different Python versions, as well as
Flake8:

$ tox -l
py36
pypy
flake8

This will run all of the tests in python 2.7, 3.4 and pypy.


<a id="orgd16acd8"></a>

# Acknowledgements

-   [FlowState](https://github.com/genesis-matrix/flowstate) is forked from the [salt-state-graph](https://github.com/ceralena/salt-state-graph) project by [@ceralena](https://github.com/ceralena).
