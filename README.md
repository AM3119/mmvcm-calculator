# MM-VCM Calculator

[![Live Calculator](https://img.shields.io/badge/Calculator-Live-brightgreen?style=flat-square&logo=github)](https://AM3119.github.io/mmvcm-calculator/)
[![DOI](https://img.shields.io/badge/DOI-10.1016%2Fj.mimet.2026.107456-blue?style=flat-square)](https://doi.org/10.1016/j.mimet.2026.107456)
[![Journal](https://img.shields.io/badge/Journal-J.%20Microbiol.%20Methods-orange?style=flat-square)](https://www.sciencedirect.com/journal/journal-of-microbiological-methods)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](LICENSE)

**Al Mahrizi–Mossolem Viability Correction Model (MM-VCM)** — Interactive browser-based calculator for delay-corrected microbial load estimation in clinical microbiology.

---

## 🔗 Live Calculator

**➡️ [https://AM3119.github.io/mmvcm-calculator/](https://AM3119.github.io/mmvcm-calculator/)**

No installation required. Runs entirely in the browser — open the link and use immediately.

---

## Background & Rationale

In clinical microbiology, quantitative culture results (CFU/mL) are routinely used to guide diagnosis and treatment decisions. However, a delay between sample **collection** and **measurement** — due to transport, batching, or laboratory workflow — means the counted load Nₜ reflects the bacterial population *at the time of measurement*, not at collection.

Depending on temperature, organism, and delay duration, the true initial load N₀ can differ from Nₜ by **one to several orders of magnitude**. The MM-VCM model provides a closed-form analytical solution to back-calculate N₀ from Nₜ, correcting for:

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

Growth does not begin immediately — a lag phase λ precedes proliferation:

$$dt = \max(0,\; t - \lambda)$$

### Temperature-Corrected Growth Rate

Growth rate follows a Gaussian model centred at optimal temperature (37 °C):

$$r(T) = \frac{\ln 2}{G_{\text{base}}} \cdot \exp\!\left(-\frac{(T - 37)^2}{2\sigma^2}\right)$$

### Parameter Reference

| Symbol | Name | Units | Typical range |
|--------|------|-------|---------------|
| Nₜ | Observed load at time t | CFU/mL | 10²–10⁹ |
| N₀ | Estimated initial load | CFU/mL | Model output |
| t | Delay | h | 0–24 |
| λ | Lag phase | h | 0.3–0.7 |
| k | Decay rate | h⁻¹ | 0.010–0.013 |
| K | Carrying capacity | CFU/mL | 8.5×10⁸–9×10⁸ |
| T | Storage temperature | °C | 4–37 |
| G_base | Base doubling time at 37 °C | h | 0.33–0.38 |
| σ | Temperature sensitivity | °C | 5 (default) |
| dt | Effective growth time | h | Derived |
| r(T) | Temperature-corrected growth rate | h⁻¹ | Derived |

---

## Supported Organism–Matrix Profiles

Default parameters are drawn from organism–matrix-specific literature ranges validated in the published study. Users may override any parameter with locally derived values.

| Organism | Matrix | G_base (h) | λ (h) | k (h⁻¹) | K (CFU/mL) |
|----------|--------|-----------|-------|---------|------------|
| *E. coli* | Urine | 0.33 | 0.40 | 0.011 | 9 × 10⁸ |
| *E. coli* | Blood | 0.33 | 0.40 | 0.011 | 9 × 10⁸ |
| *E. faecalis* | Urine | 0.35 | 0.50 | 0.011 | 9 × 10⁸ |
| *K. pneumoniae* | Urine | 0.34 | 0.50 | 0.012 | 9 × 10⁸ |
| *N. meningitidis* | CSF | 0.38 | 0.60 | 0.010 | 8.5 × 10⁸ |
| *P. aeruginosa* | Blood | 0.36 | 0.50 | 0.012 | 9 × 10⁸ |
| *S. aureus* | Blood | 0.34 | 0.50 | 0.011 | 9 × 10⁸ |
| *S. pneumoniae* | CSF | 0.38 | 0.60 | 0.010 | 8.5 × 10⁸ |

---

## Usage

1. **Select** the organism–matrix profile and click **Apply defaults**
2. **Enter** the observed count Nₜ (CFU/mL), delay t (hours), and storage temperature T (°C)
3. **Override** λ, k, K, G_base, or σ if local validation data are available
4. **Click Compute N₀** — results include N₀, effective growth time dt, and growth rate r(T)
5. For extended delays (≥ 12 h) or uncertain temperatures, **report N₀ as an interval estimate** in clinical documentation

---

## Limitations & Assumptions

- The model assumes **continuous exponential/logistic growth** during the effective period dt; intermittent temperature fluctuations are not captured.
- **Lag phase λ is treated as fixed** per organism–matrix profile; true lag may vary with inoculum size and prior stress.
- **No clamping** is applied to model output — values < 0 indicate an edge case (e.g., Nₜ > K after decay correction) and should be interpreted as N₀ ≈ 0.
- Sensitivity analyses in the published study indicate **temperature is the dominant uncertainty driver**; a ±2 °C error in T has greater impact than uncertainty in k or λ.
- Model parameters were validated for the eight organism–matrix pairs listed above. **Extrapolation to other organisms or matrices requires local validation.**

---

## Files

| File | Description |
|------|-------------|
| `index.html` | Self-contained calculator (HTML/CSS/JS, no dependencies to install) |
| `CITATION.cff` | Machine-readable citation file (used by GitHub's Cite This Repository feature) |
| `README.md` | This documentation |

---

## Citation

If you use this calculator in your work, please cite the original publication:

> Al Mahrizi AD, Mossolem F, Blundell R. Estimating original bacterial loads from delayed clinical samples: A methodological Modeling and empirical validation study. *Journal of Microbiological Methods*. 2026;244:107456. doi:[10.1016/j.mimet.2026.107456](https://doi.org/10.1016/j.mimet.2026.107456)

You can also use the **"Cite this repository"** button on the right-hand sidebar of this GitHub page (powered by `CITATION.cff`).

---

## Disclaimer

MM-VCM is a research tool intended to assist with delay-corrected interpretation of quantitative culture results. It does not constitute medical advice and must be used alongside clinical judgment and local laboratory validation. The authors accept no liability for clinical decisions made on the basis of this tool alone.
