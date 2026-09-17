# Satellite Water Stress Detection

Experimental analysis of crop water stress using **Sentinel-2 satellite imagery**, vegetation indices, and spatial anomaly detection.

This project explores how changes in vegetation moisture can be detected from satellite imagery using **NDMI (Normalized Difference Moisture Index)** and vegetation masks based on **NDVI** and **SAVI**.

> **Note:** This is an experimental research project. The results are intended to identify potential water stress anomalies and have not been presented as a validated agronomic diagnosis.

## Overview

Water availability is an important factor in crop development. Satellite imagery provides a way to monitor vegetation conditions over large areas and identify changes that may be associated with water stress.

The objective of this project was to investigate whether temporal changes in vegetation moisture, combined with spatial anomaly analysis, could be used to identify areas potentially affected by water stress.

The workflow uses Sentinel-2 imagery to:

1. Retrieve satellite images for a selected area and date range.
2. Select imagery based on cloud coverage.
3. Download and preprocess the required spectral bands.
4. Calculate vegetation and moisture indices.
5. Create vegetation masks using NDVI or SAVI.
6. Compare NDMI between two dates.
7. Detect spatial anomalies in the NDMI change.
8. Classify detected changes into different levels of potential stress.

## Methodology

### 1. Sentinel-2 imagery

The project retrieves **Sentinel-2 Level-2A** imagery.

The following spectral bands are used:

* Red
* Green
* Blue
* Near Infrared (NIR)
* SWIR 1
* SWIR 2

The 20 m bands are resampled to the 10 m reference grid before calculating the indices.

### 2. Vegetation indices

Several indices are explored throughout the research.

**NDVI**

$$
NDVI = \frac{NIR - Red}{NIR + Red}
$$

**SAVI**

$$
SAVI = \frac{(NIR - Red)(1+L)}{NIR + Red + L}
$$

where `L` is the soil brightness correction factor.

**NDMI**

$$
NDMI = \frac{NIR - SWIR}{NIR + SWIR}
$$

NDMI is used as the main indicator for monitoring changes in vegetation moisture.

### 3. Temporal NDMI change

Two Sentinel-2 acquisitions are compared to calculate the relative change in NDMI:

$$
\Delta NDMI =
\frac{NDMI_{t_2} - NDMI_{t_1}}
{|NDMI_{t_1}| + \epsilon}
\times 100
$$

The resulting change is then restricted to areas identified as vegetation.

### 4. Spatial anomaly detection

A local neighborhood median is calculated around each vegetation pixel.

The anomaly is defined as the difference between the pixel's NDMI change and the median change in its surrounding neighborhood.

This allows the method to distinguish between:

* Changes affecting a wider region.
* Localized changes that differ significantly from their surroundings.

### 5. Experimental classification

The final approach uses threshold-based rules to classify vegetation pixels into:

| Class | Description                               |
| ----- | ----------------------------------------- |
| `-1`  | No vegetation                             |
| `0`   | No significant change / stable conditions |
| `1`   | Regional stress anomaly                   |
| `2`   | Localized stress anomaly                  |

These categories represent **potential anomalies detected by the methodology**, rather than confirmed crop water stress.

## Experimental scenarios

The methodology was tested on several different geographic areas and land-cover conditions, including:

* **Seville, Spain** — agricultural areas, urban areas, buildings, and water bodies.
* **Doñana area, Spain** — agricultural fields, river, solar panels, and water.
* **Suzhou, China** — agricultural and urban areas.
* **Southwestern Egypt** — circular agricultural fields surrounded by desert.
* **Greece** — olive trees, mixed vegetation, and exposed soil.

Testing across different environments was used to explore how the methodology behaves under different vegetation, soil, and land-cover conditions.

## Project evolution

The notebooks document the progressive development of the methodology, from the initial experiments to the final approach.

```text
notebooks/
├── 01_initial_approach.ipynb
├── 02_refactored_approach.ipynb
├── 03_threshold_analysis.ipynb
└── 04_ndmi_anomaly_detection.ipynb
```

The notebooks are intentionally kept as part of the repository because they document the **research and experimentation process**, including the changes made between approaches.

## Technologies

* Python
* NumPy
* Rasterio
* Shapely
* SciPy
* Matplotlib
* ipywidgets
* Sentinel-2

## Key aspects explored

The project focuses particularly on:

* Remote sensing
* Satellite imagery processing
* Spectral vegetation indices
* NDMI-based moisture monitoring
* Temporal change detection
* Spatial anomaly detection
* Vegetation masking
* Threshold analysis
* Comparison between NDVI and SAVI
* Testing across different geographical environments

## Limitations

The current methodology is experimental and has several limitations.

* The classification relies on manually selected thresholds.
* NDMI variations can be caused by factors other than water stress.
* Cloud contamination and image quality can affect the results.
* NDVI/SAVI vegetation masks can influence the final classification.
* The approach has not been validated against field measurements or agronomic ground truth.
* Results from different geographical areas may not be directly comparable.
* The detected anomalies should not be interpreted as definitive evidence of irrigation problems or crop water stress.

Further validation with field observations and more systematic calibration would be required before using the methodology operationally.

## Future work

Possible directions for further development include:

* Validation against field measurements.
* Integration of meteorological data.
* Incorporation of additional Sentinel-2 bands and indices.
* Analysis of longer temporal series instead of only two dates.
* Evaluation using larger datasets and more crop types.
* Comparison with established remote-sensing drought and water-stress methodologies.

## Project context

The repository is intended to document the technical approach, experiments, and evolution of the methodology developed during that research.

