---
title: "CAN RJ45 pinout"
draft: false
date: 2023-10-07
description: ""
summary: "The description of CAN bus physical interface I use for CANSim"
tags: ["CANSim", "DYI"]
---

I use RJ45 for CAN communication in CAMSim.  It has many advantages over classic DSUB. I don’t think there is a better cable for CAN. The 100Ω differential resistance of RJ45 is close enough to the 120Ω required by CAN, RJ45 is cheap and easy to handle.

Both signal and power is delivered with RJ45. In theory the power should not be delivered in the same cable, but in practice I never had any problem with it and it’s much more convenient setup. It should handle up to 1.5A power total which is enough for 10-12 instruments.

I use 12V across CANSim for power delivery.

Each instrument has two RJ45 sockets, so daisy chaining is extremely easy, with few cables needed.

{{< figure
  src="rj45pinout.png"
  caption="“" 
  caption-position="center">}}


| N         | Purpose           |
|-----------|-------------------|
| 1         | CAN_H bus line    |
| 2         | CAN_L bus line    |
| 3         | GND / 0V          |
| 4         | GND (optional)    |
| 5         | V+ 12V (optional) |
| 6         | GND (optional)    |
| 7         | GND (optional)    |
| 8         | V+ 12V            |