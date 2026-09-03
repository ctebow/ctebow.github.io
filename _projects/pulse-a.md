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
[video of pid loop](https://youtube.com/shorts/TyAX-cJnz0A?feature=share)

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

<img class="photo" src="/assets/img/fsm_setup.png" alt="An image of the first FSM testing setup">

#### FSM Characterization
For proper operation, the voltage response of the FSM had to be characterized in order 
to gain an angular displacement to voltage look up table (LUT). This process can also expose any hysterisis and temerature 
dependency of the mirror.

In order to obtain the angle-voltage LUT, I used a ChArUco board (seen above) along with a raspberry pi camera.
Using custom camera calibration code with picamera and OpenCV running locally on the raspberry pi, 
I created camera lens and homography matrices to account for lens distortion and the off-angleness 
of the camera. With the matrices and the ChArUco board, and while shining a laser on the FSM along with knowing the distance from the FSM to the board, I could sweep the FSM through known voltages and use OpenCV to find the exact coordinates of the laser centroid, using the small angle approximation to obtain angular displacement as a function of voltage.

#### Fine Pointing Algorithm Design
Detailed below is a diagram of PULSE-A's optical payload, where, importantly,
the FSM steers a beacon laser towards a quadrant photo diode (QPD). The feedback from the QPD allows us
to properly align the data laser. 
![Diagram of the PULSE-A optical payload](/assets/img/pulse_payload_diagram.png)

To ensure the beacon laser is kept centered on the QPD, I developed a two-stage procedure to steer the FSM.
In the first stage, aquisition, the FSM steers the laser in a spiral until the QPD detects the laser. This stage is 
dependent on the previously obtained angular displacement to voltage mapping, as the system is running in an open loop. 

As soon as the QPD is detected, the procedure switches to the second stage, tracking, which uses a proportional-integral-derivative (PID)
loop to keep the laser centered on the QPD. I spent time tuning both the proportional and integral components of the PID loop, and still need
to tune the derivative components. To test the PID code, my partner and I fed in manufacturer-provided pointing error into the mirror
system using a second FSM, and then used the PID FSM to correct for the error. The correction results [1] are displayed below, showing the FSM performed quite well. 
![Graph of the PID loop performance](/assets/img/pulse_pid_results.png)

### Comments and Improvements
It took a ton of time, energy, and help from teammates much smarter than I am to help me get the FSM working! Here are a couple things I learned/challenges I faced.
- Considering the skills of my teammates! My teammates' knowledge of optics and electronics helped me tremendously, and allowed me to setup a mock payload in hours which would have taken me days on my own. 
- Working in a shared optics lab! My entire project took place on about six square feet of an optical table in a shared lab, and I quickly had to learn how to organize my space and communicate with other lab users to ensure I was respecting their experiments. I am very thankful for the other graduate students and professors in the lab that taught me the do's and dont's. I also got to learn a bit about networking, since wireless signals were banned in the lab and I had to configure my own router!
- The FSM and surrounding electronics were incredibly finicky. I had to get used to things going wrong and breaking. The DAC board would often just stop working mid-test at the slightest touch, the ChArUco board calibration became invalid if the board moved even a millimeter, and I even shorted-out the manufacturer-provided (>$1000) FSM controller. 3D-printing a case for the DAC and supports for the ChArUco board, reaching out to others for help, and when all else failed just pushing on were all things that helped me succeed. 

### Presentations and Media
- I presented some of my work at UChicago's undergraduate research symposium, which can be viewed [here](/assets/img/2026%20CCRF%20PULSE-A%20Payload%20Fine%20PAT%20Draft%203%20(2).pdf).
- A video of the PID loop in action can be viewed [here](https://youtube.com/shorts/TyAX-cJnz0A?feature=share).
- A much more detailed description of the above information can be found [here](/assets/img/graydonsk_physics_thesis.pdf), in the bachelors thesis of my lab partner Graydon Schulze-Kalt.
- More information about PULSE-A can be viewed [here](https://www.uchicagospaceprogram.org/cubesat) (I highly recommend you check it out!!!). 

[1] Schulze-Kalt, G. (2026). Preliminary design, analysis, and characterization of the pointing, acquisition, and tracking subsystem for the PULSE-A CubeSat [Bachelor’s thesis, University of Chicago]. Department of Physics.


