Coding style
============

The coding style for this package is defined as below. The rules are
based on my personal preference.

- Blank lines before and after statements (if, while, return, ...)
  (1), unless at beginning or end of another statement or file (8).

- Two blank lines between file level definitions (2).

- Space before and after operators (3), except for keyword arguments
  where no space is allowed (4).

- One import per line (5).

- Comments and doc strings starts with capital letter and ends with a
  period, that is, just as sentences (6).

- Blank line after doc strings (7).

- Maximum line length of 90 characters, but aim for less than 80.

- All function arguments on one line, or one per line.

- Class names are CamelCase. Underscore is not allowed.

- Function and variable names are lower case with underscore
  separating words.

  .. code-block:: python

     import sys
     from os import path                     # (5)
     from os import getcwd                   # (5)
                                             # (2)
                                             # (2)
     def foo(bars, fum=None):                # (4)
         """This is a doc string.            # (6)

         """
                                             # (7)
         fies = []                           # (3)
         kam = path.join(getcwd(), '..')
                                             # (1)
         for bar in bars:
             if len(bar) == 1):              # (8)
                 fies.append(ham + 2 * bar)  # (3)
                                             # (1)
         # This is a comment.                # (6)
         if fum in None:
             fum = 5                         # (3)
         else:
             fum += 1                        # (3)
                                             # (1)
         fies *= fum                         # (3)
                                             # (1)
         return fies
                                             # (2)
                                             # (2)
     def goo():
         return True
