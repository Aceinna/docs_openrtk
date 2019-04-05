DBC Format and Supported Keywords Specification
==================================================

*   This document was created on January 4, 2019, so it does not necessarily specify the complete Vector DBC standard.
    (`Link to Vector CANdb Specification <http://vector.com/vi_candb_en.html>`__)
*   However, it is complete up to that date and provides a detailed description for the DBC Format
    Description and Supported Keywords

DBC Format Description
======================

| ``<...>``: Required field
| ``[...]``: Optional field
| ``|``: Or (eg. <A|B>)

Supported Keywords
==================

VERSION
-------

| Version identifier of the DBC file.
| Format: ``VERSION "<VersionIdentifier>"``

BS\_
----

| Bus configuration.
| Format:: ``BS_: <Speed>``
| Speed in kBit/s

BU\_
----

List of all CAN-Nodes, seperated by whitespaces.

BO\_
----

| Message definition.
| Format: ``BO_ <CAN-ID> <MessageName>: <MessageLength> <SendingNode>``
| MessageLength in bytes.

SG\_
----

| Signal definition.
| Format:
  ``SG_ <SignalName> [M|m<MultiplexerIdentifier>] : <StartBit>|<Length>@<Endianness><Signed> (<Factor>,<Offset>) [<Min>|<Max>] "[Unit]" [ReceivingNodes]``
| Length in bits.
| Signed: + = unsigned; - = signed
| Endianness: 1 = little-endian, Intel; 0 = big-endian, Motorola
| M: If M than this signals contains a multiplexer identifier.
| MultiplexerIdentifier: Signal definition is only used if the value of
  the multiplexer signal equals to this value.

CM\_
----

| Description field.
| Format:
  ``CM_ [<BU_|BO_|SG_> [CAN-ID] [SignalName]] "<DescriptionText>";``

BA_DEF\_
--------

| Attribute definition.
| Format:
  ``BA_DEF_ [BU_|BO_|SG_] "<AttributeName>" <DataType> [Config];``

======== ============== ============================
DataType Description    Config format
======== ============== ============================
INT      integer        ``<min> <max>``
FLOAT    floating point ``<min> <max>``
STRING   string
ENUM     enumeration    ``"<Value0>","<Value1>"...``
======== ============== ============================

BA_DEF_DEF\_
------------

| Attribute default value
| Format: ``BA_DEF_DEF_ "<AttributeName>" ["]<DefaultValue>["];``

BA\_
----

| Attribute
| Format:
  ``BA_ "<AttributeName>" [BU_|BO_|SG_] [Node|CAN-ID] [SignalName] <AttributeValue>;``

VAL\_
-----

| Value definitions for signals.
| Format:
  ``VAL_ <CAN-ID> <SignalsName> <ValTableName|ValTableDefinition>;``

VAL_TABLE\_
-----------

| Value table definition for signals.
| Format: ``VAL_TABLE_ <ValueTableName> <ValueTableDefinition>;``
| ValueTableDefinition: List of ``IntValue "StringValue"`` Pairs,
  seperated by whitespaces

Attributes
==========

In this sections are standard attributes used by
`CANpy <https://github.com/stefanhoelzl/CANpy>`__ defined. The
attributes can be overwritten within a DBC file. ## Message Attributes

GenMsgSendType
--------------

| Defines the send type of a message.
| Supported types:
| \* cyclic
| \* triggered
| \* cyclicIfActive
| \* cyclicAndTriggered
| \* cyclicIfActiveAndTriggered
| \* none
| Definition:
  ``BA_DEF BO_ "GenMsgSendType" ENUM "cyclic","triggered","cyclicIfActive","cyclicAndTriggered","cyclicIfActiveAndTriggered","none"``
| Default: none Definition: ``BA_DEF_DEF "GenMsgSendType" "none"``

GenMsgCycleTime
---------------

| Defines the cycle time of a message in ms.
| Definition: ``BA_DEF BO_ "GenMsgCycleTime" INT 0 0``
| Default: 0
| Definition: ``BA_DEF_DEF "GenMsgCycleTime" 0``

GenMsgStartDelayTime
--------------------

| Defines the allowed delay after startup this message must occure the
  first time in ms.
| Definition: ``BA_DEF BO_ "GenMsgStartDelayTime" INT 0 0``
| Default: 0 (=GenMsgCycleTime)
| Definition: ``BA_DEF_DEF "GenMsgStartDelayTime" 0``

GenMsgDelayTime
---------------

| Defines the allowed delay for a message in ms.
| Definition: ``BA_DEF BO_ "GenMsgDelayTime" INT 0 0``
| Default: 0
| Definition: ``BA_DEF_DEF "GenMsgDelayTime" 0``

Signal Attributes
-----------------

GenSigStartValue
~~~~~~~~~~~~~~~~

| Defines the value as long as no value is set/received for this signal.
| Definition: ``BA_DEF SG_ "GenSigStartValue" INT 0 0``
| Default: 0
| Definition: ``BA_DEF_DEF "GenSigStartValue" 0``
