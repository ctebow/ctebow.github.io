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

### Technical Details

Using GEANT-4, C++, ROOT, and Python with NumPy and Pandas, I built an end-to-end 
simulation pipeline for users to input particle spectra into a mass model of the nDURE
instrument and recover a poisson-smeared, statistically honest distribution of energies 
reconstructed by the detector. I then conducted analysis on the energy resolution, efficiency, and 
unfolding capbailities of the nDURE instrument using the simulation pipeline. 

Here is a diagram of the pipeline:

![A Diagram of the nDURE simulation pipeline](/assets/img/ndure-diagram.png)

GEANT-4 is a package provided by CERN to simulate the passage of particles through matter
using monte-carlo techniques. Using the package, I set up my detector geometry, defined what
constitutes as an "event", a "hit", and what should be reconstructed (essentially what data goes out of the simulation),
while also allowing for user I/O. 

```python
def solve(xs):
    return sum(x * x for x in xs)
```

### Presentations and Publications

I presented my research to the heliophysics division at NASA Goddard, which can be viewed here.

I presented a poster at the NASA Student Research Symposium in August 2026, which can be viewed here.

I'm currently submitting a paper along with my advisor and research partner, which once submitted will be available here. 

### Comments and Improvements

Honest retrospective — this reads well to anyone technical.
