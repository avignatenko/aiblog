---
title: "PCB milling with GRBL CNC"
draft: false
date: 2024-09-07
description: "A quick guide to mill one-sided PCBs on a cheap GBRL CNC"
summary: "A quick guide to mill one-sided PCBs on a cheap GBRL CNC"
tags: ["CANSim", "DYI"]
---

## Overview

PCB milling is an alternative to etching for those who have a home CNC machine and 
dont like using chemicals.

It is the milling process used for removing areas of copper from a sheet of printed circuit board (PCB) material to recreate the pads, signal traces and structures according to patterns from a Gerber layout export.

Very cheap home CNCs  (like CNC 3018PRO etc) are pretty much capable of doing CNC milling.

## PCB Milling process

My approach to PCB milling that I use for CANSim making:

* I use custom-made GRBL-based CNC

* For milling I use 60 deg, 0.2mm tip V-carve. It makes wide cuts, but works for my tasks.
  For finer cuts, consider 30 deg V-carve. https://hobbycnc.com/tool-width-calculator/ can
  be used to calculate cut width.
   
* To drill holes I use "milldrill" process with 0.7mm corn mill, to avoid any tool switches
  for drilles. It doesn't make sense to make holes less than 0.7mm anyway for manual soldering

* PCB is cut with the same 0.7mm corn mill. 

Process is:

* prepare Gerber Export (I use EasyEDA, but every sofware capable of exporing gerber files will do)

* run pcb2gcode (https://github.com/pcb2gcode/pcb2gcode) with the following millproject file (create "out" directory before!):

        output-dir=./out

        # source files (replace with you naming!)
        back=Gerber_BottomLayer.GBL
        outline=Gerber_BoardOutlineLayer.GKO
        drill=drill_merged.drl

        # common settings
        path-finding-limit=4
        backtrack=10

        # common machine specific settings
        nog81=true
        nog64=true
        nom6=true
        metric=true
        metricoutput=true
        zero-start=true
        zsafe=3mm
        zchange=50mm

        # milling (60 deg, 0.2mm tip V-carve)
        # https://hobbycnc.com/tool-width-calculator/
        zwork=-0.1mm
        mill-feed=180mm/min
        mill-vertfeed=100mm/min
        mill-speed=12000rpm
        mill-diameters=0.31547mm
        isolation-width=0.5mm

        # drilling (milldrilling) (0.7mm corn mill)
        min-milldrill-hole-diameter=0 
        milldrill-diameter=0.7mm
        zmilldrill=-1.7mm
        zdrill=-1.7mm
        drill-feed=80mm/min
        drill-speed=12000rpm

        # cutting (0.7mm corn mill)
        cutter-diameter=0.7mm
        zcut=-1.7mm
        cut-infeed=0.6mm # 3 passes
        cut-feed=120mm/min
        cut-speed=12000rpm


* run resulting `back.ngc`, `milldrill.ngc`, and `outline.ngc` on your CNC.
* solder and have fun!

Hints:
* EasyEDA creates multiple drill files. I use `gerbv` (https://gerbv.github.io) to merge them.