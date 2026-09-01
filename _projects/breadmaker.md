---
title: CNN for Circuit Elements
date: 2026-01-15                      # used for ordering (newest first)
blurb: Trained a CNN to detect and digitially lay out hand-drawn circuit elements. 
tags:
repo:
link:                                 # optional live demo URL
image:                                # optional, e.g. /assets/img/example.png
featured: true
---

[github -- CNN](https://github.com/ctebow/breadMaker-backend)
[breadmaker gui](https://bread-maker-frontend.vercel.app/)

### Background

While working in my lab at school, I noticed that copying over hand-drawn circuit
diagrams into programs like LT-Spice took a ton of time, and I wanted to see if 
I could make some code that allowed the user to just take a picture of their hand-drawn
circuit and import it into a circuit schematic design program. I ended up training 
a CNN on about 10,000 hand-drawn images, testing its confidence, class loss, and box loss,
and then making my own circuit-design website that I could import hand-drawn schematics into. 

### Technical Details

This project was split into two major parts, the first being training a CNN and developing
a pipeline around it to classify all circuit elements in a hand-drawn diagram and
output a netlist of their connections. The second was coding a simple circuit design 
program to import the CNN-detected netlist into. 

Concretely, I...
- Found over 4000 pre-labeled hand-drawn circuit images containing over 50 classes using Roboflow, 
and then manually augmented them by cropping, distorting, and graying out images to have
a total 10,000 image dataset. 
- Trained a convolutional neural network, Ultralytic's YOLOv8s, with a 70/15/15 train
validate test split on a local NVDIA 3070 GPU over 100 epochs. It performed with 
about 80% Map@50 on test images, with class and box loss <1. 
- Developed a pipeline using OpenCV to take the CNN output and create a netlist 
of connected components by using line segment detection and heuristics (line-class overlap, line angle, line thickness, etc). 
- Built a simple circuit editing website with JavaScript, and linked it to my backend using FastAPI to allow users
to upload hand-drawn images to be classified by my CNN. I deployed everything on Vercel. 

Here is an image of the pipeline being ran on a drawn circuit diagram
![Image of the CNN classifying a drawn circuit diagram](/assets/img/cnn_demo_image.png)

### Comments and Improvements

- Creating a netlist of interconnected components based solely on a hand-drawn image is super difficult, and
it required a ton of trial and error to filter out noisy handwriting, lines, and backgrounds. My current implementation
is over-tuned to a couple test cases, and often fails on data that it hasn't been exposed to. A much more rigorous implementation
would either require higher-quality drawings or contain better tuned heuristics for determining which compoenents are connected. 
- My current backend is too large for Vercel free tier's compute budget (it needs to run a fairly large CNN every call), and only 
works in a higher paid tier. To regain the functionality of the backend, I need to find a better provider or scale down the amount of parameters in my CNN. 
- The JavaScript website is super messy and not maintainable! I would like to refactor it to be much more modular and also use more of a netlist-centered 
system to associate components instead of relying solely on endpoint coordinates. 
