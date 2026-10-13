cantools command
================

The ``cantools`` command provides utilities for CAN bus data. It consists of subcommands that are selected by name. Each subcommand has its own arguments and options.

.. code-block:: text

   cantools [-h] [-d] [--version] <subcommand> ...

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Subcommand
     - Purpose
   * - :ref:`decode <cli-decode>`
     - Decode ``candump`` frames read from standard input.
   * - :ref:`plot <cli-plot>`
     - Plot signals decoded from ``candump`` frames.
   * - :ref:`monitor <cli-monitor>`
     - Monitor CAN bus traffic in a text based user interface.
   * - :ref:`dump <cli-dump>`
     - Print a database in a human readable format.
   * - :ref:`list <cli-list>`
     - Print database contents in an easily processed format.
   * - :ref:`generate_c_source <cli-generate_c_source>`
     - Generate C source code from a database.
   * - :ref:`convert <cli-convert>`
     - Convert a database from one format to another.

Global options
--------------

.. program:: cantools

.. option:: -h, --help

   Show the help message and exit.

.. option:: -d, --debug

   Enable debug logging. Exceptions are not converted into a one line error message.

.. option:: --version

   Print version information and exit.

.. _cli-decode:

``decode``
----------

.. program:: cantools decode

Decode "candump" CAN frames read from standard input and print them in a human readable format.

.. code-block:: text

   cantools decode [-h] [-c] [-t] [-s] [-e ENCODING] [--prune] [--no-strict] [-m FRAME_ID_MASK] database

**Positional arguments**

``database``
   Database file.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -c, --no-decode-choices

   Do not convert scaled values to choice strings.

.. option:: -t, --no-decode-containers

   Do not decode container messages.

.. option:: -s, --single-line

   Print the decoded message on a single line.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

.. option:: -m FRAME_ID_MASK, --frame-id-mask FRAME_ID_MASK

   Only compare selected frame id bits to find the message in the database. By default the candump and database frame ids must be equal for a match.

**Input and output**

Input: ``candump`` lines on standard input.

Output: Decoded messages on standard output.

.. _cli-plot:

``plot``
--------

.. program:: cantools plot

Decode ``candump`` CAN frames, or the output of ``cantools decode``, read from standard input and plot them using matplotlib.

Select the signals to plot by passing them as arguments. Each signal argument has the pattern ``[bo.]sg[:fmt]``:

``bo``
   Name of the message. Defaults to ``*`` if omitted.

``sg``
   Name of the signal.

``fmt``
   Format of the graph, passed to matplotlib (line style, markers, color). If the first
   character of ``fmt`` is ``|``, a stem plot is used instead of a line plot.

The wildcards ``*`` (any number of arbitrary characters) and ``?`` (exactly one arbitrary character) can be used in ``bo`` and ``sg``.

Separate signals with ``-`` to show them in different subplots. Separate signals with ``,`` to assign them to different vertical axes in the same subplot (see ``--auto-color-ylabels``). All signals share the same horizontal axis.

The following ``fmt`` characters can be combined (see the `matplotlib.pyplot.plot documentation <https://matplotlib.org/api/_as_gen/matplotlib.pyplot.plot.html>`_ for the complete list):

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Category
     - Values
   * - Line style
     - ``-`` solid, ``--`` dashed, ``-.`` dash-dot, ``:`` dotted
   * - Marker
     - ``.`` point, ``,`` pixel, ``o`` circle, ``s`` square, ``D`` diamond, ``x`` x marker, and others
   * - Color
     - ``b`` blue, ``g`` green, ``r`` red, ``c`` cyan, ``m`` magenta, ``y`` yellow, ``k`` black, ``w`` white, ``C0`` to ``C9`` colors of the current style

.. code-block:: text

   cantools plot [-h] [-c] [-e ENCODING] [-m FRAME_ID_MASK] [-I] [-l] [-t BREAK_TIME] [--show-invalid-syntax] [--show-unknown-frames] [--show-invalid-data] [-s] [--ignore-invalid-syntax] [--ignore-unknown-frames] [--ignore-invalid-data] [-q] [-o OUTPUT_FILE] [-ss START] [-to STOP] [--style STYLE] [--list-styles] [-ac] [--prune] [--no-strict] [--title TITLE] [--color COLOR] [--xlabel XLABEL] [--ylabel YLABEL] [--ymin YMIN] [--ymax YMAX] [--no-units] database [signals ...]

**Positional arguments**

``database``
   Database file.

``signals ...``
   The signals to be plotted.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -c, --no-decode-choices

   Do not convert scaled values to choice strings.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding of dbc file.

.. option:: -m FRAME_ID_MASK, --frame-id-mask FRAME_ID_MASK

   Only compare selected frame id bits to find the message in the database. By default the candump and database frame ids must be equal for a match.

.. option:: -I, --case-sensitive

   Match the signal names case sensitive.

.. option:: -l, --line-numbers

   Use line numbers instead of time stamps on the horizontal axis (useful with ``candump -td``).

.. option:: -t BREAK_TIME, --break-time BREAK_TIME

   If the time distance between two consecutive signals is longer than this value the line in the plot will be interrupted. The value is given in seconds (if timestamps are used) or input lines (if line numbers are used). -1 means infinite. Default: ``100``.

.. option:: --show-invalid-syntax

   Show a marker for lines which could not be parsed. This implies -l.

.. option:: --show-unknown-frames

   Show a marker for messages which are not contained in the database file.

.. option:: --show-invalid-data

   Show a marker for messages with data which could not be parsed.

.. option:: -s, --show-errors

   Show all error messages in the plot. This is an abbreviation for all --show-* options. This implies -l.

.. option:: --ignore-invalid-syntax

   Don't print an error message for lines which could not be parsed.

.. option:: --ignore-unknown-frames

   Don't print an error message for messages which are not contained in the database file.

.. option:: --ignore-invalid-data

   Don't print an error message for messages with data which could not be parsed.

.. option:: -q, --quiet

   Don't print any error messages. This is an abbreviation for all --ignore-* options.

.. option:: -o OUTPUT_FILE, --output-file OUTPUT_FILE

   A file to write the plot to instead of displaying it in a window.

.. option:: -ss START, --start START

   A start time or line number. Everything before is ignored. This filters the lines/messages to be processed. It does *not* set the minimum value of the x-axis.

.. option:: -to STOP, --stop STOP

   An end time or line number. Everything after is ignored. This filters the lines/messages to be processed. It does *not* set the maximum value of the x-axis.

.. option:: --style STYLE

   The matplotlib style to be used.

.. option:: --list-styles

   Print all available matplotlib styles without drawing a plot.

.. option:: -ac, --auto-color-ylabels

   This is equivalent to applying --color C0 to the first y-axis, --color C1 to the second and so on.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

.. option:: --title TITLE

   Title of the plot.

.. option:: --color COLOR

   The color to be used for the y-label and the signals (unless a different color is given for the signal). All string formats explained in the following link are allowed: https://matplotlib.org/tutorials/colors/colors.html.

.. option:: --xlabel XLABEL

   Label of the horizontal axis.

.. option:: --ylabel YLABEL

   Label of the vertical axis.

.. option:: --ymin YMIN

   Lower limit of the vertical axis.

.. option:: --ymax YMAX

   Upper limit of the vertical axis.

.. option:: --no-units

   Do not show units in legend labels.

**Input and output**

Input: ``candump`` lines or the output of ``cantools decode`` on standard input.

Output: A matplotlib window, or the file given by ``-o``.

.. _cli-monitor:

``monitor``
-----------

.. program:: cantools monitor

Monitor CAN bus traffic in a text based user interface.

.. code-block:: text

   cantools monitor [-h] [-s] [-e ENCODING] [-m FRAME_ID_MASK] [--prune] [--no-strict] [--filter-regex FILTER_REGEX] [-c CHANNEL] [-i INTERFACE] [-b BITRATE] [--fd] [--data-bitrate DATA_BITRATE] [--timing TIMING_ARG [TIMING_ARG ...]] [--filter FILTER [FILTER ...]] [--bus-kwargs BUS_KWARG [BUS_KWARG ...]] database

**Positional arguments**

``database``
   Database file.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -s, --single-line

   Print the decoded message on a single line.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding.

.. option:: -m FRAME_ID_MASK, --frame-id-mask FRAME_ID_MASK

   Only compare selected frame id bits to find the message in the database. By default the received and database frame ids must be equal for a match.

.. option:: --prune

   Refrain from shortening the names of named signal values.

.. option:: --no-strict

   Skip database consistency checks.

.. option:: --filter-regex FILTER_REGEX

   Use given filter regex.

.. option:: -c CHANNEL, --channel CHANNEL

   Most backend interfaces require some sort of channel. For example with the serial interface the channel might be a rfcomm device: "/dev/rfcomm0". With the socketcan interface valid channel examples include: "can0", "vcan0".

.. option:: -i INTERFACE, --interface INTERFACE

   Backend CAN interface to use. If omitted, python-can reads the interface from its configuration files. Valid values: ``canalystii``, ``cantact``, ``etas``, ``gs_usb``, ``iscan``, ``ixxat``, ``kvaser``, ``neousys``, ``neovi``, ``nican``, ``nixnet``, ``pcan``, ``robotell``, ``seeedstudio``, ``serial``, ``slcan``, ``socketcan``, ``socketcand``, ``systec``, ``udp_multicast``, ``usb2can``, ``vector``, ``virtual``.

.. option:: -b BITRATE, --bitrate BITRATE

   Bitrate to use for the CAN bus.

.. option:: --fd

   Activate CAN-FD support.

.. option:: --data-bitrate DATA_BITRATE

   Bitrate to use for the data phase in case of CAN-FD.

.. option:: --timing TIMING_ARG [TIMING_ARG ...]

   Bit rate and bit timing configuration. For classical CAN: ``--timing f_clock=8_000_000 tseg1=5 tseg2=2 sjw=2 brp=2 nof_samples=1``. For CAN FD: ``--timing f_clock=80_000_000 nom_tseg1=119 nom_tseg2=40 nom_sjw=40 nom_brp=1 data_tseg1=29 data_tseg2=10 data_sjw=10 data_brp=1``. Whether the ``timing`` argument is supported depends on the CAN interface; see the python-can documentation.

.. option:: --filter {<can_id>:<can_mask>,<can_id>~<can_mask>} [{<can_id>:<can_mask>,<can_id>~<can_mask>} ...]

   Space separated CAN filters for the given CAN interface. ``<can_id>:<can_mask>`` matches when ``<received_can_id> & mask == can_id & mask``. ``<can_id>~<can_mask>`` matches when ``<received_can_id> & mask != can_id & mask``. ID and mask are always interpreted as hexadecimal values. For example, ``--filter 100:7FC 200:7F0`` shows only frames with ID 0x100 to 0x103 and 0x200 to 0x20F.

.. option:: --bus-kwargs BUS_KWARG [BUS_KWARG ...]

   Keyword arguments passed to the instantiation of the bus class. For example, ``-i vector -c 1 --bus-kwargs app_name=MyCanApp serial=1234`` is equivalent to opening the bus with ``can.Bus('vector', channel=1, app_name='MyCanApp', serial=1234)``.

**Input and output**

Input: CAN bus traffic, received through python-can (see ``--interface``, ``--channel`` and the python-can configuration).

Output: A text based user interface in the terminal.

.. _cli-dump:

``dump``
--------

.. program:: cantools dump

Dump given database in a human readable format.

.. code-block:: text

   cantools dump [-h] [-e ENCODING] [--prune] [--no-strict] [--with-comments] [-m MESSAGE] database

**Positional arguments**

``database``
   Database file.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

.. option:: --with-comments

   Print the comments.

.. option:: -m MESSAGE, --message MESSAGE

   Print only the specified message.

**Input and output**

Input: Database file.

Output: Database contents in a human readable format on standard output.

.. _cli-list:

``list``
--------

.. program:: cantools list

Print the contents of a bus description file in an easy to process and humanly readable format. This is similar to "dump" with the output being less pretty but more complete and much easier to process by shell scripts.

.. code-block:: text

   cantools list [-h] [-n] [-x] [--skip-format-specifics] [-a] [-b] [-c] [--prune] [--no-strict] FILE [[BUSES|MESSAGES] ...]

**Positional arguments**

``FILE``
   Bus description file to read.

``BUSES|MESSAGES ...``
   The names of the buses or messages which shall be inspected.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -n, --exclude-normal

   Do not print non-extended CAN messages.

.. option:: -x, --exclude-extended

   Do not print extended CAN messages.

.. option:: --skip-format-specifics

   Do not print any information which is specific to the format used by the database file.

.. option:: -a, --all

   Print detailed infos for all messages found in the input file.

.. option:: -b, --buses

   Print information about the buses described by the input file.

.. option:: -c, --nodes

   Print information about the CAN nodes described by the input file.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

**Input and output**

Input: Database file.

Output: Requested database contents on standard output, in a format that is easy to process by shell scripts.

.. _cli-generate_c_source:

``generate_c_source``
---------------------

.. program:: cantools generate_c_source

Generate C source code from given database file.

.. code-block:: text

   cantools generate_c_source [-h] [--database-name DATABASE_NAME] [--no-floating-point-numbers] [--bit-fields] [-e ENCODING] [--prune] [--no-strict] [-f] [-o OUTPUT_DIRECTORY] [--use-float] [--use-round] [--node NODE] infile

**Positional arguments**

``infile``
   Input database file.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: --database-name DATABASE_NAME

   The database name. Uses the stem of the input file name if not specified.

.. option:: --no-floating-point-numbers

   No floating point numbers in the generated code.

.. option:: --bit-fields

   Use bit fields to minimize struct sizes.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

.. option:: -f, --generate-fuzzer

   Also generate fuzzer source code.

.. option:: -o OUTPUT_DIRECTORY, --output-directory OUTPUT_DIRECTORY

   Directory in which to write output files. Default: ``.``.

.. option:: --use-float

   Use float instead of double for floating point generation.

.. option:: --use-round

   Use rounding instead of truncation for floating point conversions.

.. option:: --node NODE

   Generate pack/unpack functions only for messages sent/received by the node.

**Input and output**

Input: Database file.

Output: ``<database-name>.h`` and ``<database-name>.c`` in the output directory (created if it does not exist). With ``--generate-fuzzer``, also ``<database-name>_fuzzer.c`` and ``<database-name>_fuzzer.mk``. The names of the written files are printed on standard output.

.. _cli-convert:

``convert``
-----------

.. program:: cantools convert

Convert given database from one format to another.

.. code-block:: text

   cantools convert [-h] [-e ENCODING] [--prune] [--no-strict] infile outfile

**Positional arguments**

``infile``
   Input database file.

``outfile``
   Output database file.

**Options**

.. option:: -h, --help

   Show the help message and exit.

.. option:: -e ENCODING, --encoding ENCODING

   File encoding.

.. option:: --prune

   Try to shorten the names of named signal choices.

.. option:: --no-strict

   Skip database consistency checks.

**Input and output**

Input: Database file.

Output: Database file. The output format is selected by the extension of ``outfile`` (``.dbc``, ``.kcd`` or ``.sym``). 