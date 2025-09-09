# EO_Advanced
**Workflow Summary**

***Version 1 – Baseline Classification Workflow***

****Stage 1: Data Collection (Sentinel-1 & Sentinel-2)****

It relies on Sentinel-2 optical imagery and Sentinel-1 SAR radar data as the primary data sources, selecting representative seasonal periods.

*****Critical points:*****
 - Sentinel-2 provides multispectral information to identify vegetation, water, snow, and ice.
 - Sentinel-1 is unaffected by clouds, complementing optical imagery in mountainous regions where cloud cover and shadows are common.
 - Combining both datasets addresses challenges from “optical cloud contamination” and “SAR terrain/roughness effects.”

****Stage 2: Preprocessing & Feature Extraction****

Basic preprocessing was applied, including:
 - Optical imagery: cloud masking and shadow removal.
 - SAR imagery: terrain correction and speckle noise filtering.
 - Spectral indices: NDVI (vegetation), NDWI (water), and NDSI (snow/ice).

*****Critical points:*****
 - Preprocessing enhances data quality and comparability.
 - Spectral indices highlight land cover features more effectively, especially in heterogeneous alpine environments.

****Stage 3: Feature Stack Construction****

All selected features were combined into a single multi-source feature stack, including:
 - Sentinel-2 bands.
 - Sentinel-1 VV/VH backscatter.
 - VV/VH ratio.
 - Spectral indices (NDVI, NDWI, NDSI).

*****Critical points:*****
 - Integrating optical, SAR, and spectral features maximizes complementarity between data sources.
 - A richer feature space improves the ability to separate complex alpine land covers (forest, grass, rock, snow/ice, water).

****Stage 4: Training & Classification (Random Forest)****

Training and testing datasets were created from manually labeled points, and a Random Forest (RF) classifier was applied for supervised classification.

*****Critical points:*****
 - RF is an ensemble learning algorithm that handles high-dimensional and heterogeneous data effectively.
 - It is robust with small training samples and nonlinear relationships, reducing the risk of overfitting.
 - By using random feature and sample selection to build decision trees, RF improves generalization and avoids dependence on single variables.

****Stage 5: Preprocessing & Feature Extraction****

Classification results were validated and assessed using:
 - Confusion Matrix: to evaluate misclassification.
 - Overall Accuracy (OA): to measure classification performance.
 - k-fold Cross Validation: to test model stability.
 - Export of final land cover classification maps.

*****Critical points:*****
 - Accuracy assessment ensures reliability of the model results.
 - Outputs provide robust land cover information for environmental monitoring and change detection.
 - K-fold cross-validation was chosen because it maximizes the use of our limited alpine training samples, provides a more stable and unbiased estimate of classification accuracy than a single split, and helps detect overfitting while ensuring all land cover classes are properly represented in evaluation.



***Version 2 – Baseline Classification Workflow***

****Stage 1: Preprocessing & Feature Extraction****

Uses the same Sentinel-1 and Sentinel-2 datasets as Version 1, but additionally integrates DEM (SRTM elevation data).

*****Critical points:*****
 - Elevation is a critical factor in alpine regions, where snowline, vegetation zones, and forest boundaries are strongly elevation-dependent.
 - Combining DEM with optical and SAR data enhances class separability.

****Stage 2: Preprocessing & Feature Extraction****

Similar preprocessing as Version 1, but extended to include texture analysis on SAR data.
 - Optical: cloud/shadow filtering.
 - SAR: terrain correction, speckle noise reduction.
 - Texture features: derived from SAR VV band using GLCM (Gray-Level Co-occurrence Matrix), e.g., contrast, correlation.

*****Critical points:*****
 - Texture adds spatial context beyond intensity values, crucial for distinguishing forest vs grassland and rock vs snow/ice.

****Stage 3: Feature Stack Construction****

The feature stack in Version 2 is more comprehensive:
 - Sentinel-2 spectral bands.
 - Sentinel-1 VV/VH + VV/VH ratio.
 - DEM (elevation).
 - SAR texture features (GLCM contrast, correlation).

*****Critical points:*****
 - Expanding the feature space captures both spectral and structural/terrain information.
 - Provides the classifier with richer context to reduce spectral overlap in mountainous terrain.

****Stage 4: Training & Classification (Random Forest)****

Same supervised classification setup with labeled training/testing data, but the RF classifier is trained on the expanded feature stack.

*****Critical points:*****
 - Random Forest is well-suited for handling a large number of diverse input variables.
 - With DEM and texture features included, RF builds more complex decision rules to capture subtle differences.
 - This improves training accuracy but risks overfitting when the feature space becomes too large.

****Stage 5: Validation & Output****

Accuracy was evaluated using the same three approaches:
 - Confusion Matrix.
 - Overall Accuracy.
 - k-fold Cross Validation (3-fold).

*****Critical points:*****
 - Provides consistency with Version 1 for fair comparison.
 - Results show much higher training accuracy (~0.93–0.97), but lower test accuracy (~0.62 winter, ~0.96 summer), reflecting possible seasonal dependence and overfitting.
 - Final classification maps include refined spatial detail due to texture and terrain.
 - K-fold cross-validation was chosen because it maximizes the use of our limited alpine training samples, provides a more stable and unbiased estimate of classification accuracy than a single split, and helps detect overfitting while ensuring all land cover classes are properly represented in evaluation.



****Comparative Summary: Version 1 vs Version 2****

<img width="556" height="682" alt="image" src="https://github.com/user-attachments/assets/d4ff2391-3b7a-47a8-bcaf-e1a05bc0308d" />






























****Methods****

*****Version 1:*****

Features: Sentinel-2 bands + indices (NDVI, NDWI, NDSI) + Sentinel-1 backscatter.

Classifier: Random Forest (robust, handles non-linearity).

Outcome: Balanced, generalizable, but struggles with subtle class boundaries.

*****Version 2:*****

Features: Version 1 + DEM + Sentinel-1 texture (GLCM).

Classifier: Random Forest and SVM.

Outcome: Higher training accuracy, better differentiation of complex classes, but risk of overfitting and noise.


*****🔹 Why Random Forest / SVM?*****

Random Forest: Handles non-linear data, robust to noise, works well with mixed features (optical, SAR, DEM).

SVM : Effective for small training sets, strong at handling overlapping spectral signatures.


*****🔹 How does k-fold CV help with accuracy and class recognition?*****

Accuracy:

Provides a more trustworthy estimate of how well the classifier will generalize to unseen imagery.

Reduces the chance of reporting overly optimistic accuracy from a “lucky” train/test split.

Class recognition:

Ensures each class (snow, rock, grass, forest, water) gets represented in both training and validation folds.

Improves the model’s ability to recognize minority classes that might otherwise be underrepresented.


*****🔹Why is Version 2 not necessarily better despite higher training accuracy?*****

Version 2 shows signs of overfitting because it uses many features (DEM + texture + spectral). While training accuracy is high, the testing accuracy drops, meaning it learns noise instead of general patterns. This makes Version 1 more generalizable in some cases.

*****🔹Why is cloud interference less of a problem in your methodology?*****
It combined Sentinel-1 radar (not affected by clouds) with Sentinel-2 optical imagery. Additionally, using indices like NDVI and NDSI reduces confusion between clouds and snow. The integration of radar and DEM features further improves discrimination.

*****🔹Why did the project utilize VV + VH polarizations from Sentinel-1?*****

Complementary information:

VV (Vertical transmit, Vertical receive): Sensitive to surface roughness and geometric structure (e.g., rock, bare ground, calm water).

VH (Vertical transmit, Horizontal receive): Sensitive to volume scattering (e.g., vegetation, snowpack, forest canopy).

Improved class separation: Using both captures surface + volume scattering differences, which helps distinguish water vs. vegetation, snow vs. rock, and forest vs. grassland.

Noise reduction: VV alone may misclassify forests as rock, while VH adds extra backscatter variability, improving classification robustness.







