---
title: Light Sheet Fluorescence Microscopy
subtitle: "A stability result for the reconstruction of fluorophore distribution
  is established for the 2D Light Sheet Fluorescence Microscopy model. The
  result is equivalent to a Lipschitz-type stability for the reconstruction of
  the initial temperature for the heat equation in $\\mathbb{R}$. "
date: 2021-02-23T02:28:58.054Z
summary: "A stability result for the reconstruction of fluorophore distribution
  is established for the 2D Light Sheet Fluorescence Microscopy model. The
  result is equivalent to a Lipschitz-type stability for the reconstruction of
  the initial temperature for the heat equation in $\\mathbb{R}$.  "
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## Overview

This post describes the research developed during my MSc thesis in applied mathematics. The inverse problem that I shall explain was firstly established by Evelyn Cueva in her PhD thesis, where she also solved numerically this problem and a uniqueness theoretical result was demonstrated. My work consisted of demonstrating the stability of the inverse problem but I also proposed a new numerical algorithm based on deep learning techniques.

The Light Sheet Fluorescence Microscope (LSFM in what follows) is a modern technique that allows biological researchers to observe live specimens and dynamical processes at high resolution in space and time. In fluorescence microscopes, first, some relevant structures of the tissue are labelled with *fluorophores*, particles that can fluoresce. Then, a light source excites the fluorophores and finally, they emit light at some frequency that is measured by cameras as shown in the figure. 

{{< figure library="true" src="LSFM.PNG" title="LSFM steps. First, fluorophores within a plane are excited and, second, they emit fluorescent light captured by cameras." numbered="true">}}

The main characteristic of the LSFM is that illuminates the specimen with thin light sheets, plane by plane, leading to optical sectioning. This approach reduces light exposure and, in consequence, photo-toxicity and photo-bleaching are trimmed down as opposed to other fluorescence microscopes. The latter allows long periods of time for acquisition among other benefits that make the LSFM one of the preferred techniques for 3D imaging of big specimens. As a result, this procedure gives a stack of 2D images along the direction of detection. However, as with almost every optical phenomena, it presents problems such as blurring, and shadows as the laser goes through the object. One of the reasons for this is the scattering that photons suffer, exciting fluorophores not only in the focal plane but also in close-plane. Hence, we want to reconstruct the fluorophore distribution from the images of the microscope, for which an inverse problem is established.


Thus, the direct model is divided into two steps: **illumination** or **excitation** and **projection** or **fluorescence**. A PDE describing the photon distribution is used for each step obtaining an explicit formula that shall be related to the solution of the heat equation in $\mathbb{R}$.

## The model

As a first idea, a 2D model is set, so, instead of considering a 3D specimen, we illuminate a 2D object and, instead of illuminating plane by plane, we illuminate lines or fibres of the specimen emitting laser beams at different heights. To move from this 2D model to the 3D one, the plane illumination shall be modelled with a collection of laser beams.

{{< figure library="true" src="LSFM_2D.png" title="2D model for LSFM." numbered="true">}}

### Illumination step. Fermi pencil-beam equation.

For the illumination step, it is considered the Fermi pencil-beam equation, which describes the transport of photons in a highly scattering and highly peaked forward regime when emitted from height $y=0$. The equation is as follows:

$$\begin{cases}\begin{array}{rcl}
(\partial_x + \theta_y \partial_y +\lambda(x,h) - \psi(x,h) \partial_{ \theta_y}^2)u(x,y,\theta_y) & = & 0 \\\\
u(x_h,y,\theta_y) & = & \delta_h(y)\delta_0(\theta_y)\\\\
x\in(x_h,\infty), y\in \mathbb{R}, \theta_y\in \mathbb{R} &&
\end{array}\end{cases}$$

But we are interested in the function $v_h(x,y)$ that describes the photon distribution independent of the angle $\theta_y$:

$$\begin{array}{rcl} v_h(x,y)&=&\displaystyle\int_{\mathbb{R}}u(x,y,\theta_y)d\theta_y \\\\
&=&\text{exp} \left(-\displaystyle\int_{x_h}^x\lambda(\tau,h)d\tau\right) \dfrac{1}{\alpha(x,h)\sqrt{2\pi}}\text{exp}\left(-\dfrac{(y-h)^2}{2\alpha^2(x,h)}\right) \end{array}$$

Hence, the fluorescent source $w_h(x,y)$ (that is, the fluorophores that have been excited) is given by

$$w_h(x,y) = c \cdot \mu(x,y) \cdot v_h(x,y)$$

### Fluorescence step. Radiative transfer equation.

Fluorophores excited will now be able to emit fluorescent light. Let $p_h(x,\theta)$ be the intensity of photons at point $x$ moving along the direction $\theta$ and coming from fluorophores when illuminating at height $h$. We consider that the transport of photons is governed by a linear transport equation with attenuation $a$ and source $w_h$:

$$\begin{cases}\begin{array}{rl}
\theta\cdot \nabla_{x,y}p_h(x,y,\theta) + a(x,y)p_h(x,y,\theta) = w_h(x,y), & \forall (x,y)\in \mathbb{R}^2, \theta\in \mathbb{S}^1 \\\\

\displaystyle\lim_{t\to \infty}p_h((x,y)-t\theta,\theta) = 0, & \forall (x,y)\in \mathbb{R}^2, \theta\in \mathbb{S}^1
\end{array}\end{cases}$$

$$\begin{cases}\begin{array}{rl}
\boldsymbol{\theta}\cdot \nabla_{x,y}p_h(x,y,\boldsymbol{\theta}) + a(x,y)p_h(x,y,\boldsymbol{\theta}) = w_h(x,y), & \forall (x,y)\in \mathbb{R}^2, \boldsymbol{\theta}\in \mathbb{S}^1 \\\\

\displaystyle\lim_{t\to \infty}p_h((x,y)-t\boldsymbol{\theta},\boldsymbol{\theta}) = 0, & \forall (x,y)\in \mathbb{R}^2, \boldsymbol{\theta}\in \mathbb{S}^1
\end{array}\end{cases}$$

