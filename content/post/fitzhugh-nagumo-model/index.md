---
title: FitzHugh-Nagumo model
subtitle: "This two-dimensional system of PDE models an excitable system such
  as  or neurons. It is solved by means of a finite differences scheme. "
date: 2021-02-15T16:24:21.868Z
summary: "This two-dimensional system of PDE models an excitable system such as
  electric signals on heart tissue. It is solved by means of a finite
  differences scheme. Also, an inverse problem is established and partially
  solved: reconstruct cardiac tissue properties by using measurements available
  from an ECG. "
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## Overview

Heart disease is one of the leading causes of death in the world, thus it is necessary a fully-understanding of its biomechanical behaviour. One attempt to do so is the FitzHugh-Nagumo model, one of the most successful mathematical models that describe excitable media, that is, materials consisting of elementary segments with well-defined res state, a threshold for excitation and a diffusive-type coupling to its nearest neighbours. Hence, this model is useful for modelling different phenomena such as nerve pulses, spreading of forest fires or certain types of chemical reactions. In particular, it is used in cardiac electrophysiology to qualitatively describe the voltage propagation through the cardiac tissue. Consequently, it is possible to gain some insights on heart imparities such as arrhythmias.  

The most important characteristic of excitable media is the almost immediate damping out of signals below a certain threshold. On the other hand, signals exceeding this threshold propagate without damping. In cardiac cells, ions move through small pores in the cellular membrane which can be either open (excited) or closed (rest) making the heart muscle pumping blood in and out.

## The equation

The FitzHugh-Nagumo model is a system of two variables and two PDEs. The variables are an **activator** (the electric potential $V$) and an **inhibitor** (a variable $R$ that describes the voltage-dependent probability of the pores in the membrane being open and ready to transmit ionic current).

$$\begin{array}{rcll}
\dfrac{\partial V}{\partial t}&=&\div(\sigma\nabla V)+\nu V(\alpha-V)(V-1)- R + I & \text{, in }\Omega\times[0,T]\\

\dfrac{\partial R}{\partial t}&=&\delta (V-Rd) & \text{, in } \Omega\times[0,T]\\

\dfrac{\partial V}{\partial n}&=& 0 & \text{, on} \partial \Omega \times [0,T]\\

V(x,0) &=& V_0 & \text{, for} x\in\Omega\\

R(x,0) &=& R_0 & \text{, for} x\in\Omega
\end{array}
$$
where
- $V(t,x)$: the voltage or transmembrane potential in the point $x\in\Omega$ and time $t\in[0,T]$. 
- $R(t,x)$: inhibitor or recovery variable.
- $\sigma(x)$: conductivity of the tissue that may depends on $x\in\Omega$. Moreover, $\sigma(x)\geq0$ and if $\sigma(x)=0$ then the tissue on the point $x\in\Omega$ is dead and no diffussion may occur.
- $\nu$: excitation rate, with $\nu>0$.
- $\alpha$: threshold for excitation, with $0<\alpha<1/2$.
- $\delta$: excitation decay, with $0<\delta$.
- $d$: recovery decay, with $\gamma>0$.
- $I$: stimulating current. 

## Solving the equation. Finite differences scheme
