Python Interface
================

The OpenIMU Python driver supports communication with the hardware for datalogging and 
device configuration over the main user UART interface of the OpenIMU hardware.  When
run in server mode, it allows connection of the OpenIMU with the developer's website
Aceinna Navigation Studio and its friendly GUI interface.

The Python driver attempts to automatically find a connected OpenIMU hardware by scanning available ports
at various baurd rates.  Once a connection is established, this connection is recorded in a file named
*connection.json*.  On the next use of the driver, the driver will first attempt communication on this port
speeding up connection time.

The Python driver reads a JSON file by default named *openimu.json* to understand 
the messages - both primary output packets, as well as command/response type packets from the IMU. 
These can be customized by changing the JSON file and the Python script will use that information
to parse data (literally the byte stream) from the OpenIMU in real-time appropriately.  

Here are a few samples function you can call with the driver.

.. code:: python

    # Create a device and connect to it
    imu = OpenIMU()
    imu.find_device()

    # Get all parameters by issuing 'gA' command
    imu.openimu_get_all_param()

    # Update a parameter by issuing 'uP' command
    # See openimu.json for the parameter numbers
    # This example changes output packet rate to 100Hz
    imu.openimu_update_param(4,100)

    # Save parameter changes by issuing 'sC' command
    imu.openimu_save_config()

    # Log data for 1Hr
    # Data is loggged into data directory with time of day string as default filename
    imu.start_log()
    time.sleep(3600)
    imu.stop_log()

    # Update units firmware
    # bin file is stored in .pioenvs directory and created after compilation
    # the file most be moved to where the Python driver can find it
    imu.openimu_upgrade_fw('myapp.bin')


.. note::

    As you develop code and customize your OpenIMU, you should also update *openimu.json* to keep it in sync with your changes.  This 
    way both the Python driver and developers website, ANS, will function properly and understand your units special
    programmed characteritics.



.. contents:: Contents
    :local:

