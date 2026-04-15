# MM-VCM Calculator

**Al Mahrizi–Mossolem Viability Correction Model (MM-VCM)** — Interactive browser-based calculator for delay-corrected microbial load estimation.

## Live Calculator

🔗 **[Open Calculator](https://AM3119.github.io/mmvcm-calculator/)**

## Description

This tool estimates the initial microbial load **N₀** (CFU/mL) at the time of sample collection, back-calculated from a delayed measurement **Nₜ**, accounting for:

- Temperature-dependent growth rate (Gaussian around 37°C)
- Lag phase before proliferation begins
- Decay/death rate during storage/transport
- Carrying capacity of the biological matrix

## The Model

$$N_0 = \frac{K \cdot N_t \cdot e^{k \cdot dt}}{N_t \cdot e^{k \cdot dt} + (K - N_t \cdot e^{k \cdot dt}) \cdot e^{r(T) \cdot dt}}$$

where:
- `dt = max(0, t − λ)` — effective growth time after lag
- `r(T) = ln(2)/G_base · exp(−(T−37)²/2σ²)` — temperature-corrected growth rate

## Supported Organisms & Matrices

| Organism | Matrix |
|---|---|
| *E. coli* | Urine, Blood |
| *E. faecalis* | Urine |
| *K. pneumoniae* | Urine |
| *N. meningitidis* | CSF |
| *P. aeruginosa* | Blood |
| *S. aureus* | Blood |
| *S. pneumoniae* | CSF |

## Usage

1. Select organism–matrix profile and click **Apply defaults**
2. Enter observed count Nₜ (CFU/mL), delay (hours), and storage temperature
3. Optionally override λ, k, K, G_base, σ for local validation data
4. Click **Compute N₀**

## Citation

If you use this calculator, please cite the original publication:

> Al Mahrizi & Mossolem. *[Article title]*. *Microbial Pathogenesis*, 2026. DOI: [10.1016/j.micpath.2026.107738](https://linkinghub.elsevier.com/retrieve/pii/S0167701226000680)

## Disclaimer

MM-VCM is intended to assist with delay-corrected interpretation of CFU counts. Use alongside clinical judgment and local laboratory validation. For long delays (≥ 12 h) or uncertain temperatures, interpret N₀ as an interval estimate.
