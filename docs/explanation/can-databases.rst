CAN databases
=============

Raw frames and their interpretation
-----------------------------------

A CAN bus is a system for transmitting data between electronic control units. The information is transmitted bit by bit as a stream of zeros and ones. The CAN controller assembles data frames from this bit stream. A data frame carries an identifier, a length and data bytes, but it does not say what the bytes mean. Which bits belong to which signal, how a raw value converts to a physical value and which unit it has is defined outside the bus, in a CAN database.

The raw frames are the same regardless of how they are described. The database formats are different dictionaries for interpreting the same raw frames.

Database formats
----------------

A widely used format is DBC, a database in text form. A DBC file describes:

- messages with identifier and length
- signals with start bit, bit length, byte order, factor, offset, unit and value tables
- nodes, the ECUs that send and receive the messages
- attributes

Other formats describe the same kind of information. KCD is based on XML. SYM is a text format defined by PEAK. ``cantools`` also reads ARXML and CDD files, see :doc:`architecture-overview`.

A DBC message
-------------

A DBC message always has an identifier and a length. It usually has at least one signal, together with the properties of each signal.

The following excerpt describes the message ``Windshield_Sensor`` with identifier 512 and a length of 8 bytes. Its signals are ``Pressure``, ``Temperature``, ``Humidity`` and ``Sensor_Status``. The excerpt omits the header sections of a DBC file, such as the version and the list of nodes.

.. code-block:: text

   BO_ 512 Windshield_Sensor: 8 ECU_Sensor
    SG_ Pressure : 0|16@1+ (1,0) [0|2000] "hPa"  ECU_Display
    SG_ Temperature : 16|8@1+ (1,-40) [-40|125] "degC"  ECU_Display
    SG_ Humidity : 24|8@1+ (1,0) [0|100] "%"  ECU_Display
    SG_ Sensor_Status : 32|2@1+ (1,0) [0|3] ""  ECU_Display

   VAL_ 512 Sensor_Status 0 "Sensor OK" 1 "Clean sensor" 2 "Sensor defective" 3 "Not available" ;

The ``BO_`` line defines the message: identifier, name, length in bytes and sending node. Each ``SG_`` line defines one signal. The line of the signal ``Temperature`` reads as follows:

.. list-table::
   :header-rows: 1
   :widths: 25 75

   * - Part
     - Meaning
   * - ``Temperature``
     - Name of the signal
   * - ``16|8``
     - Start bit 16, length 8 bits
   * - ``@1``
     - Byte order: 1 is little endian (Intel), 0 is big endian (Motorola)
   * - ``+``
     - Unsigned (``-`` would be signed)
   * - ``(1,-40)``
     - Factor 1 and offset -40
   * - ``[-40|125]``
     - Minimum and maximum of the physical value
   * - ``"degC"``
     - Unit
   * - ``ECU_Display``
     - Receiving node

The value table of ``Sensor_Status`` is not part of the signal line. It is defined separately in the ``VAL_`` line, which assigns a text to each raw value.

Bits are numbered from 0. For a little endian signal, the start bit is the least significant bit of the signal. Start bit 16 is therefore the first bit of byte 2, which is the third byte of the frame (bytes are also counted from 0).

Decoding a frame
----------------

A frame with identifier 512 arrives on the bus with the following data bytes::

   00 00 34 00 00 00 00 00

The database defines where each signal is located in these bytes and how its raw value converts to the physical value:

.. code-block:: text

   physical value = raw value * factor + offset

.. list-table::
   :header-rows: 1
   :widths: 20 30 20 30

   * - Signal
     - Position
     - Raw value
     - Physical value
   * - ``Pressure``
     - Bits 0 to 15 (bytes 0 and 1)
     - 0
     - 0 hPa
   * - ``Temperature``
     - Bits 16 to 23 (byte 2)
     - 0x34 = 52
     - 52 * 1 + (-40) = 12 degC
   * - ``Humidity``
     - Bits 24 to 31 (byte 3)
     - 0
     - 0 %
   * - ``Sensor_Status``
     - Bits 32 and 33 (lowest two bits of byte 4)
     - 0
     - "Sensor OK" (value table)

For ``Temperature``, the byte ``0x34`` is ``00110100`` in binary and 52 in decimal. Factor 1 and offset -40 give 12 degC.

Encoding is the same conversion in the opposite direction: from physical values to raw values, and from raw values to the bits in the data bytes.
