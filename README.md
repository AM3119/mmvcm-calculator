# MM-VCM Calculator

[![Live Calculator](https://img.shields.io/badge/Calculator-Live-brightgreen?style=flat-square&logo=github)](https://AM3119.github.io/mmvcm-calculator/)
[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.mimet.2026.107456-blue?style=flat-square)](https://doi.org/10.1016/j.mimet.2026.107456)
[![Zenodo](https://img.shields.io/badge/Zenodo-10.5281%2Fzenodo.19600204-blue?style=flat-square)](https://doi.org/10.5281/zenodo.19600204)
[![Journal](https://img.shields.io/badge/Journal-J.%20Microbiol.%20Methods-orange?style=flat-square)](https://www.sciencedirect.com/journal/journal-of-microbiological-methods)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

**Al Mahrizi-Mossolem Viability Correction Model (MM-VCM)** - Interactive browser-based calculator for delay-corrected microbial load estimation in clinical microbiology.

---

## 🔗 Live Calculator

**➡️ [https://AM3119.github.io/mmvcm-calculator/](https://AM3119.github.io/mmvcm-calculator/)**

No installation required. Runs entirely in the browser - open the link and use immediately.

---

## Background & Rationale

In clinical microbiology, quantitative culture results (CFU/mL) are routinely used to guide diagnosis and treatment decisions. However, a delay between sample **collection** and **measurement** - due to transport, batching, or laboratory workflow - means the counted load Nt reflects the bacterial population *at the time of measurement*, not at collection.

Depending on temperature, organism, and delay duration, the true initial load N0 can differ from Nt by **one to several orders of magnitude**. The MM-VCM model provides a closed-form analytical solution to back-calculate N0 from Nt, correcting for:

- **Growth during delay** (temperature-dependent, organism-specific)
- **Lag phase** before net proliferation begins
- **Decay/death** during storage or transport
- **Carrying capacity** of the biological matrix

This approach is particularly relevant for time-sensitive infections (bacteraemia, meningitis) where load estimation informs severity and therapeutic decisions.

---

## The Mathematical Model

### Inverse (Back-Calculation) Equation

The MM-VCM closed-form solution solves the modified logistic growth equation in reverse:

$$N_0 = \frac{K \cdot N_t \cdot e^{k \cdot dt}}{N_t \cdot e^{k \cdot dt} + \left(K - N_t \cdot e^{k \cdot dt}\right) \cdot e^{r(T) \cdot dt}}$$

### Effective Growth Time

Growth does not begin immediately - a lag phase λ precedes proliferation:

$$dt = \max(0,\; t - \lambda)$$

### Temperature-Corrected Growth Rate

Growth rate follows a Gaussian model centred at optimal temperature (37 °C):

$$r(T) = \frac{\ln 2}{G_{\text{base}}} \cdot \exp\!\left(-\frac{(T - 37)^2}{2\sigma^2}\right)$$

---

## Parameter Reference

| Symbol | Name | Units | Typical range |
|--------|------|-------|----------------|
| Nt | Observed load at time t | CFU/mL | 10²-10⁸ |
| N0 | Estimated initial load | CFU/mL | Model output |
| t | Delay | h | 0-24 |
| λ | Lag phase | h | 0.3-0.7 |
| k | Decay rate | h⁻¹ | 0.010-0.013 |
| K | Carrying capacity | CFU/mL | 8.5×10⁸-9×10⁸ |
| T | Storage temperature | °C | 4-37 |
| G_base | Base doubling time at 37 °C | h | 0.33-0.38 |
| σ | Temperature sensitivity | °C | 5 (default) |
| dt | Effective growth time | h | Derived |
| r(T) | Temperature-corrected growth rate | h⁻¹ | Derived |

---

## Supported Organism-Matrix Profiles

Default parameters are drawn from organism-matrix-specific literature ranges validated in the published study. Users may override any parameter with locally derived values.

| Organism | Matrix | G_base (h) | λ (h) | k (h⁻¹) | K (CFU/mL) |
|----------|--------|------------|--------|----------|------------|
| *E. coli* | Urine | 0.33 | 0.5 | 0.010 | 9×10⁸ |
| *K. pneumoniae* | Urine | 0.35 | 0.4 | 0.011 | 8.5×10⁸ |
| *S. aureus* | Blood | 0.37 | 0.6 | 0.012 | 9×10⁸ |
| *S. epidermidis* | Blood | 0.38 | 0.7 | 0.013 | 8.5×10⁸ |
| *S. pneumoniae* | CSF | 0.36 | 0.5 | 0.011 | 9×10⁸ |
| *E. coli* | CSF | 0.34 | 0.5 | 0.010 | 9×10⁸ |

---

## Limitations & Assumptions

- **Simplified lag phase**: The model assumes a fixed lag phase λ. In practice, lag duration varies with inoculum size, prior stress, and growth history.
- **Temperature homogeneity**: A single representative temperature T is assumed for the entire delay period. Fluctuating temperatures (e.g., during transport) are not modelled.
- **No output clamping**: The calculator will return mathematically valid results even for biologically implausible inputs. Users should apply clinical judgment.
- **Extrapolation risk**: Parameters are validated for delays up to 24 h. Extrapolation beyond this range is not recommended without independent validation.
- **Research use only**: This tool is intended to support research and hypothesis generation. It is not a validated clinical diagnostic device.

---

## Files

| File | Description |
|------|-------------|
| `index.html` | Self-contained browser calculator (no dependencies) |
| `README.md` | Full documentation |
| `CITATION.cff` | Machine-readable citation file (APA, BibTeX export via GitHub) |
| `LICENSE` | MIT License |

---

## Citation

If you use this calculator in your research, please cite both the journal article and the software:

**Journal Article:**
> Al Mahrizi AD, Mossolem F, Blundell R. Estimating original bacterial loads from delayed clinical samples: A methodological Modeling and empirical validation study. *Journal of Microbiological Methods*. 2026;244:107456. doi:[10.1016/j.mimet.2026.107456](https://doi.org/10.1016/j.mimet.2026.107456)

**Software (this calculator):**
> Al Mahrizi, A. D., Mossolem, F., & Blundell, R. (2026). MM-VCM Calculator: Al Mahrizi-Mossolem Viability Correction Model (v1.0.0). Zenodo. doi:[10.5281/zenodo.19600204](https://doi.org/10.5281/zenodo.19600204)

---

## Authors

- **Ahmed Dawood Al Mahrizi** — [ORCID: 0009-0006-0722-827X](https://orcid.org/0009-0006-0722-827X)
- **Fatima Mossolem** — [ORCID: 0009-0008-1513-6409](https://orcid.org/0009-0008-1513-6409)
- **Renald Blundell** — [ORCID: 0000-0002-1483-0991](https://orcid.org/0000-0002-1483-0991)

---

## Disclaimer

This tool is provided for **research purposes only**. It is not intended for direct clinical decision-making and has not been validated as a medical device. Always apply professional clinical judgment. The authors and affiliated institutions accept no liability for clinical decisions made on the basis of this calculator's outputs.
