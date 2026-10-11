Virtual CAN interface setup
===========================

Virtual CAN interface setup in Ubuntu:

.. code:: text

   sudo modprobe vcan
   sudo ip link add dev vcan0 type vcan
   sudo ip link set vcan0 mtu 72         # For CAN-FD
   sudo ip link set up vcan0
