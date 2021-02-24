---
title: Light Sheet Fluorescence Microscopy. MSc thesis.
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

This post describes in a non-rigorous way the research developed during my MSc thesis in applied mathematics. The inverse problem that I shall explain was firstly established by Evelyn Cueva in her PhD thesis, where she also solved numerically this problem and a uniqueness theoretical result was demonstrated. My work consisted of demonstrating the stability of the inverse problem but I also proposed a new numerical algorithm based on deep learning techniques.

{{< icon name="download" pack="fas" >}} {{% staticref "media/presentacion_cortada.pdf" "newtab" %}}You may download the presentation (in spanish) of my thesis here.{{% /staticref %}}  

The Light Sheet Fluorescence Microscope (LSFM in what follows) is a modern technique that allows biological researchers to observe live specimens and dynamical processes at high resolution in space and time. In fluorescence microscopes, first, some relevant structures of the tissue are labelled with *fluorophores*, particles that can fluoresce. Then, a light source excites the fluorophores and finally, they emit light at some frequency that is measured by cameras as shown in the figure. 

{{< figure library="true" src="LSFM.PNG" title="LSFM steps. First, fluorophores within a plane are excited and, second, they emit fluorescent light captured by cameras." numbered="true">}}

The main characteristic of the LSFM is that illuminates the specimen with thin light sheets, plane by plane, leading to optical sectioning. This approach reduces light exposure and, in consequence, photo-toxicity and photo-bleaching are trimmed down as opposed to other fluorescence microscopes. The latter allows long periods of time for acquisition among other benefits that make the LSFM one of the preferred techniques for 3D imaging of big specimens. As a result, this procedure gives a stack of 2D images along the direction of detection. However, as with almost every optical phenomena, it presents problems such as blurring, and shadows as the laser goes through the object. One of the reasons for this is the scattering that photons suffer, exciting fluorophores not only in the focal plane but also in close-plane. Hence, we want to reconstruct the fluorophore distribution from the images of the microscope, for which an inverse problem is established.


Thus, the direct model is divided into two steps: **illumination** or **excitation** and **projection** or **fluorescence**. A PDE describing the photon distribution is used for each step obtaining an explicit formula that shall be related to the solution of the heat equation in $\mathbb{R}$.

## The model

As a first idea, a 2D model is set, so, instead of considering a 3D specimen, we illuminate a 2D object and, instead of illuminating plane by plane, we illuminate lines or fibres of the specimen emitting laser beams at different heights. To move from this 2D model to the 3D one, the plane illumination shall be modelled with a collection of laser beams.

{{< figure library="true" src="LSFM_2D.png" title="2D model for LSFM." numbered="true">}}

In what follows, the fluorophore distribution shall be denoted $\mu$ and will be the unknown for the inverse problem. An example of this function is given in the next figure:

{{< figure library="true" src="mu.PNG" title="Fluorophore distribution $\mu$." numbered="true">}}

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
\theta \cdot \nabla_{x,y}p_h(x,y,\theta) + a(x,y)p_h(x,y,\theta) = w_h(x,y), & \forall (x,y)\in \mathbb{R}^2, \theta\in \mathbb{S}^1 \\\\
\displaystyle\lim_{t\to \infty}p_h((x,y)-t \theta,\theta) = 0, & \forall (x,y)\in \mathbb{R}^2, \theta\in \mathbb{S}^1
\end{array}\end{cases}$$

The boundary condition states that there are no external sources. There exists a unique solution for this equation: 

$$p_h(x,y,\boldsymbol{\theta}) = \displaystyle\int_{-\infty}^{0}w_h((x,y)+r\boldsymbol{\theta})\text{exp}\left(-\displaystyle\int_r^0a((x,y)+\tau\boldsymbol{\theta})d\tau\right)dr$$

We are interested in what is measured in pixel $s$ and, also, we assume that cameras are collimated. Hence, the final measurement obtained at pixel $s$ when illuminating at height $h$ is given by the next expression:

\begin{equation}\label{eq:measurements}
\begin{array}{rcl}
p(s,h) & = & c\cdot\text{exp}\left(-\displaystyle\int_{x_h}^s\lambda(\tau,h)d\tau\right) \cdot \\\\ &&\displaystyle\int_{-\infty}^{\infty}\dfrac{\mu(s,r)e^{-\int_{r}^{\infty}a(s,\tau)d\tau}}{\alpha(s,h)\sqrt{2\pi}}\text{exp}\left(-\dfrac{(r-h)^2}{2\alpha^2(s,h)}\right)dr
\end{array}
\end{equation} 

An example for measurements obtained when illuminating by left and right the source in figure 2 and measuring with upper and lower cameras is given in the next figure:

{{< figure library="true" src="mediciones_finales.PNG" title="Final measurements given by the model." numbered="true">}}

## The inverse problem and the heat equation.

The inverse problem consists of reconstructing the fluorophore distribution $\mu$ from measurements $p(s,h)$. We suppose as known the functions $\lambda$, $a$ and $\psi$. The latter function may be seen as the convolution of $\mu(s,\cdot)\cdot e^{-\int_{\cdot}a(s,\tau)d\tau}$ with the heat kernel. In fact, let $s$ fixed and define 

$$
\begin{array}{rcl}
\sigma(y)&:=& \dfrac{1}{2}\alpha^2(s,y) = \dfrac{1}{2}\displaystyle\int_{x_h}^s(s-\tau)^2\psi(\tau,y)d\tau\\\\
f(r) & := & \mu(s,r)\text{exp}\left(-\displaystyle\int_r^{\infty}a(s,\tau)d\tau\right)\\\\ 
g(y) &:=& \dfrac{1}{c}p(s,y)\text{exp}\left(\displaystyle\int_{x_h}^s\lambda(\tau,y)d\tau\right)
\end{array}$$
hence, 
$$g(y) = \displaystyle\int_{\mathbb{R}}\dfrac{f(r)}{\sqrt{4\pi\sigma(y)}}\text{exp}\left(-\dfrac{(r-y)^2}{4\sigma(y)}\right)$$ 

Thus, considering the heat equation in $\mathbb{R}$:

$$\begin{cases}\begin{array}{rcll}
u_t-u_{yy} &=& 0, & (y,t)\in \mathbb{R}\times (0,+\infty),\\\\
u(y,0) & = & f(y), & \text{si } y\in Y_s,\\\\
u(y,0) & = & 0, & \text{si } y\notin Y_s, \\\\
\displaystyle\lim_{|y|\to \infty}u(y,t) & = & 0, & \forall t>0   
\end{array}\end{cases}$$

we have

$$u(y,\sigma(y)) = \displaystyle\int_{\mathbb{R}}\dfrac{f(r)}{\sqrt{4\pi \sigma(y)}}\text{exp}\left(-\dfrac{(y-r)^2}{4\sigma(y)}\right)dr = g(y)$$

That is, we can see $g$ as the measurements or available information, $f$ is the initial condition related to our unknown $\mu$ and $\sigma$ may be related to the time variable for the heat kernel. Thus 

> The fluorophore distribution $\mu$ is related to the initial condition of the heat equation.

Observations of $u$ are available on the curve $\Gamma:=\{(y,\sigma(y)): y\in Y_s\}\subseteq \mathbb{R}\times[0,+\infty)$, which turns out to have the following form:

{{< figure library="true" src="gamma.png" title="Measurements of $u$ are available on the curve $\Gamma$." numbered="true">}}

### Uniqueness of the inverse problem

By linearity, the uniqueness of the inverse problem is reduced to an injectivity result:

> Theorem. Let $s$ be fixed. If $p(s,y)=0$ $\forall y \in Y_s$, then $\mu(s,y)=0$ $\forall y\in Y_s$.

But this result is a direct consequence of a uniqueness result for the heat equation:

> Theorem. Let $\sigma \in C^1_c(\mathbb{R})$ and $\Gamma = \{(t,y) \in \mathbb{R}^2: t=\sigma(y)\}$. Let $\underline{y}=inf(supp(\sigma))$ and $\overline{y}=sup(supp(\sigma))$. Assume that there exists $\delta>0$ such that $\sigma'(y)>0$ in $(\underline{y},\underline{y}+\delta)$. Hence, if $u\equiv 0$ on $\Gamma$, then $u(\cdot,0)\equiv 0$.

### Stability of the inverse problem

A stronger (but harder) result than uniqueness is stability, which asks for the continuity of the inverse operator, that is if small changes in measurements lead to small changes in the respective fluorophore distributions. From the numerical point of view, stability is a critical point since mathematical models do not represent reality in a perfect way and the instruments used to measure may present noise. Thus, if the problem is not stable, the reconstructed solution may present huge differences from the original solution.  

A logarithmic and a Lipschitz-type stability results are presented.

> Theorem. Let $0<\beta<1$. Let $u$ be the solution of the heat equation in $\mathbb{R}^n$ with $u_0 \in A=\{\, a\in H^{2\beta}(\mathbb{R}^n) \vert\vert a\vert\vert_{H^{2\beta}(\mathbb{R}^n)}\leq M\,\}$. Let $0\leq \tau<T$ and $\omega \subset \mathbb{R}^n$ be an open set with bounded complement such that $\omega\times(\tau,T)$ is the observation set. If $\vert\vert u\vert \vert_{L^2(\omega\times(\tau,T))}<1$, then for every $0<\varepsilon<(T-\tau)/2$, $\delta>0$ and $\theta\in(\tau+\varepsilon,T-\varepsilon)$ there exist constants $\kappa=\kappa(\beta)\in (0,1)$ and $C=C(\varepsilon,\delta,M,\theta)>0$ such that
$$\vert\vert u_0\vert\vert_{L^2(\mathbb{R}^n)}\leq C\left(-log\vert\vert u\vert\vert_{L^2(\omega\times (\tau,T))}\right)^{-\kappa}$$

> Theorem. Let $0< t_1< t_2$ and $R>0$. Let $u_0 \in L^1(\mathbb{R}^n)$ such that $supp(u_0)\subset B:=B(0,R)$. Ther exists a constant $C>0$ such that 
$$\vert\vert u_0\vert\vert_{L^1(\mathbb{R}^n)}\leq C \vert\vert u\vert\vert_{L^2(2B\times(t_1,t_2))}$$