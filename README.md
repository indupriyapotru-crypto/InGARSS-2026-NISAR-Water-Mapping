# InGARSS 2026: Dual-Frequency NISAR Surface Water Mapping & Flood Dynamics

An operational dual-frequency (L-band & S-band) polarimetric SAR framework for surface water extraction, sandbar de-aliasing, and flood dynamics tracking across the Lower Ganga & Kosi Floodplain (57,557.48 km² swath).

---

## 🔗 Quick Deliverable Links
1. **[Jupyter Notebook / Code](InGARSS_2026_NISAR_Water_Mapping.ipynb)**: Complete dual-frequency geospatial pipeline and scientific workflows.
2. **[Technical Note (Markdown)](TECHNICAL_NOTE.md)** / **[Technical Note (DOCX)](TECHNICAL_NOTE.docx)**: Full scientific report, mathematical formulation, and numerical scorecard.
3. **[Presentation Slides (PPTX)](InGARSS_2026_NISAR_Water_Mapping.pptx)**: Visual executive summary, methodology diagrams, and high-resolution maps.
4. **[Output Visualizations / Figures](outputs/plots/)**:
   - [Dual-Frequency Fusion Map](outputs/plots/Dual_Frequency_Fusion.png)
   - [Multi-Temporal Dynamics Map](outputs/plots/Multi_Temporal_Dynamics.png)
   - [Hydrological & Polarimetric Validation](outputs/plots/Hydrological_and_Polarimetric_Validation.png)

---

## 🛰️ Methodology Summary
- **Linear-Domain Sensor Harmonization**: S-band imagery (28.4 m) resampled to L-band grid (80.0 m) strictly in linear power domain (`P = 10^(dB/10)`) before re-conversion to dB, avoiding Jensen's inequality bias.
- **Sandbar De-aliasing via NDPI**: Normalized Difference Polarization Index (`NDPI = (gamma0_HH - gamma0_HV) / (gamma0_HH + gamma0_HV)`) deployed with threshold `NDPI > 0.40` to suppress specular bare sands that alias as open water.
- **Linear NDFI Dual-Frequency Fusion**: Normalized Difference Frequency Index evaluated on linear backscatter power to capture canopy-penetrating L-band signals and rough water S-band scattering, recovering 223.07 km² (+9.75%) of missed inundation.
- **Hydrological Distance Partitioning**: Continuous river buffering (`distance_transform_edt`) distinguishes near-channel lateral overbank spills (<= 1.5 km) from inland floodplain depression chaurs (> 1.5 km).

---

## 📊 Quantitative Scorecard (57,557.48 km² Swath)
| Hydrological Metric | Area (km²) | Pixels | % of Swath / Class |
| :--- | :--- | :--- | :--- |
| **Total Valid Scene Swath** | **57,557.48** | **8,993,357** | 100.00% |
| Raw July L-Water (Pre-NDPI) | 3,508.60 | 548,219 | 6.10% |
| **Suppressed Sandbars / Non-Water** | **1,219.87** | **190,604** | **34.8% of Raw Water** |
| Clean July L-Water (Post-NDPI) | 2,288.74 | 357,615 | 3.98% |
| Clean July S-Water | 435.24 | 68,007 | 0.76% |
| **Fused July Water (L + S)** | **2,511.81** | **392,471** | **4.36% (+9.75% over L)** |
| **Fused October Water (Post-Monsoon)** | **1,176.90** | **183,890** | **2.04% (-53.15% contraction)** |
| **Persistent River Channels** | **433.74** | **67,772** | **0.75%** |
| **Flood Recession / Drying** | **2,016.17** | **315,026** | **3.50%** |
| **Newly Detected Water** | **725.95** | **113,429** | **1.26%** |
| ↳ Near-Channel Lateral Overbank (<= 1.5 km) | 34.32 | 5,362 | 4.7% of New Water |
| ↳ Inland Floodplain Chaurs (> 1.5 km) | 691.63 | 108,067 | 95.3% of New Water |
| **Stable Non-Water Land** | **53,180.36** | **8,309,432** | **92.40%** |

---

## 👥 Authors
- **InGARSS 2026 Hackathon Team**
