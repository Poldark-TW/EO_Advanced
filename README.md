# EO_Advanced
**Workflow Summary**
***Version 1 – Baseline Classification***

1.Define study area (ROI) and time period.

2.Load Sentinel-2 (optical) and Sentinel-1 (radar) imagery.

3.Compute indices: NDVI (vegetation), NDWI (water), NDSI (snow/ice).

4.Derive VV/VH ratio from Sentinel-1.

5.Build image stack: Sentinel-2 bands + Sentinel-1 (VV, VH) + ratio + indices.

6.Collect and merge training samples (water, snow, rock, grass, forest).

7.Split into training/testing datasets.

8.Train a Random Forest classifier.

9.Evaluate model: training, testing, and cross-validation accuracy.

10.Classify full image and export results.

***Version 2 – Enhanced Classification***

1.Same ROI and time period setup as Version 1.

2.Load Sentinel-2 (optical) and Sentinel-1 (radar) imagery.

3.Derive VV/VH ratio.

4.Add DEM (elevation) as topographic input.

5.Compute SAR texture features (GLCM: contrast, correlation).

6.Build extended image stack: Sentinel-2 + Sentinel-1 + ratio + DEM + textures.

7.Collect and merge training samples.

8.Split into training/testing datasets.

9.Train a Random Forest classifier with enhanced feature space.

Evaluate with training, testing, and k-fold cross-validation.

Classify full image and export results.
