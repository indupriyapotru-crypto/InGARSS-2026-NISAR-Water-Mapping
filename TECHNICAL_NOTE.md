InGARSS 2026

Dynamic Surface-Water Mapping & Change Detection Using NISAR Dual-Frequency (L- & S-Band) Radar

1. Study Footprint & NISAR Granules Description

The study area encompasses the highly dynamic alluvial floodplains of the Ganga, Kosi, and Gandak rivers in northern Bihar, India, centered around (25.5000° N, 86.5000° E). In strict accordance with the challenge guidelines, the full unclipped Level-2 GCOV footprint was processed, spanning 57,557.48 km² (8,993,357 valid pixels at 80 m grid spacing).

Sensor Configurations: NISAR Track 98 Ascending Dual-Pol (HH/HV) in both L-band and S-band Temporal Baselines: Ingested July 14, 2026 and October 18, 2026.

2. Geometric Alignment & Memory Calibration

Sub-Pixel Centroid Offset (-Δx/2 = -40 m): HDF5 coordinate arrays define pixel centers, whereas GDAL/Rasterio affine transforms specify the top-left outer corner. Applying a -40 m shift eliminates channel bleeding and guarantees geodetic edge alignment in EPSG:32645 (UTM Zone 45N).

Bilinear Co-Registration: Warped S-band arrays onto master L-band geometry in the linear power domain with bilinear interpolation and explicit NaN NoData handling, ensuring sub-pixel spatial consistency.

3. Single-Frequency Water Classification via Bounded Otsu

Between-class variance was maximized dynamically within a physical radar search interval of [-24.0 dB, -11.5 dB] in July and [-24.0 dB, -12.0 dB] in October, preventing bimodal failure in dry post-monsoon scenes:

July 14 L-Band Classification: Threshold = -11.64 dB; Clean Water Area = 2,288.74 km² (357,615 pixels).

July 14 S-Band Classification: Threshold = -11.64 dB; Water Area = 435.24 km² (68,007 pixels).

Cross-Polarization Filtering: Applied cross-pol cutoffs (L_HV < -21.0 dB; S_HV < -20.0 dB) to reject urban shadow, steep mountain layover, and dry sandbar reflections.

4. Dual-Frequency (L+S) Radar Fusion

Naive union (L OR S) doubles false alarms, while naive intersection (L AND S) eliminates canopy water. Our workflow enforces an asymmetric fusion rule based on the Normalized Difference Frequency Index (NDFI):

Core Perennial Water: Water confirmed by both L-band and S-band (100% confidence).

Sub-Canopy Water Recovery: L-band water retained if NDFI < -0.05 and cross-pol HV < -21.0 dB, recovering hidden floodwaters beneath agricultural canopies.

Fine Edge Recovery: S-band water retained if S_HV < -20.0 dB and L_HH < -10.0 dB, capturing narrow riverbank boundaries.

Quantitative Fused Extent: Total Fused Water = 2,511.81 km² (392,471 pixels). This yields +223.08 km² (+9.75%) of recovered water over L-band alone, and +2,077.06 km² (+477.75%) over S-band alone.

Spatial Agreement: L vs. S IoU = 5.53%, Dice Score = 10.47%. Low sensor agreement mathematically confirms that L and S bands observe complementary physical surface layers.

5. Seasonal Flood Dynamics (July Peak vs. October Recession)

Comparing the dual-frequency fused masks between July 14, 2026 (Monsoon Peak) and October 18, 2026 (Post-Monsoon Recession) establishes the complete multi-temporal dynamic transition matrix:

Persistent Core Water (433.74 km² / 67,772 pixels): Perennial channels of the Ganga, Kosi, and Gandak rivers, plus permanent oxbow lakes (mauns) and wetlands that retain surface water year-round.

Floodwater Recession / Drying (2,016.17 km² / 315,026 pixels): Massive post-monsoon drying: shallow floodplains and low-lying agricultural depressions (chaurs) evaporated, drained, or were cleared for winter rabi crop sowing (net -53.15% flood contraction).

Newly Detected Water (725.95 km² / 113,429 pixels): Post-monsoon braided channel expansion and calm side-channels formed as river discharge dropped.

6. Hydrological Distance Partitioning (1.5 km Cutoff)

To evaluate flood risk, an Euclidean Distance Transform was computed from persistent core river channels. Newly detected October water was partitioned using a 1.5 km (18.75 pixel) threshold:

Near-Channel Lateral Expansion (<= 1.5 km): 34.32 km² (4.7% of new water). Corresponds to lateral river braiding, channel migration, and meander cutoffs along primary river corridors.

Isolated Floodplain Depressions (> 1.5 km): 691.63 km² (95.3% of new water). Corresponds to trapped agricultural standing water and rainwater retention in shallow inland chaurs.

7. Polarimetric Sandbar De-Aliasing (False Alarm Suppression)

Flat quartz sandbars along the braided Kosi reflect radar specularly, producing very low HH backscatter (< -18 dB) identical to calm open water. Naive single-pol Otsu falsely flagged 10,880.51 km² as water. Multi-polarization filtering successfully resolved this ambiguity:

Physical Depolarization Mechanism: Smooth open water produces pure specular reflection with zero cross-polarization (HV ≈ 0, linear NDPI ≈ 1.0). Sand grains induce multiple subsurface scattering, causing measurable cross-polarization.

NDPI Filtering Rule: Linear NDPI = (HH - HV) / (HH + HV) > 0.65 computed strictly in the linear power domain.

Quantitative Suppression: Eliminated 1,219.87 km² (190,604 pixels, 34.8%) of false alarms, retaining 2,288.74 km² of verified, clean surface water.

8. Quantitative Results

| Processing Stage / Parameter | Physical Definition | Pixel Count | Surface Area (km²) |
| --- | --- | --- | --- |
| Valid Scene Footprint | EPSG:32645 UTM Zone 45N Swath | 8,993,357 | 57,557.48 km² |
| Single-Band L-Water (July 14) | Adaptive Otsu Threshold = -11.64 dB | 548,219 | 3,508.60 km² |
| Suppressed Sandbars (NDPI) | Specular quartz sand false alarms | 190,604 | 1,219.87 km² (34.8%) |
| Clean Polarimetric L-Water | Genuine water verified by NDPI | 357,615 | 2,288.74 km² |
| Single-Band S-Water (July 14) | Adaptive Otsu Threshold = -11.64 dB | 68,007 | 435.24 km² |
| Combined Fused Water (July 14) | Asymmetric Linear NDFI Sensor Fusion | 392,471 | 2,511.81 km² |
| Post-Monsoon Water (Oct 18) | Combined Fused Recession Extent | 183,890 | 1,176.90 km² |
| Persistent Core Water | Water present in both July & Oct | 67,772 | 433.74 km² |
| Flood Recession / Drying | Water in July, dry in October | 315,026 | 2,016.17 km² |
| Newly Detected Water (Oct) | Water in October, dry in July | 113,429 | 725.95 km² |
| Near-Channel Expansion | Distance from primary river channel <= 1.5 km | 5,362 | 34.32 km² (4.7%) |
| Isolated Floodplain Chaurs | Distance from primary river channel > 1.5 km | 108,067 | 691.63 km² (95.3%) |
| Stable Non-Water | Unflooded land in both observation dates | 8,309,432 | 53,180.36 km² |
