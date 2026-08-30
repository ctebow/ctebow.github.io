---
title: Simulating a Neutron Spectrometer
date: 2026-08-01                      # used for ordering (newest first)
blurb: Using GEANT-4 to simulate and characterize NASA's Neutron Dosimeter for Unkown Radiation Environments.
tags:
repo: https://github.com/ctebow/example
link:                                 # optional live demo URL
image:                                # optional, e.g. /assets/img/example.png
featured: true
---

### Background

Neutrons make up about 20% of expected radiation dosages on the lunar and
martian surfaces, posing a risk to astronauts. NASA's neutron dosimeter for unknown
radiation environments (nDURE) will measure
the energy spectra of neutrons with energies from 1 MeV to over 100 MeV. Importantly,
detector electronic noise, counting statistics, and physical processes distort what
nDURE "sees", and I used simulations to model nDURE's detector effects while also exploring 
ways to "unfold" detector errors from measured data in order to give scientists an
effective way to interpret the radiation readings that nDURE will provide. 

A beam of neutrons passing through my nDURE model:
![A render of nDURE](/assets/img/ndure_render.png)

### Technical Details

Using GEANT-4, C++, ROOT, and Python with NumPy and Pandas, I built an end-to-end 
simulation pipeline for users to input particle spectra into a mass model of the nDURE
instrument and recover a poisson-smeared, statistically honest distribution of energies 
reconstructed by the detector. I then conducted analysis on the energy resolution, efficiency, and 
unfolding capbailities of the nDURE instrument using the simulation pipeline. 

The simulation pipeline:
![A Diagram of the nDURE simulation pipeline](/assets/img/ndure-diagram.png)

Key features of this project include...
- A custom GEANT-4 mass model and simulation routine for the nDURE instrument that 
implements Birks's quenching for particles in a plastic scintillator. 
- A JSON-driven simulation harness that allows for parallel, batched simulation runs and 
on-the-fly input energy calculation using Python.
- A configurable simulation parser that allows for custom energy reconstruction, tunable smearing parameters, and output into 
.root or .csv format with PyRoot. 
- Analysis and data unfolding using CERN's rooUnfold to characterize detector energy resolution,
peak separation, and unfolding performance. 

### Key Results

I used the pipeline I built to compare two separate detectors in their unfolding capability, 
finding that the nDURE instrument out-performs a comparable single-scatter neutron spectrometer.

![An image of the nDURE results](/assets/img/across_spectra_grid.png)

### Presentations and Publications

I presented my research to the heliophysics division at NASA Goddard, which can be viewed [here](/assets/img/Tebow_Caden_HSD_Presentation_Slides.pdf).

I presented a poster at the NASA Student Research Symposium in August 2026, which can be viewed [here](/assets/img/2026%20NASA%20Intern%20Poster%20Session%20DRAFT%202%20(2).pdf).

I'm currently submitting a paper along with my advisor and research partner, which once submitted will be available here. 

### Comments and Improvements

I worked on this project for about nine weeks, learning a ton in a short amount of time while
also running into a ton of roadblocks. Just to name a few roadblocks...
- Not settling on a pipeline data schema early: This caused me a ton of headaches early on,
as my notion of what counted as a "hit" in my detector, how to organize my .root files, and what 
events to write down constantly got mixed up and made analysis of the physics difficult. 
- Space and time complexity! Very quickly, my simulation outputs approached gigabytes in size, and my
simulations became so lengthy I had to run them overnight. As a solution, I came up with strategic ways to parallelize runs, 
cut down on wastful simulations, and drastically reduce which events I was tracking. 
- The underlying physics! There is some super cool (and also very confusing) math and quantum mechanics
behind how incident neutrons eventually get detected by nDURE, and it tooks me weeks of reading textbooks, papers, 
and my lab group's past work to begin to feel comfortable. 

There is also a ton of room for improvement with this project: 
- Simulating neutron detectors using GEANT-4 typically devolves to instrument-specific code, but my pipeline could be useful for other groups working with double-scatter neutron detectors. A high priority improvement is to refactor the C++ code such that other scientists only need to define their own detector geometry, which they can plug in to my simulation logic that tracks hits, double scatters, and does Birk's quenching. 
- The code is not optimized/configured for use on an HPC cluster, which is an important next step for more realistic physics simulations that are
computationally intensive. This would require writing HPC user I/O and more robust versioning for all packages that my project uses. 
