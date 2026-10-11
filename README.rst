|github-actions| |coverage|

About
=====

CAN BUS tools in Python 3.

- `DBC`_, `KCD`_, SYM, ARXML 3&4 and CDD file parsing.

- CAN message encoding and decoding.

- Simple and extended signal multiplexing.

- Diagnostic DID encoding and decoding.

- ``candump`` output decoder.

- Node `tester`_.

- `C` source code generator.

- CAN bus monitor.

- Graphical plots of signals.

Project homepage: https://github.com/cantools/cantools

Documentation: https://cantools.readthedocs.io

Installation
============

.. code-block:: bash

    python3 -m pip install cantools




.. |github-actions| image:: https://github.com/cantools/cantools/actions/workflows/pythonpackage.yml/badge.svg?branch=master
   :target: https://github.com/cantools/cantools/actions/workflows/pythonpackage.yml
   :alt: Github Actions workflow status

.. |coverage| image:: https://coveralls.io/repos/github/cantools/cantools/badge.svg?branch=master
   :target: https://coveralls.io/github/cantools/cantools?branch=master
   :alt: Test coverage reports on Coveralls.io

.. _DBC: http://www.socialledge.com/sjsu/index.php?title=DBC_Format

.. _KCD: https://github.com/julietkilo/kcd

.. _tester: http://cantools.readthedocs.io/en/latest/#cantools.tester.Tester
