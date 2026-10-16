|github-actions| |coverage|

About
=====

``cantools`` is a Python 3 library and command line tool for working with
Controller Area Network (CAN) bus data. It parses network databases, encodes
and decodes messages, and tests nodes on a CAN bus.

Key features:

- Parsing of `DBC`_, `KCD`_, SYM, ARXML 3&4 and CDD files.

- Creating and extending databases from files or strings, and writing them
  back as DBC, KCD or SYM.

- Encoding of signal values into the data bytes of a CAN frame and decoding
  of data bytes into signal values, including simple and extended signal
  multiplexing.

- Diagnostic DID encoding and decoding.

- Node `tester`_ for testing a node on a CAN bus.

- Command line tool with the subcommands ``decode`` (decodes ``candump``
  output), ``plot``, ``monitor``, ``dump``, ``list``, ``convert`` and
  ``generate_c_source`` (C source code generator).

Project homepage: https://github.com/cantools/cantools

Installation
============

.. code-block:: bash

    python3 -m pip install cantools

Documentation
=============

The documentation at https://cantools.readthedocs.io is organized in four parts:

- `Tutorials <https://cantools.readthedocs.io/en/latest/tutorials/index.html>`_:
  install ``cantools`` and decode and encode a first CAN frame.

- `How-to <https://cantools.readthedocs.io/en/latest/how-to/index.html>`_:
  solve specific tasks, such as decoding ``candump`` output, scripting
  with the Python API and setting up a virtual CAN interface.

- `Reference <https://cantools.readthedocs.io/en/latest/reference/index.html>`_:
  Python API and command line interface.

- `Explanation <https://cantools.readthedocs.io/en/latest/explanation/index.html>`_:
  how CAN databases describe messages and how ``cantools`` is built.

.. |github-actions| image:: https://github.com/cantools/cantools/actions/workflows/pythonpackage.yml/badge.svg?branch=master
   :target: https://github.com/cantools/cantools/actions/workflows/pythonpackage.yml
   :alt: Github Actions workflow status

.. |coverage| image:: https://coveralls.io/repos/github/cantools/cantools/badge.svg?branch=master
   :target: https://coveralls.io/github/cantools/cantools?branch=master
   :alt: Test coverage reports on Coveralls.io

.. _DBC: http://www.socialledge.com/sjsu/index.php?title=DBC_Format

.. _KCD: https://github.com/julietkilo/kcd

.. _tester: http://cantools.readthedocs.io/en/latest/#cantools.tester.Tester

Contributing and license
========================

Contributions are welcome. Read `CONTRIBUTING.rst <https://github.com/cantools/cantools/blob/master/CONTRIBUTING.rst>`_ before opening a pull request, and follow the `coding style <https://github.com/cantools/cantools/blob/master/CODING_STYLE.rst>`_.

``cantools`` is released under the `MIT license <https://github.com/cantools/cantools/blob/master/LICENSE>`_.
