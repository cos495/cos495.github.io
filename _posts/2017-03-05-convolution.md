---
layout: post
title: "Convolution and correlation"
date:  Sun Mar  5 09:36:45 EST 2017
author: Sebastian Seung
---
\\(
\newcommand{\Conv}{\mathop{\rm Conv}}
\newcommand{\Corr}{\mathop{\rm Corr}}
\\)

These notes contain basic information about two mathematical operations that are commonly used in signal processing, *convolution* and *correlation*.  Convolution is used to linearly filter a signal.
Correlation is used to characterize the statistical dependencies between two signals.

# Convolution
Consider two time series, $$g_i$$ and $$h_i$$, where the index $$i$$ runs from $$-\infty$$ to $$\infty$$.  Their convolution is defined as

$$
\begin{equation}
\label{eq:ConvolutionInfinite}
(\vec{g}\ast \vec{h})_i = \sum_{j=-\infty}^\infty g_{i-j} h_j
\end{equation}
$$

This definition is applicable to time series of infinite length.  If
$$\vec{g}$$ and $$\vec{h}$$ are finite, they can be extended to infinite length by
adding zeros at both ends.  After this trick, called zero padding, the
definition in Eq. (\ref{eq:ConvolutionInfinite}) becomes applicable.
For example, Eq. (\ref{eq:ConvolutionInfinite}) reduces to

$$
\begin{equation}\label{eq:ConvolutionFinite}
(\vec{g}\ast\vec{h})_i = \sum_{j=0}^{n-1} g_{i-j}h_j
\end{equation}
$$

for the finite time series $$h_0,\ldots,h_{n-1}$$.  

<!---
Another trick for
turning a finite time series into an infinite one is to repeat it over
and over.  This is sometimes called periodic boundary conditions, and
will be encountered later in our study of Fourier analysis.
--->

*Exercise*. Convolution is commutative and associative.  Prove that $$g\ast h=h\ast
g$$ and $$f\ast(g\ast h) = (f\ast g)\ast h$$.

*Exercise*. Convolution is distributive over addition. Prove that $$(g_1+g_2)\ast h
= g_1 \ast h + g_2 \ast h$$. This means that filtering a signal via
convolution is a linear operation.

Although $$g$$ and $$h$$ are treated symmetrically by convolution,
they usually have very different meanings.  Typically, one is a
signal that goes on for a long time.  The other is concentrated
near time zero, and is called a kernel or filter.  The output of the convolution
is also a signal, a filtered version of the input signal.

In Eq. (\ref{eq:ConvolutionFinite}), we chose $$h_i$$ to be zero for
all negative $$i$$.  This is called a *causal filter*, because
$$g\ast h$$ is affected by $$h$$ in the present and past, but not in
the future.  In some contexts, the causality constraint is not
important, and one can take $$h_{-M},\ldots,h_M$$ to be nonzero, for
example.

Formulas are nice and compact, but now let's draw some diagrams to see
how convolution works. To take a concrete example, assume a causal
filter $$(h_0,\ldots,h_{n-1})$$.  Then the $$i$$th component of the
convolution $$(g\ast h)_i$$ involves aligning $$g$$ and $$h$$ this way:

$$
\begin{equation}
\begin{array}{ccccccccccc}
\cdots	&g_{i-m-1}&g_{i-m}&g_{i-m+1}&\cdots	&g_{i-2}&g_{i-1}&g_i 	& g_{i+1} &g_{i+2}&\cdots\\
\cdots	&0&0&h_{m-1}&\cdots&h_2	&h_1	&h_0 	& 0	& 0	&\cdots\\
\end{array}
\end{equation}
$$

In words, $$(g\ast h)_i$$ is computed by looking at the signal $$g$$
through a window of length $$m$$ starting at time $$i$$ and extending back
to time $$i-m+1$$. The weighted sum of the signals in the window is
taken, using the coefficients given by $$h$$.  

Note that $$h$$ is left-right "flipped" in the diagrams. This is because of the minus sign that appears in the indices of convolution.  The flipping may seem counterintuitive, but it's necessary to make convolution commutative.  Also it means the kernel can be interpreted as the impulse response function, which will be defined shortly.

If the signal $$g$$ has a finite length, then the diagram looks
different when the window sticks out over the edges. Consider a signal
$$g_0,\ldots,g_{m-1}$$.  Let's consider the two extreme cases where the
window includes only one time bin in the signal.  One extreme is
$$(g\ast h)_0$$, which can be visualized as

$$
\begin{equation}
\begin{array}{cccccccccc}
\cdots	&0	&\cdots	&0	&g_0 	& g_1	&\cdots	&g_{n-1}& 0	&\cdots\\
\cdots	&h_{m-1}&\cdots	&h_1	&h_0 	& 0	& 0	& 0	& 0	&\cdots\end{array}
\end{equation}
$$

The other extreme is $$(g\ast h)_{m+n-2}$$, which can be visualized as

$$
\begin{equation}
\begin{array}{cccccccccc}
\cdots	&g_0	&g_1	&\cdots	&g_{n-1}&\cdots	& 0	& 0	& 0	&\cdots\\
\cdots	&0	&0	&\cdots	&h_{m-1}&\cdots	&h_1	&h_0	& 0	&\cdots
\end{array}
\end{equation}
$$

The range $$(g\ast h)_0\ldots (g\ast h)_{m+n-2}$$ has length $$m+n-1$$. Outside this "full" range of the convolution, the elements of $$g\ast h$$ must vanish.

The range $$(g\ast h)_{n-1},\ldots,(g\ast h)_{m-1}$$ has length $$m-n+1$$.  Inside this "valid" range, the kernel does not stick out beyond the borders of the signal; the kernel never encounters the zero padding of the signal.  (Here we are assume that the signal $$g$$ is at least as long as the kernel $$h$$, $$m\geq n$$.) 


# Using the `conv` function in Julia and MATLAB.
Suppose that $$\vec{f}=\vec{g} \ast \vec{h}$$, where $$g_0,g_1,\ldots,g_{M-1}$$ and $$h_0,h_1\ldots,h_{N-1}$$.

Then `conv(g,h)` returns the "full" convolution $$f_0,f_1,\ldots,f_{M+N-2}$$. 
It is the only option for Julia and the default option in MATLAB.

Alternatively, `conv(g,h,"valid")` returns the "valid" convolution "$$f_{\min(M,N)-1},\ldots,f_{\max(M,N)-1}$$.  This extension to Julia's built-in `conv` is provided in the sample code accompanying the homework. The "valid" option is available in MATLAB.

The Numpy function is called `convolve`, and has both "full" and "valid" options like MATLAB.

Note that the inputs to the `conv` function don't include information about absolute time.
In general, the inputs could be interpreted as $$g_{M_1},\ldots,g_{M_2}$$ and $$h_{N_1},\ldots,h_{N_2}$$.
Then the output should be interpreted as $$f_{M_1+N_1},\ldots,f_{M_2+N_2}$$. In other words, shifting either $$g$$ or
$$h$$ in time is equivalent to shifting $$g\ast h$$ in time.

For example, suppose that $$g$$ is a signal, and $$h$$ represents an
acausal filter, with $$N_1<0$$ and $$N_2>0$$.  Then the first element of
$$f$$ returned by `conv` is $$f_{M_1+N_1}$$, and the last is
$$f_{M_2+N_2}$$.  So discarding the first $$|N_1|$$ and last $$N_2$$
elements of $$f$$ leaves us with $$f_{M_1},\ldots,f_{M_2}$$. This is
time-aligned with the signal $$g_{M_1},\ldots,g_{M_2}$$, and has the
same length.

# Impulse response
Consider the signal consisting of a single impulse at time zero,

$$
\begin{equation}
\delta_j = 
\left\{
\begin{array}{ll}
1,&	j=0\\
0,&	j\neq 0
\end{array}
\right.
\end{equation}
$$

The convolution of this signal with a kernel $$h$$ is

$$
\begin{equation}
(\delta\ast h)_i = \sum_k \delta_{j-k}h_k = h_j
\end{equation}
$$

which is just the kernel $$h$$ again.  In other words $$h$$, is the
response of the kernel to an impulse, or the *impulse response function*.  If the impulse is displaced from time 0 to time $$i$$, then
the result of the convolution is the kernel $$h$$, displaced by $$i$$ time
steps.

Elsewhere you may have seen the ``Kronecker delta'' notation
$$\delta_{ij}$$, which is equivalent to $$\delta_{i-j}$$.  The Kronecker
delta is just the identity matrix, since it is equal to one only for
the diagonal elements $$i=j$$. You can think about convolution with
$$\delta_j$$ as multiplication by the identity matrix
$$\delta_{ij}$$. More generally, the following exercise shows that
convolution is equivalent to multiplication of a matrix and a vector.

*Exercise*. Matrix form of convolution.
Show that the convolution of $$g_0,g_1,g_2$$ and $$h_0,h_1,h_2$$ can be written as

$$
\begin{equation}
g \ast h = G h
\end{equation}
$$

where the matrix $$G$$ is defined by

$$
\begin{equation}\label{eq:ConvolutionMatrix}
G=\left(
\begin{array}{lll}
g_0	&0	&0\\
g_1	&g_0	&0\\
g_2	&g_1	&g_0\\
0	&g_2	&g_1\\
0	&0	&g_2
\end{array}
\right)
\end{equation}
$$

and $$g\ast h$$ and $$h$$ are treated as column vectors.

*Exercise*. Each column of $$G$$ is the same time series, but shifted by a different
amount.  Use the MATLAB function `convmtx` to create matrices
like $$G$$ from time series like $$g$$.

If you don't have this toolbox installed, you can make use of the fact
that Eq. (\ref{eq:ConvolutionMatrix}) is a Toeplitz matrix, and can
be constructed by giving its first column and first row to the
`toeplitz` command in MATLAB.

*Exercise*. Convolution as polynomial multiplication.
If the second degree polynomials $$g_0 + g_1 z + g_2 z^2$$ and $$h_0 + h_1 z + h_2 z^2$$ are multiplied together, the result is a fourth degree polynomial.  Let's call this polynomial $$f_0 + f_1 z + f_2 z^2 + f_3 z^3 + f_4 z^4$$.  Show that this is equivalent to $$f=g\ast h$$.

<!--
# Correlation
The cross-correlation of two time series is

$$
\begin{equation}\label{eq:Correlation}
\Corr[g,h]_j = \sum_{i=-\infty}^\infty g_{i+j} h_{j}
\end{equation}
$$

You can show this is equivalent to $$\vec{g}\ast\overline{\vec{h}}$$, where $$\bar{\vec{h}}$$ denotes $$\vec{h}$$ after left-right flipping, $$\bar{h}_j = h_{-j}$$. 

As with the convolution, this definition can be applied to finite time
series by using zero padding.  Note that
$$\Corr[g,h]_j=\Corr[h,g]_{-j}$$, so that the correlation operation is
not commutative.  Typically, the correlation is applied to two
signals, while its output is concentrated near zero.

The zero lag case looks like

$$
\begin{equation}
\begin{array}{cccccccc}
\cdots	&0	&g_1	&g_2	&\cdots	&g_{n}	&0	&\cdots\\
\cdots	&0	&h_1	&h_2	&\cdots	&h_{n}	&0	&\cdots
\end{array}
\end{equation}
$$

and the other lags correspond to sliding $$h$$ right or left.  

The autocorrelation is a special case of the correlation, with $$g=h$$.
If $$g\neq h$$, the correlation is sometimes called the crosscorrelation
to distinguish it from the autocorrelation.  

*Exercise*. Prove that the autocorrelation is the same for equal and opposite time
lags $$\pm j$$.

# Using the `xcorr` function in Julia and MATLAB
If $$g$$ and $$h$$ are $$n$$-dimensional vectors, then the MATLAB command
`xcorr(g,h)` returns a $$2n-1$$ dimensional vector, corresponding
to the lags $$j=-(n-1)$$ to $$n+1$$.  Lags beyond this range are not
included, as the correlation trivially vanishes.  The $$n$$th element of
the result corresponds to zero lag.

One irritation is that MATLAB 5 and 6 follow different conventions.
MATLAB 5 calls Eq.\ (\ref{eq:Correlation}) \verb+xcorr(g,h)+, while
MATLAB 6 calls it \verb+xcorr(h,g)+.

A maximum lag can also be given as an argument,
\verb+xcorr(g,h,maxlag)+, to restrict the range of lags computed to
\verb+-maxlag+ to \verb+maxlag+. Then the \verb@maxlag+1@ element
corresponds to zero lag. 

The default is the unnormalized correlation given above, but there is
also a normalized version that looks like

$$
\begin{equation}
Q^{xy}_j = \frac{1}{m} \sum_{i=1}^m x_i y_{i+j}
\end{equation}
$$

To compensate for boundary effects, the form 

$$
\begin{equation}
Q^{xy}_j = \frac{1}{m-|j|} \sum_{i=1}^m x_i y_{i+j}
\end{equation}
$$

is sometimes preferred. Both forms can be obtained through the
appropriate options to the `xcorr` command.

\section{Two examples}
The autocorrelation of a sine wave. The period should be evident.

The autocorrelation of Gaussian random noise. There is a peak at zero,
while the rest is small. As the length of the signal goes to infinity,
the ratio of the sides to the center vanishes.  If the autocorrelation
of a signal vanishes, except at lag zero, we'll call it *white
noise*. The origin of this term will become clear when we study
Fourier analysis.
-->
