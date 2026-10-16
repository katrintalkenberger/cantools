Getting started
===============
 
This tutorial provides a step-by-step guide to installing and using ``cantools`` for the first time.
 
For practice, we use the example DBC file from the explanation section: :doc:`../explanation/can-databases`.
 
Step 1: Installation
--------------------
 
Create an empty folder named ``cantools-tutorial`` in your home directory. Save all files of this tutorial in this folder.
 
Enter the commands in a terminal. Open a terminal and change to the folder with ``cd cantools-tutorial``. On Windows, use PowerShell.
 
Install ``cantools`` from the command line with ``python3 -m pip install cantools``.
 
Check that ``cantools`` is installed correctly with ``python3 -m cantools --version``.
 
Important note: On Windows, if the error message "Python was not found" appears, run both commands with ``python`` instead of ``python3``: ``python -m pip install cantools`` (installation) and ``python -m cantools --version`` (check). Use ``python`` instead of ``python3`` in all following commands as well.
 
Step 2: DBC file
----------------
 
Use the DBC file from the :doc:`explanation <../explanation/can-databases>`. Copy the following text and save it as ``windshield.dbc`` in your tutorial folder.
 
.. code-block:: text
 
   BO_ 512 Windshield_Sensor: 8 ECU_Sensor
    SG_ Pressure : 0|16@1+ (1,0) [0|2000] "hPa"  ECU_Display
    SG_ Temperature : 16|8@1+ (1,-40) [-40|125] "degC"  ECU_Display
    SG_ Humidity : 24|8@1+ (1,0) [0|100] "%"  ECU_Display
    SG_ Sensor_Status : 32|2@1+ (1,0) [0|3] ""  ECU_Display
 
   VAL_ 512 Sensor_Status 0 "Sensor OK" 1 "Clean sensor" 2 "Sensor defective" 3 "Not available" ;
 
Step 3: Decoding
----------------
 
We continue with the explanation example of the following incoming data frame::
 
   00 00 34 00 00 00 00 00
 
To decode the data bytes there are two options: the command line or a Python function.
 
Command line: The command line tool reads frames from standard input. The following call passes one frame in the format of the Linux program ``candump``. ``vcan0`` is only the name of the interface in this format, you do not need such an interface. ``200`` is the identifier 512 in hexadecimal notation.
 
Linux and macOS:
 
.. code-block:: bash
 
   echo '  vcan0  200   [8]  00 00 34 00 00 00 00 00' | python3 -m cantools decode windshield.dbc
 
Windows:
 
.. code-block:: powershell
 
   '  vcan0  200   [8]  00 00 34 00 00 00 00 00' | python -m cantools decode windshield.dbc
 
In both cases the information in the data bytes is interpreted with the Windshield database. The output is:
 
.. code-block:: text
 
     vcan0  200   [8]  00 00 34 00 00 00 00 00 ::
   Windshield_Sensor(
       Pressure: 0 hPa,
       Temperature: 12 degC,
       Humidity: 0 %,
       Sensor_Status: Sensor OK
   )
 
Python: To run the ``cantools`` Python function directly, save the following minimal script as ``decode_example.py``:
 
.. code-block:: python
 
   import cantools
 
   db = cantools.database.load_file('windshield.dbc')
   print(db.messages)
 
   data = bytes.fromhex('0000340000000000')
   print(db.decode_message(512, data))
 
Run the script with ``python3 decode_example.py``.
 
Output:
 
.. code-block:: text
 
   [message('Windshield_Sensor', 0x200, False, 8, None)]
   {'Pressure': 0, 'Temperature': 12, 'Humidity': 0, 'Sensor_Status': 'Sensor OK'}
 
Step 4: Encoding
----------------
 
Encoding is done with a Python function. Save the following example as ``encode_example.py``:
 
.. code-block:: python
 
   import cantools
 
   db = cantools.database.load_file('windshield.dbc')
 
   data = db.encode_message(512, {'Pressure': 0, 'Temperature': 12,
                               'Humidity': 0, 'Sensor_Status': 'Sensor OK'})
   print(data.hex())
 
Run the script with ``python3 encode_example.py``.
 
Output:
 
.. code-block:: text
 
   0000340000000000
 
This guide is an introduction to the topics of ``cantools``. For more information, see the :doc:`how-to guides <../how-to/index>` (scripting, vcan), the :doc:`explanation <../explanation/index>` (architecture) and the :doc:`CLI reference <../reference/cli/commands>`.