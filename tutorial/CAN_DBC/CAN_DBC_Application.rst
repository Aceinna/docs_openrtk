*****************************
CAN DBC Example Application
*****************************

.. contents:: Contents
    :local:


*   The CAN DBC Example Application provides the framework for custom DBC-based
    applications for the OpenIMU300RI unit.
*   The example can be used as is or customized to suit the customer's system requirements.
*   DBC is a CAN Database specification developed by Vector
    (`Link to Vector Informatik GmbH <http://www.vector.com>`_)
*   The DBC example application builds and sends messages that a DBC database manager
    can read and interpret.

.. note::

    The DBC Example Application is less mature than the J1939 Example Application,
    as the DBC Example Application is being ported from a proprietary DBC messaging
    application to an OpenIMU open source application.


The following pages describe the CAN DBC Example Application Details:

*   The DBC Database File Format, Supported Keywords, and Supported Attributes
*   Example VSCode project directory structure and files for the DBC CAN Example Application
*   Example Application User Code Walkthrough
*   Task Synchronization Diagram for the Example Application
*   Descriptions of the example CAN messages implemented in the application.

.. toctree::
    :maxdepth: 1
    :hidden:

    DBC_Specification
    CAN_DBC_ExampleApplication_VSCodeProject
    CAN_DBC_ExampleApplication_UserCodeWalkthrough
    CAN_DBC_ExampleApplicationDiagrams
    CAN_DBC_CAN_Messages
