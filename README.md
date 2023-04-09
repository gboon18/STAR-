# STAR Zr+Zr and Ru+Ru collisions (at $\sqrt{s_{NN}} = 200 GeV$) of data analysis
## Collision event-by-event net-proton number fluctuation analysis code.
Thousands of particle produced in the collisions $\rightarrow$ statistical mechanics.\
Our detector is detecting a part of the phase space $\rightarrow$ Grand Canonical Ensemble
Grand particion function looks like: $lnZ^{GCE}(T,V,\mu) = ln\left [ \sum_N e^{\mu N / T} Z^{CE} (T, V, N) \right ] $, where $T$ is the temperature, $V$ is the volume, and $\mu$ is the chemical potential. \
Cumulant generating function $G_n(t) = ln\left \langle e^{tN} \right \rangle = \sum_{n=1}^{\infty} C_n \frac{t^n}{n!}$, where $C_n$ is the n-th order cumulant. \
Then the relation between the Grand partition function and the cumulants can be written as $C_n \propto \frac{\partial \left ( ln Z^{GCE} \right )}{\partial \mu^n} $. \
Therefore, cumulants measure chemical potential derivatives of the equation of state.
- Susceptibility: charactrize the response of the system (in this case especially, baryonic chemical potential)
- Cumulants: measure statistical properties of the probability distribution: describe fluctuations and correlations in conserved quantities in the system (in this case especially, net-baryon/proton number)

Let's calculation net-proton number cumulants and go through how we correct the inefficiency of the detector and estimate the statistical uncertainties of the observables.

## Table of contents

- [Quick start](#quick-start)
- [Net-proton number fluctuation] (#net-proton-number-fluctuation)
- [Detector efficiency correction] (#detector-efficiency-correction)
- [Centrality bin width correction] (#centrality-bin-width-correction)

## Quick start
This program requires c++11 and CERN ROOT [v6.22/06](https://root.cern/releases/release-62206/).
At this stage, we have collected proton and anti-proton tracks from the STAR detector at the Relativisitic Heavy-Ion Collider at Brookhaven National Laboratory.
We have taken into account the particle track detection efficiency.
The calculations are done in [calcum](https://github.com/gboon18/STAR/tree/main/calcum) directory.
The CalCum.cpp calculates the cumulants up to the sixth order.
Cum.cpp fills the cumculants to the histogram in the event multiplicity bin and merge the multiplicity bins to several centrality with the centrality bin widht correction (CWBC).
First, let's clone the code 
```sh
git clone https://github.com/gboon18/STAR.git
```
then run
```sh
./Cum.sh
```

After the calculations are done in [calcum](https://github.com/gboon18/STAR/tree/main/calcum) directory, we move to the plot directory to plot the results.


## Net-proton number fluctuation
- moments analysis of the probability distribution
To study the thermodynamic properties of the matter, hadronic matter and Quark Gluon Plasma (QGP), formed in the heavy-ion collisions, we do fluctuation analysis. In this particular case, net-proton number analysis.
It is a proxy to the net-baryon number which is an collision event-by-event conserved quantity.
The fluctuation of this conserved quantity arises due to the underlying interaction and statistical nature of the interaction (partonic and/or hadronic) of the particles.
Depending on the nature of transition from the QGP to hadronic matter, i.e. is it a smooth crossover or the first-order?, the aspect of the fluctuation will be different.

- Correlation length
Correlation length si a measure of distance which physical quantities are correlated. 
QGP has longer correlation length than the hadronic matter: imagine QGP being a more homogeneous soup of partons and gluons where hadronic matter.
If the transition from the QGP to hadronic matter is smooth, the change of correlation length will also be smooth.
Near the critical point, if it exists, the correlation length is expected to diverge: checkout [critical opalescence](https://www.youtube.com/watch?v=DIGdbmJvFUw) due to increased correlation length near the critical point.

The fluctuation of the conserved quantities like net-baryon number will be present in its distribution.
<!--![Net-proton distribution](https://github.com/gboon18/STAR/blob/main/images/net-proton_distribution.png)-->
<img src="https://github.com/gboon18/STAR/blob/main/images/net-proton_distribution.png" alt="Net-proton distribution" width="700*0.7" height="500*0.7">

- Moment analysis
The event-by-event distributions of the conserved quantities are characterized by the moments such as mean, standard deviation, skewness, kurtosis, etc.
The cumulants can be derived from these moments and the higher the order, the more it become sensitive to the distribution.
In very short, the higher the order, the more it become sensitive to the correlation length and to the phase transition.

$$ 
\begin{align} 
        C_1 &= \left \langle N \right \rangle, \\ 
        C_2 &= \left \langle \left ( \delta N \right )^2 \right \rangle, \\ 
        C_3 &= \left \langle \left ( \delta N \right )^3 \right \rangle, \\ 
        C_4 &= \left \langle \left ( \delta N \right )^4 \right \rangle
                - 3 \left \langle \left ( \delta N \right )^2 \right \rangle^2, \\ 
        C_5 &= \left \langle \left ( \delta N \right )^5 \right \rangle
                - 10 \left \langle \left ( \delta N \right )^2 \right \rangle
                        \left \langle \left ( \delta N \right )^3 \right \rangle, \\ 
        C_6 &= \left \langle \left ( \delta N \right )^6 \right \rangle
                + 30 \left \langle \left ( \delta N \right )^2 \right \rangle^3
                    - 15 \left \langle \left ( \delta N \right )^2 \right \rangle
                            \left \langle \left ( \delta N \right )^4 \right \rangle 
                    - 10 \left \langle \left ( \delta N \right )^3 \right \rangle^2.
\end{align}
$$

## Detector efficiency correction
In real situation, the detector cannot detect all the particles. That is why we are detecting protons and anti-protons, not neutrons and all other bayrons.
We take the detector efficiency into account considering that the efficiencies for the generated particles are independent to each other.
Then we can describe the detector efficiency with binomial distribution.
This binomial assumption is applicable if we segment the acceptance of the detector as finely as we can.
Say we have a probability of $p$ to detect a particle every single time.
Then, the distribution of $n$ number of detected particles $\widetilde{P}(n)$ will be is: $\bar{P}(n) = \sum_N P(N) B_{p,N}(n)$,\
where $P(N)$ is the distribution of a $N$ number of particles and $B_{p,N}(n)$ represents the binomial distribution: $\frac{N!}{n!(N-n)!}p^n(1-p)^{N-n}$.

## Statistical uncertainty
We use delta theorem to estimate the statistical uncertainty.
The Delta theroem builds off of the central limit theorem and allows us to approximate the uncertainty when direct calculation of statistical uncertainty is not feasible.
Central limit theorem states: a random sample $\{  X_1,...,X_n \}$ of size $n$, where $n$ is a sequence of indentically distributed random variables from a distribution of expected value given by $\mu$ and $\sigma^2 (< \infty)$. As the sample number goes to infinity, the difference between sample average $\bar{X}$ and $\mu$ approximate to a normal distribution with mean 0 and variance $n$.

$$
\sqrt{n} \left ( \bar{X} - \mu \right ) \rightarrow N \left (0, \sigma^2 \right ) \
$$

Building off from the central limit theorem, the Delta theorem states: If there is a sequence of random variables $X_n$ satisfying the central limit theorem, any function of sample average $g(\bar{x})$ can be expressed as normal distribution. 

$$
\sqrt{n} \left ( \bar{g(X)} - g(\mu) \right ) \rightarrow N \left (0, \sigma^2 \cdot g'(\mu)^2 \right ) \
$$

This is the first-order Taylor expansion of $g$ around the $\mu$.

## Centrality bin width correction (CBWC)
The multiplicity-bin-by-bin resolution of the cumulants and ratios can be low due to low statistics in high order of cumulants.
We can merge the multiplicity bins into several collision centrality classes (0-5%, 5-10%, 10-20%, 20-30%,..., 70-80%) depending on the number of produced particles (multiplicity) in the event.
During merging the bins, we assign weight to the observables so that the signals don't get lost.
This method is called the centrality bin width correction (CBWC).
