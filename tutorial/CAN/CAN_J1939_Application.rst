CAN J1939 Example Application
*******************************

.. contents:: Contents
    :local:


*   The CAN J1939 Example Application provides the framework for custom J1939-based applications for
    the OpenIMU300RI unit.
*   The example can be used as is or customized to suit the customer's system requirements.
*   The SAE J1939 standards document set specifies the requirements for systems based on J1939 messaging.
    The SAE site provides a full list of the J1939 standard document set
    (`SAE J1939 Standards Collection <https://www.sae.org/standardsdev/groundvehicle/j1939a.htm>`_)
    In particular:

    *   Section 3 of the SAE J1939 standards document provides the high-level technical requirements
        for systems that use J1939 messaging.
    *   Section 5 of the SAE J1939-21 standards document provides the technical requirements
        for J1939 data link layer for all SAE J1939 applications.
    *   The license for using an SAE standards document do not allow distribution of the documents.
        SAE J1939 documents can be purchased online at the IHS Standards Store
        (`IHS Market - J1939 <"https://global.ihs.com/search_res.cfm?&rid=Z56&mid=SAE&input_doc_number=J1939&input_doc_title=&sort=RELEVANCE">`_)
    *   There are many J1939 related documents available that can be freely distributed.  We provide two such documents here:

        *   Vector Informatik GmbH provides a document which is a good introduction to J1939.

            *   Download here - :download:`download <../../media/Vector AN-ION-1-3100_Introduction_to_J1939.pdf>`.
        *   Kvaser provides a J1939 Overview document.

            *   Download here - :download:`download <../../media/Kvaser-J1939-Overview.pdf>`.


The following pages describe the CAN J1939 Example Application Details:

*   VSCode project for the J1939 CAN Example Application

*   Example Application User Code Walkthrough

*   Diagrams for the Example Application

    *   Data Acquisition Task Dataflow
    *   CAN Communication Task State Transition Diagram
    *   Task Synchronization Diagram

*   Descriptions of the example CAN messages implemented in the application.

.. toctree::
    :maxdepth: 1
    :hidden:

    CAN_J1939_ExampleApplication_VSCodeProject
    CAN_J1939_ExampleApplication_UserCodeWalkthrough
    CAN_J1939_ExampleApplicationDiagrams
    CAN_J1939_CAN_Messages
