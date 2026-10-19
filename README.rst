About
=====

.. note::

   This fork restructures the documentation of ``cantools`` according to the
   `Diátaxis <https://diataxis.fr>`_ framework. The original project is
   https://github.com/cantools/cantools.

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

The documentation at https://cantools-diataxis.readthedocs.io is organized in four parts:

- `Tutorials <https://cantools-diataxis.readthedocs.io/en/latest/tutorials/index.html>`_:
  install ``cantools`` and decode and encode a first CAN frame.

- `How-to <https://cantools-diataxis.readthedocs.io/en/latest/how-to/index.html>`_:
  solve specific tasks, such as decoding ``candump`` output, scripting
  with the Python API and setting up a virtual CAN interface.

- `Reference <https://cantools-diataxis.readthedocs.io/en/latest/reference/index.html>`_:
  Python API and command line interface.

- `Explanation <https://cantools-diataxis.readthedocs.io/en/latest/explanation/index.html>`_:
  how CAN databases describe messages and how ``cantools`` is built.

Contributing and license
========================

Guidelines for contributions are in `CONTRIBUTING.rst
<https://github.com/katrintalkenberger/cantools/blob/HEAD/CONTRIBUTING.rst>`_.
The code follows the `coding style
<https://github.com/katrintalkenberger/cantools/blob/HEAD/CODING_STYLE.rst>`_.

``cantools`` is released under the `MIT license
<https://github.com/katrintalkenberger/cantools/blob/HEAD/LICENSE>`_.

.. _DBC: http://www.socialledge.com/sjsu/index.php?title=DBC_Format

.. _KCD: https://github.com/julietkilo/kcd

.. _tester: https://cantools-diataxis.readthedocs.io/en/latest/reference/api/tester.html#cantools.tester.Tester
