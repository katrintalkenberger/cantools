Scripting
=========

The example starts by parsing a `small DBC-file`_ and printing its
messages and signals.

.. code-block:: python

   >>> import cantools
   >>> from pprint import pprint
   >>> db = cantools.database.load_file('tests/files/dbc/motohawk.dbc')
   >>> db.messages
   [message('ExampleMessage', 0x1f0, False, 8, 'Example message used as template in MotoHawk models.')]
   >>> example_message = db.get_message_by_name('ExampleMessage')
   >>> pprint(example_message.signals)
   [signal('Enable', 7, 1, 'big_endian', False, 1.0, 0, 0.0, 0.0, '-', False, None, {0: 'Disabled', 1: 'Enabled'}, None),
    signal('AverageRadius', 6, 6, 'big_endian', False, 0.1, 0, 0.0, 5.0, 'm', False, None, None, ''),
    signal('Temperature', 0, 12, 'big_endian', True, 0.01, 250, 229.53, 270.47, 'degK', False, None, None, None)]

The example continues `encoding`_ a message and sending it on a CAN
bus using the `python-can`_ package.

.. code-block:: python

   >>> import can
   >>> can_bus = can.interface.Bus('vcan0', bustype='socketcan')
   >>> data = example_message.encode({'Temperature': 250.1, 'AverageRadius': 3.2, 'Enable': 1})
   >>> message = can.Message(arbitration_id=example_message.frame_id, is_extended_id=example_message.is_extended_frame, data=data)
   >>> can_bus.send(message)

Alternatively, a message can be encoded using the `encode_message()`_
method on the database object.

The last part of the example receives and `decodes`_ a CAN message.

.. code-block:: python

   >>> message = can_bus.recv()
   >>> db.decode_message(message.arbitration_id, message.data)
   {'AverageRadius': 3.2, 'Enable': 'Enabled', 'Temperature': 250.09}

See `examples`_ for additional examples.

.. _motohawk.dbc: https://github.com/cantools/cantools/blob/master/tests/files/dbc/motohawk.dbc

.. _python-can: https://python-can.readthedocs.io/en/master/

.. _encode_message(): http://cantools.readthedocs.io/en/latest/#cantools.database.can.Database.encode_message

.. _decodes: http://cantools.readthedocs.io/en/latest/#cantools.database.can.Database.decode_message

.. _examples: https://github.com/cantools/cantools/blob/master/examples

.. _encoding: http://cantools.readthedocs.io/en/latest/#cantools.database.can.Message.encode

.. _small DBC-file: https://github.com/cantools/cantools/blob/master/tests/files/dbc/motohawk.dbc

