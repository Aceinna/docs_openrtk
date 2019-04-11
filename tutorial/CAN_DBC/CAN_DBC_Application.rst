*****************************
CAN DBC Example Application
*****************************

.. contents:: Contents
    :local:


*   The CAN DBC Example Application provides the framework for custom DBC-based
    applications for the OpenIMU300RI unit.
*   The example can be used as is or customized to suit the customer's system requirements.
*   The DBC Specification specifies sets of parsing rules for creating and interpreting messages. The DBC specification was developed by Vector
    (`Link to Vector Informatik GmbH <http://www.vector.com>`_), which retains control of the specification.
*   Vector has published the "DBC File Format Documentation", which is available for download here:
    :download:`download link <../../media/Vector_DBC_File_Format_Documentation.pdf>`.  (**Use your browser back button to return here**)
*   The DBC example application builds and sends messages that a DBC database manager
    can read and interpret.

.. note::

    The DBC Example Application is less mature than the J1939 Example Application,
    as the DBC Example Application is being ported from a proprietary DBC messaging
    application to an OpenIMU open source application.


The following pages describe the CAN DBC Example Application Details:

*   Example VSCode project directory structure and files for the DBC CAN Example Application
*   Example Application User Code Walkthrough
*   Task Synchronization Diagram for the Example Application
*   Descriptions of the example CAN messages implemented in the application.

.. toctree::
    :maxdepth: 1
    :hidden:

    CAN_DBC_ExampleApplication_VSCodeProject
    CAN_DBC_ExampleApplication_UserCodeWalkthrough
    CAN_DBC_ExampleApplicationDiagrams
    CAN_DBC_CAN_Messages
