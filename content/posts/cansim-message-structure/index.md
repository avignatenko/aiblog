---
title: "CANSim message format"
draft: false
date: 2024-08-22
description: ""
summary: "The specs for CAN messages used in CANSim"
tags: ["CANSim", "DYI"]
---

On hardware side of CANSim, the [communication is handled by RJ45 sockets and plugs]({{< ref "/can-rj45-pinout" >}}). Its time to discuss the message exchange protocol that I use.

I use *1Mbps CAN* with *extended IDs* to handle communication between gauges in the cockpit.

Every message is identified by it's ID (29 bits) and payload (8 bytes). 

### CAMSim message format

CAN id (high bit -> low bit, total 29 bits, only extended format supported, data is little-endian):

* 4 bits: (25 .. 28) priority (0 .. 15, less is more priority)
* 5 bits: (20 .. 24) port
* 10 bits (10 .. 19): dst address (0 .. 1023)
* 10 bits (0 .. 9): src address (0 .. 1023)
* CAN payload (0 - 8 bytes)


Every instrument has local address in CAN network and also one or more "ports" which work as input/output data channels.

Each address has max 10 bits allowing to maximum 1024 devices in the network.

Address reservation:
* 0 - unknown
* 1 - 15 master (connected to simulation PC)
* 16 - 1023 client devices

Payload is full defined by the device and port.
Every devices has to specify how many ports it has and data input/output for each port.

### Example

So let's assume we have an indicated airspeed gauge. It has the following properties labeled somewhere on the enclose: address 25, 1 input-only port, accepts 4 byte IEEE floating point value. We're going to send a speed update to 45 kn from a sim master with address 1.

The message will look like this:

* priority 0 (default)
* port 0
* dst address 25
* src address 1
* payload 4 bytes, 45 


| Data         | Value             |
|--------------|-------------------|
|msg           | (0 << 25) + (0 << 20) + (25 << 10) + (1 << 0) = 25601 (0x6401)|
|payload       | 45 = 0x42340000|
|extended_bit  | 1|