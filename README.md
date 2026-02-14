# Planetary Science Practical

## *Pandora: A Transmission Spectrum in Search of Life*

Welcome, intrepid explorers, to Pandora!

Pandora was identified as a transiting exoplanet of radius $3 R_{\oplus}$ orbiting $0.03$ AU from a relatively quiet M5V star ($R_* = 0.2 R_{\odot}$, $T_* = 2800$ K), first detected through a subtle, periodic dimming of stellar flux that hinted at the presence of a planetary companion crossing the stellar disk. Subsequent observations by a next-generation space telescope provided transmission spectroscopy. The result is the dataset you now hold in synthetic form: a transmission spectrum of an alien world.

This practical invites you to interpret this transmission spectrum. Your task is to determine whether Pandora’s atmospheric chemistry might plausibly hint at biological activity, or whether the spectrum is fully explicable by abiotic processes.

Download the repository. Load the spectrum. Examine it carefully. Then begin the search.

---

# Transmission Spectroscopy: The Physical Basis

When an exoplanet transits its host star, the observed stellar flux decreases because the planet blocks a fraction of the stellar disk. The wavelength-dependent transit depth is:

$
\delta(\lambda) = \left[\dfrac{R_p(\lambda)}{R_*}\right]^2.
$

where $R_p$ is the planetary radius, $\lambda$ the wavelength, and $R_*$ is the stellar radius.

At wavelengths where atmospheric molecules absorb strongly, the effective altitude of optical depth unity increases, and the planet appears larger. The amplitude of spectral features is controlled by the atmospheric scale height,

$
H = \dfrac{k_B T}{\mu m_p g},
$

which sets the vertical distance over which pressure decreases significantly. Hotter atmospheres, lower gravity, or lighter mean molecular weight gases all increase $H$, amplifying spectral modulation. The height of spectroscopic signals can give a hint at molecules that could be present in abundance in an atmosphere, and that would be otherwise difficult to detect, especially molecules with small or no dipole moment, such as N$_2$ or H$_2$.

Your dataset consists of transit depths across a range of wavelengths. Hidden within that function may be absorption bands corresponding to specific molecules.

---

# Method I: Visual Inspection

Before invoking sophisticated machinery, begin by plotting the transmission spectrum directly and examining its structure by eye. Identify peaks and troughs. Compare them against known molecular cross-sections. Ask whether broad bands align with expected absorption regions of molecules such as H₂O, CO₂, CH₄, N₂O, or others.

---

# Method II: Cross-Correlation

When spectral features are weak or buried in noise, template matching provides a more quantitative detection method. Cross-correlation tests whether a known molecular template spectrum resembles structure present in the data.

Given an observed spectrum $f(\lambda)$ and a template $g(\lambda)$, the discrete cross-correlation as a function of shift $\ell$ is

$
C(\ell) = \sum_{i} f_i , g_{i+\ell}.
$

Continuous form includes an integral in place of the sum.

Often one subtracts the mean and normalizes by variances to compute a normalized cross-correlation coefficient:

$
C_{\text{norm}}(\ell) =
\dfrac{\sum_i (f_i - \bar f)(g_{i+\ell} - \bar g)}
{\sqrt{\sum_i (f_i - \bar f)^2 \sum_i (g_{i+\ell} - \bar g)^2}}.
$


If the template molecule is present in the data, the cross-correlation function should exhibit a statistically significant peak near zero shift (or at a known Doppler shift, if velocity offsets are considered).

This technique effectively stacks many weak absorption lines, increasing signal-to-noise. It is particularly powerful at high spectral resolution, where dense line forests exist.

---

# Method III: Bayesian Atmospheric Retrieval

This is probably too much for you to accomplish in this practical, but it is worth thinking about how you'd implement this.

Retrievals that are informed by Bayes are useful to answer the question: How significant is the correlation? How confident ought I be that the signal I think is there really is?

## The Forward Model

Suppose your atmospheric model predicts a transmission spectrum $ m(\lambda; \theta)$, where $\theta$ represents parameters such as:

* Temperature $T$
* Molecular abundances $X_i$
* Cloud properties
* Reference radius
* Etc. etc.

## Likelihood

Assuming Gaussian observational errors with variance $\sigma_i^2$, the likelihood is

$
\mathcal{L}(\theta) =
\prod_i
\dfrac{1}{\sqrt{2\pi \sigma_i^2}}
\exp\left\{
-\dfrac{[f_i - m_i(\theta)]^2}{2\sigma_i^2}
\right\}.
$

## Priors

Bayesian inference requires priors $P(\theta)$, which encode physical constraints or prior knowledge. For example:

* Log-uniform priors for mixing ratios
* Uniform priors for temperature within plausible bounds
* Informative priors from stellar parameters

## Posterior

By Bayes’ theorem,

$
P(\theta | \text{data}) =
\dfrac{\mathcal{L}(\theta) P(\theta)}
{P(\text{data})}.
$

The denominator is the evidence, often difficult to compute directly, but unnecessary for parameter estimation with MCMC.

## Markov Chain Monte Carlo (MCMC)

MCMC constructs a Markov chain whose stationary distribution is the posterior. For example, the Metropolis–Hastings algorithm proceeds by:

1. Proposing a new parameter set ( \theta' ).
2. Computing the acceptance ratio:
$
\alpha =
\min\left(
1,
\dfrac{\mathcal{L}(\theta') P(\theta')}
{\mathcal{L}(\theta) P(\theta)}
\right).
$
3. Accepting or rejecting the proposal probabilistically.

After convergence, the sampled chain approximates the posterior distribution. From this, you can estimate:

* Median parameter values
* Credible intervals
* Correlations between parameters

You may also compare models using evidence estimates or approximate criteria such as the Bayes Information Criterion (BIC) to evaluate whether adding a molecule significantly improves the fit.

---

# Interpreting Potential Biosignatures

A molecule’s presence alone does not imply life. Context matters. Abiotic pathways can generate many species traditionally associated with biology. Strong claims require:

* Statistical significance in detection
* Chemical plausibility
* Consideration of disequilibrium combinations

For example, sustained coexistence of CH₄ and O₂ in significant quantities indicate continuous replenishment, as these gases react readily, and, as far as we presently know only biology is sufficient to explain this coexistence.

After you identify molecules, discuss among yourselves whether these are indicators of life, and what else you would want to know about this planet to better assess this possibility.

---

# Mission Objectives

1. Plot and inspect the synthetic transmission spectrum.
2. Identify candidate absorption features using the technique that you think is most appropriate.
5. Consider context evidence for life.
6. Argue for or against the plausibility of biological activity.

Document not only what you conclude, but why.

Download the repository.
Run the code.

Somewhere in the noise may lie the atmospheric signatures of a living world.
