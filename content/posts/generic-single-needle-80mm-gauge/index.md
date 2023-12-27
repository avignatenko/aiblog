---
title: "Generic Single Needle 3 1/8 inch Gauge"
draft: false
date: 2023-10-01
description: "This 3 1/8 inch gauge has a single arrow with about 300 degree smooth movement range.
             Typical application is airspeed (ASI) or vertical speed (VSI) indicators."
summary: "Description of DYI 3 1/8 inch gauge for flight simulation"
categories: ["CANSim", "DYI"]
ShowToc: true
TocOpen: true
cover:
    image: "gsng-overview.jpg"
---

The gauge is designed as a series of parallel 3D printed plastic “decks”, separated by standard PCB standoffs. It's compatible with a typical 3 1/8 inch panel cutouts.

Controller and interface board is the last deck. Like every gauge in CANSim, It’s based on Arduino Nano and provides [RJ45-based CAN interface]({{< ref "/can-rj45-pinout" >}}). All gauges communicate with a master controller which in turn has USB connection to a host PC with a simulator (or to a networked PC).

## Parts List


{{< figure
  src="gsng-explode.png"
  caption="“" 
  caption-position="center">}}


| N  | Component                  | Quantity | Notes       |
|----|----------------------------|----------|-------------|
| 1  | Front Bezel Glass          | 1        |             |
| 2  | Front Bezel                | 1        |             |
| 3  | Arrow                      | 1        |             |
| 4  | Face                       | 1        |             |
| 5  | Face deck                  | 1        |             |
| 6  | Motor deck                 | 1        |             |
| 7  | Motor x27.168 (or similar) | 1        |             |
| 8  | M3 Stand-off               | 8        |             |
| 9  | M3 Screw                   | 4        |             |
| 10 | M3 Nut                     | 4        |             |
| 11 | M3 Heat Set Inserts        | 4        |             |
| 12 | M2 Screw 20mm              | 2        |             |
| 13 | Controller/IO PCB          | 1        | Custom made |


## Assembly

* Insert glass (1) into front bezel (2). Optionally glue (normally not required for home use). Front bezel is ready.
* Glue the face paper (4) into its deck using PVA or similar glue.
* Mount the motor (7) into its deck (6) using two M2x20 screws.
* Stack the face deck and the motor desk using 4 standoffs (8). Attach the arrow (3) to the motor.
* Place the front bezel on the top of the assembly and fix with four M3 screws (9).
* Attach PCB and fix with M3 nuts (10).
* Mount on the panel using cutouts.

## Front Bezel

A front bezel design is shared by most of CANSim instruments. It contains the plate itself and a glass.
A bezel is typically 3D printed from ABS.

[front-bezel-v42.stl](front-bezel-v42.stl)

A circular glass (74mm diameter) can be cut from 2mm transparent acrylic plate. 1.5mm or 3mm will work as well

[front-bezel-glass-v7.stl](front-bezel-glass-v7.stl)

## Faceplate

## Motor deck

## Assembly tips

## Electronics and PCB

Schematics:

{{< figure
  src="gsng-schematic.svg"
  caption="“" 
  caption-position="center">}}

The board is done with single-side PCB design, for easier home manufacturing.

{{< figure
  src="gsng-top.svg"
  caption="Top (component) side" 
  caption-position="center">}}

  {{< figure
  src="gsng-bottom.svg"
  caption="Bottom (solder) side " 
  caption-position="center">}}

## Firmware and I/O

Generic gauge is receive-only instrument, it can receive a single float value as a gauge position.

| Port | Data Type | Description           |
|------|-----------|-----------------------|
| 0    | FLOAT     | Arrow Position 0..MAX |

Firmware source code: https://github.com/avignatenko/SimGauges/tree/master/src/GenericSingleNeedle