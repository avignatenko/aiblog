---
title: "Z-Wave water valve controller"
draft: false
date: 2023-12-28
description: ""
description: "This small project is about making home water resources a part of home
              automation with z-wave network. As usual I'm using Z-Uno modules for interfacing with the
              network"
summary: "A DYI Z-Wave controller for GydroLock water valve"
tags: ["Smart Home", "DYI"]
ShowToc: true
TocOpen: true
---

## Approach

I already had Gydrolock motorized valves installed and working through a proprtietary controller unit. With my board the intention is to bring the following under home automation:
* open/close of water valves, both cold and hot
* emergency water leak alarm (via sensor installed with gydrolock)
* water meter pulse counter 
* air temperature and humidity inside the water block

## Hardware design

The controller is powered by 12v coming from Gydrolock unit. I wasn't sure about power source quality so preferred to add LM7085 to power Z-Uno with 
stable 5V (though it can consume 12v directly). Ground is shared between my controller and the Gydrolock unit.

Gydrolock unit exports two interfaces:

* Alarm signal (low/high), connected to Z-Wave digital input port.
* Water valves switch, exported as dry contact. We control it with 5v output port with a regular transistor scheme.

For temperature and humidity I used a cheap DHT22 sensor, which needs one digital pin to work.

Water pulse counters implemented as with RJ12 sockets, but in fact only need two contacts (input and ground) to work. We'll pull-up inputs with internal Z-Uno registers, so that each pulse will be recorded as either 0V or 3V level.

{{< figure 
   src="Schematic_Z-Wave Water Controller_2024-01-03.svg" 
   align=center 
   caption="Water controller Schematic" 
   caption-position="center">}}

Implementation is based on a two-side PCB:

{{< figure
  src="pcb-top.png"
  width=70%
  align=center
  caption="Top side (component)"
  caption-position="center">}}

{{< figure
  src="pcb-bottom.png"
  width=70%
  align=center
  caption="Bottom side (solder)"
  caption-position="center">}}

 I only used through-hole contacts since they are simpler for me to solder (YMMW).

 [Gerber files](gerber.zip).

## Controller firmware

[Firmware](https://github.com/avignatenko/zwavewatercontroller/blob/main/watercontroller.ino) is made for Z-Uno 2, 

It defines four Z-Wave channels, self-explained in the code:

```ZUNO_SETUP_CHANNELS(
  ZUNO_SWITCH_BINARY(getWaterStopSwitch, setWaterStopSwitch),
  ZUNO_SENSOR_MULTILEVEL_TEMPERATURE(s_temperature),
  ZUNO_SENSOR_MULTILEVEL_HUMIDITY(s_humidity),
  ZUNO_SENSOR_BINARY_WATER(s_water));
```

## Lessons learned

* None

## References

* [Gerber files](gerber.zip)
* [Code](https://github.com/avignatenko/zwavewatercontroller/blob/main/watercontroller.ino)

## Photos

