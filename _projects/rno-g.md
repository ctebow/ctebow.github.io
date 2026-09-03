---
title: PCB Design for RNO-G
date: 2026-02-15                      # used for ordering (newest first)
blurb: Developed a custom PCB and tested its integrated PCR distance sensor.
tags:
repo:
link:                                 # optional live demo URL
image:                                # optional, e.g. /assets/img/example.png
featured: true
---

[github -- custom firmware](https://github.com/ctebow/XM125-expose-raw-iq) 
[github -- pcb & analysis](https://github.com/ctebow/xm125)
[technical report](/assets/img/Optimizing%20Distance%20Readings%20on%20XM125.pdf)

### Background

The Radio Neutrino Observitory in Greenland (RNO-G) places radio antenna in 
boreholes in the ice in Greenland in order to detect ultra-high energy neutrinos
via the askaryan effect. These in-ice boreholes can shrink in diameter over time,
which can affect instrument response as well as damage equipment. While working
in the Vieregg Lab at UChicago, I worked on a tunnel diameter logging system
to track how the boreholes shrink over time. 

### Technical Details

To build the logging system, I selected acconeer's XM125 pulsed-coherent radar
sensor, and built a PCB breakout to control the sensor with a teensy 4.1 microcontroller.
After building the logger, I characterized its accuracy and precision. Concretely, I...

- Design, tested, and built a custom printed circuit board using KiCad, hand-soldering
all SMD components while performing electrical checks with an oscilloscope.
- Developed custom firmware for an STM32 MCU to send raw IQ radar data to a master controller over I2C,
and then tested the firmware with my custom PCB.
- Characterized a pulsed coherent radar (PCR) sensor to within 2.5mm of accuracy while
optimizing for power consumption by using a custom Python library to control a Teensy MCU. 

Many details of the project can be found in the technical report linked at the top and also
[here]().

Below is an image of the fully built PCB
![Image of the built distance logging PCB](/assets/img/xm125-front.jpg)

And here is a pretty unconventional testing setup to mock the sensor in a borehole

<img class="photo" src="/assets/img/pcb_setup.png" alt="Image of distance logger in pvc tube">

### Comments and Improvements

- The boreholes in Greenland shrink on the order of a couple mm per year, and after analysis and conversation with the manufacturer, I
determined the acconeer distance sensor only is accurate up to about +/- 2.5mm. This meant it wasn't super useful as a 
precise borehole diameter, and unfortunately would not be deployed in Greenland. However, I still learned a ton about firmware and
PCB design!
- It took several tries to get my PCB up and running, I had to re-manufacture boards twice and I only got a working version on 
my very last board! Persistence was key here, and being super meticulous with my electrical and schematic checks. 


