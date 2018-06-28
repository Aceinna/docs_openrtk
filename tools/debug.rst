Debugging
=========

.. contents:: Contents
    :local:

There are two primary methods to debug a program on OpenIMU.  The first is to use the debug serial port which by default output 
user defined ASCII messages over the debug serial connection at 38.4Kbaud.

.. code-block:: c 

    // Custom print like syntax outputs ASCII data on debug serial port
    fprint("%d", myvar);

Alternatively use the debug window in Visual Studio code along with the ST-LINK JTAG pod. The following screenshots show where to find the
JTAG download button and where to find the full debug screen.

.. note::

    Using JTAG to download code to the OpenIMU is the fastest method to download code and generally requires just a few seconds.  This feature is detailed above.
    However, using more advanced JTAG debugging features requires a subscription to Platform IO's Enterprise addition.  More information is available here.

.. note::

    The documentation and tutorials on this site assume use of the ST-LINK JTAG pod.  The JTAG pod is shipped with every OpenIMU developer's kit.



