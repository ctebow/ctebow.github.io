---
title: Engineering a CubeSat Payload
date: 2026-04-1
blurb: Characterizing and testing the fine-pointing system for the PULSE-A CubeSat.
tags:
repo:
link:                                 # optional live demo URL
image:                                # optional, e.g. /assets/img/example.png
featured: true
---

[github repository](https://github.com/ctebow/mirrorctl/tree/main)

### Background

The UChicago Space Program (UCSP) is building a 3U CubeSat called PULSE-A, set to
launch in summer 2027, that will demonstrate optical communication using circular
polarization shift keying (CPolSK) to transmit data at speeds of 10 megabits/second. 
I am an optical payload engineer for PULSE-A, where this past year I've developed control firmware, 
characterized, and written closed-loop tracking logic for the payload's fast steering mirror (FSM).
The FSM is a micro-electro mechanical device that rotates a small mirror up to +/- 4 degrees on two axes
at rates over 200 Hz with a resolution of single microradians. The FSM is essential for correcting for
fine pointing error that our satellite will experience, which is crucial for optical signal alignment. 

### Technical Details

My work can be divided into three parts: 
1. FSM Control and Interface
2. FSM Characterization
3. Fine Pointing Algorithm Design

#### FSM Control and Interface
The FSM is controlled using biased quad differential (BQD) voltages to tilt its mirror.
Any improper use, such as exceeding voltage or rate limits, can permanently break the
($10,000!!!) mirror, meaning I needed to develop a reliable, safe, and error proof FSM
translation layer to convert from input voltages to hexadecimal DAC commands.

I accomplished this by using a raspberry pi 4b as my "brain", and developing 
a safe user-facing "FSM" object in Python that handles all communication over SPI
with the FSM DAC, that also auto-enforces rate and voltage limits for safety. The object 
includes user methods for FSM setup, slewing to a voltage on a single axis, status checks, and FSM
shutdown. Below is one of the first testing setups, with the rasperry pi serving as a
stand-in for the actual satellite beaglebone flight computer. 

![An image of the first FSM testing setup](/assets/img/fsm_setup.png)

#### FSM Characterization
For proper operation, the voltage response of the FSM had to be characterized in order 
to gain an angular displacement to voltage look up table (LUT). This process can also expose any hysterisis and temerature 
dependency of the mirror.

In order to obtain the angle-voltage LUT, I used a ChArUco board (seen above) along with a raspberry pi camera.
Using custom camera calibration code with picamera and OpenCV running locally on the raspberry pi, 
I created camera lens and homography matrices to account for lens distortion and the off-angleness 
of the camera. With the matrices and the ChArUco board, and while shining a laser on the FSM along with knowing the distance from the FSM to the board, I could sweep the FSM through known voltages and use OpenCV to find the exact coordinates of the laser centroid, using the small angle approximation to obtain angular displacement as a function of voltage. I was able to obtain the following characterization, which closely matches manufacturer specifications.

#### Fine Pointing Algorithm Design




