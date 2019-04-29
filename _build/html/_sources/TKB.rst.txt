.. _TKB:

=========
BSPD 2019
=========

:Author: arsphotographika
:Date:   April 2019

|image|

**MIL0012-RA** **Thruster/Kill System**

Electrical Team Members
=======================

| Firmware Des. By: Marquez Jones
| PCB Des. By: Frank Mitchell

|image|

Description
===========

The Thruster/Kill System describes the PCB and accompanying Firmware to
control and monitor Blue Robotics Thrusters and Kill network on
Subjugator 8. The hardware for this system is designed to interface with
the vehicle’s CAN communication system, 8 Blue robotics ESCs via PWM,
and control power relays to both main vessel and its thrusters. A note
about this, is the main vessel relay connection was repurposed in
firmware to accommodate another relay as opposed to being an optional
UART connection as designated in hardware. The CAN handling on this
board is designed to filter for motherboard commands on the thruster
channel and read in all kill messages sent on the kill channel. The
board will also output the status of a hall effect sensor corresponding
to GO periodically(explained later). The board also has two other hall
effect connections which correspond to Hard Kill(On/Off) and Soft
Kill(Enable). If a Hard Kill is asserted, power to main will be cut. If
a Soft Kill is asserted via either CAN or the Hall effect, power to
thrusters will be cut. Other possible sources of kill are documented in
this manual.

Firmware FSM
============

Main Program
------------

|image|

Interrupt Service Routines
--------------------------

|image|

Kill System
===========

Overview
--------

THe Kill System describes sub’s safety features that prevents that
disables the system thus disabling its ability to act erratically.
There’s multiple reasons as to why we may want to kill sub and there are
several ways to accomplish killing sub. Sub has two external ways to
kill via hall effect magnets that should be mounted to the bulkhead.
There’s also the ability to soft kill sub via a CAN message. Two passive
kill features also exist,one being losing communication with motherboard
and the other is a softer idle where it’ll tell thrusters to stop if
motherboard ceases sending thruster commands. Refer to the table below
to see kill sources and their effects.

Kill Sources and Effects
------------------------

================= ==================== ================ =============
Sources           Kills Thruster Power Kills Main Power Locks Program
================= ==================== ================ =============
Hall Hard Kill    Yes                  Yes              Yes
Hall Soft Kill    Yes                  No               Yes
CAN Soft Kill     Yes                  No               Yes
Heart Beat Missed Yes                  No               No
Thruster Idle     No                   No               No
================= ==================== ================ =============

Passive Kill Features
---------------------

This section will more in depth describe passive or kills that aren’t
caused by outside request or action. These are kills the board will
enact if the board believes motherboard is inactive. The two passive
kill features are the Heart Beat Kill and Thruster Idle. These are
timing sensitive events. Idles will occur every 500 ms. Heart Beat Kills
will occur every 1000 ms.

Passive Kill Table
------------------

================= ====== ===========================
Sources           Period To Unkill
================= ====== ===========================
Heart Beat Missed 1000ms Transmit Heart Beat Message
Thruster Idle     500ms  Transmit Thruster Command
================= ====== ===========================

Thruster System
===============

== ========
ID Thruster
== ========
0  FHL
1  FHR
2  FVL
3  FVR
4  BHL
5  BHR
6  BVL
7  BVR
== ========

-  | The firmware on this board used the TM4C123G PWM module to
     communicate with the Blue Robotics Basic ESCs as opposed to
     providing PWM commands to the thrusters themselves. In this, I used
     the word communicate. This is because the ESCs accept a protocol
     similar to One Shot which effectively uses PWM to send commands.
     This is abstracted in CAN interface as the board will receive a
     thrust between 1 and -1 then convert that into the correct command
     to the ESCs. In this, it sends a PWM signal with a 2000us period.
     This is important to know if you wish to modify the firmware or if
     we want to use the board to control thrusters that aren’t blue
     robotics.
   | For this board, each thruster is assigned an ID 0 to 7.This is how
     commands are distributed. This will make more sense when looking at
     the thruster command packet. The thruster mapping is posted here.
     Note this is based on the connections diagram.

Communications
==============

-  | In this version of the firmware, the primary means to communicate
     with the board is via control area network(CAN). This board also
     falls in line with the MIL CAN protocol of using task
     groups/channels. The firmware is set up to receive messages from
     both the thruster and kill channels. Check the ID assignment
     spreadsheets for the specifics on what ECUs and ECU IDs these refer
     to. In regards to ID filtering, this board will receive any kill
     messages on the kill channel. In this, if a Kill command is sent on
     the channe, it will kill. This board also filters for motherboard
     specific messages on the Thruster channel.

-  | This board will transmit two different messages. It will
     periodically transmit the status of the Go hall effect sensor. The
     only other case it should be transmitting is if the board is either
     soft or hard killed

==== ===== ======================
Hex  Ascii Meaning
==== ===== ======================
0x54 ’T’   Thruster Header Byte
0x4B ’K’   Kill Header Byte
0x47 ’G’   Go Header Byte
0x48 ’H’   Heart Beat Header Byte
0x43 ’C’   Command
0x52 ’R’   Response
0x41 ’A’   Assert
0x55 ’U’   Unassert
0x48 ’H’   Hard Kill
0x53 ’S’   Soft Kill
0x4D ’M’   Mother Board
==== ===== ======================

Kill Command/Response Message(RX/TX)
------------------------------------

| Channel = Kill Channel
| Will sending a ’C’(command) will tell the board to instantiate a kill
  by which the board will
| respond with ’R’ to verify its state.

|image|

Thruster Command Message(RX Only)
---------------------------------

| Channel = Thruster Channel
| Is used to transmit thruster commands. Float value should be between
  -1 and 1. 1 being full forward and -1 being full reverse of that
  thruster.

|image|

Heart Beat Message(RX Only
--------------------------

| Channel = Thruster Channel
| Must be sent by motherboard every 1000 ms or a kill state will occur.

|image|

Go Check Message(TX Only)
-------------------------

| Channel = Thruster Channel
| Will be trasmitted every 500 ms. Checks the state of the go hall
  effect. If the hall effect is
| attached, ’A’ for asserted will be transmitted. [0pt][0pt]

Connections
===========

| NOTE: The designated UART connector was implemented
| as an additional relay control pin in the firmware. This is
| designed to be a relay to control main power in case of Hard
| Kill. The provided hardware does not have a mosfet to accommodate that
| so an external one will have to attached. Refer to hardware schematic
  to
| determine required external circuit.

|image|

.. |image| image:: MIL_LOGO.png
.. |image| image:: TKB_Board.PNG
   :width: 70.0%
.. |image| image:: MAIN_FSM_R1.PNG
   :width: 100.0%
.. |image| image:: ISRS_FSM_R1.PNG
   :width: 100.0%
.. |image| image:: Kill_Msg.PNG
   :width: 100.0%
.. |image| image:: Thruster_Msg.PNG
   :width: 100.0%
.. |image| image:: HeartBeat_Msg.PNG
   :width: 70.0%
.. |image| image:: KillBoard_connectors.png
   :width: 100.0%
