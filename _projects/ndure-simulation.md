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
