Contributing
============

Code Contributions
------------------

#. Fork the repository.

#. Install prerequisites. You can skip this if you use `tox`_.

   .. code-block:: text

      python3 -m pip install -e .[dev]

#. Implement the new feature or bug fix.

#. Implement test case(s) to ensure that future changes do not break
   legacy.

#. Run the linters

   .. code-block:: text

      ruff check src
      mypy src

   or

   .. code-block:: text

      tox -e ruff
      tox -e mypy

#. Run the tests.

   .. code-block:: text

      tox -e py

#. Check test coverage.

   .. code-block:: text

      tox -e cov
      firefox htmlcov/index.html

#. Create a pull request.

.. _tox: http://tox.readthedocs.org/

Documentation Contributions
---------------------------

To build the documentation, Sphinx and the ``cantools`` package must both be
installed. ``docs/conf.py`` reads the package version from the installed
package.

.. code-block:: text

   python3 -m pip install -e . -r docs/requirements.txt

Build the documentation with Sphinx:

.. code-block:: text

   python3 -m sphinx -b html docs docs/_build/html

Open ``docs/_build/html/index.html`` in a browser to check the result.

