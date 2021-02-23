---
title: Light Sheet Fluorescence Microscopy
subtitle: "A stability result for the reconstruction of fluorophore distribution
  is established for the 2D Light Sheet Fluorescence Microscopy model.  "
date: 2021-02-23T02:28:58.054Z
summary: "A stability result for the reconstruction of fluorophore distribution
  is established for the 2D Light Sheet Fluorescence Microscopy model.  "
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## Overview

This post describes the research developed during my MSc thesis in applied mathematics. The model that I shall explain was established by Evelyn Cueva in her PhD thesis where she also solved numerically the inverse problem and a uniqueness theoretical result was demonstrated. My work consisted of demonstrating the stability of the inverse problem and I also proposed a new numerical algorithm based on deep learning techniques.

The Light Sheet Fluorescence Microscope (LSFM in what follows) is a modern technique that allows biological researchers to observe live specimens and dynamical processes at high resolution in space and time. First, some relevant structures of the tissue are labelled with *fluorophores*, particles that can fluoresce. Then, a light source excites the fluorophores and finally, they emit light at some frequency that is measured by cameras as shown in the figure. 

{{< figure library="true" src="LSFM.PNG" title="A caption" numbered="true">}}

The LSFM illuminates the specimen plane by plane, reducing the light exposure and, in consequence, photo-toxicity and photo-bleaching are not problems in this setting as opposed to other fluorescence microscopes. 

The direct model is divided into two steps: **illumination** or **excitation** and **projection** or **fluorescence**. A PDE describing the photon distribution is used for each step obtaining an explicit formula that shall be related to the solution of the heat equation in $\mathbb{R}$.