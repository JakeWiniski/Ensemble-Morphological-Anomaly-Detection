# Ensemble Morphological Anomaly Detection

## Summary

This repository provides a Python workflow for detecting unusual morphology states in image datasets using a combination of classical image processing, morphology-based feature extraction, and multiple unsupervised anomaly-detection algorithms.

The workflow converts images into structured tabular features, including global texture statistics and blob-level morphology measurements, then evaluates each image using several anomaly-detection methods. The results are combined through an ensemble voting approach so that images flagged by multiple methods can be prioritized for review.

This project is intended for applied image-analysis settings where visual differences may be meaningful but difficult to evaluate consistently by manual inspection alone. Potential use cases include materials science, biomaterials research, microscopy analysis, image-based screening, process development, surface inspection, and quality-control workflows.

---

## Novelty and Scope

This repository presents an applied workflow for morphology-based image anomaly detection. It does **not** claim to introduce a new anomaly-detection algorithm, computer-vision method, or deep-learning architecture.

Related concepts already exist across:

- classical image processing
- blob detection
- texture analysis
- feature engineering
- unsupervised anomaly detection
- ensemble anomaly scoring
- visual inspection workflows
- materials image analysis
- quality-control analytics

The contribution of this repository is an **applied, reproducible workflow** that combines grayscale image featurization with multiple unsupervised anomaly-detection methods and an ensemble consensus strategy.

The practical goal is to help answer questions such as:

- Which images have unusual morphology relative to the rest of the dataset?
- Which samples differ in texture, blob structure, intensity distribution, or feature combinations?
- Which images are consistently flagged as unusual across multiple detection methods?
- Can image-derived features support exploratory screening or quality-control review?
- Which samples should be prioritized for human inspection or follow-up analysis?

This repository should be interpreted as:

- a practical anomaly-detection workflow
- a technical prototype
- a morphology-screening tool for image datasets
- a companion workflow to image featurization pipelines
- a foundation for future benchmarking and refinement

It should not currently be interpreted as:

- a validated production inspection system
- a supervised defect classifier
- a causal or mechanistic morphology model
- a replacement for expert review
- a universal anomaly-detection framework
- a claim of novelty over existing anomaly-detection literature

---

## Workflow Overview

The workflow proceeds through four major stages:

1. image loading and preprocessing
2. morphology and texture feature extraction
3. unsupervised anomaly detection using multiple methods
4. ensemble voting across anomaly-detection models

Together, these steps convert image datasets into interpretable feature tables and identify images with unusual morphology-derived profiles.

---

## Image Processing and Feature Extraction

### 1. Directory Setup

The user specifies a directory containing image files. Supported formats may include:

```text
.jpg
.jpeg
.png
.bmp
.tiff
```

Images should be organized in a single folder or adapted to the directory structure expected by the notebook.

### 2. Image Preprocessing

Each image is:

- loaded from the specified directory
- cropped to focus on the central region of the image
- converted to grayscale
- optionally adjusted for contrast and brightness

In the current workflow, images are cropped to 70% of their original size to focus on the central field and reduce edge artifacts. Optional contrast and brightness adjustments can be applied using parameters such as:

```text
alpha = 1
beta = 0
```

These defaults preserve the original contrast and brightness unless adjusted by the user.

### 3. Blob Detection

The workflow uses OpenCV `SimpleBlobDetector` to identify localized regions of interest within each grayscale image.

Blob detection can be controlled through criteria such as:

- size
- circularity
- convexity
- inertia
- intensity thresholding

For each image, the workflow extracts blob-level statistics including:

- number of detected blobs
- blob diameter statistics
- blob area statistics
- mean grayscale value within detected blobs

Blob-level features allow localized morphology to be summarized in a compact, tabular form.

### 4. Texture and Intensity Feature Extraction

The workflow computes global image-level statistics that describe texture, contrast, and intensity distribution.

Examples include:

| Feature | Description |
|---|---|
| `contrast` | Standard deviation of pixel intensity values |
| `energy` | Sum of squared intensity or histogram probabilities; measures uniformity |
| `homogeneity` | Similarity or uniformity among pixel intensities |
| `entropy` | Randomness or complexity in the image |
| `skewness` | Asymmetry of the grayscale intensity distribution |
| `kurtosis` | Tailedness of the grayscale intensity distribution |

These features help represent visual patterns that may not be captured by blob-level measurements alone.

### 5. Data Export

Extracted features are collected into a tabular dataset and saved as a CSV file:

```text
image_analysis_results.csv
```

This file can be used for downstream statistical analysis, visualization, clustering, anomaly detection, or machine learning.

---

## Anomaly Detection Methods

The workflow applies multiple unsupervised anomaly-detection algorithms to the extracted image features. Each method provides a different view of what may count as unusual.

### 1. Isolation Forest

Isolation Forest identifies outliers by recursively partitioning feature space. Observations that can be isolated with fewer splits are more likely to be anomalous.

In this workflow, Isolation Forest flags images with unusual combinations of morphology and texture features.

### 2. Autoencoder Neural Network

An autoencoder learns a compressed representation of the image-feature table and attempts to reconstruct the original feature values.

Images with high reconstruction error are flagged as potential anomalies because their feature patterns are harder for the model to represent.

### 3. Local Outlier Factor

Local Outlier Factor compares the local density around each observation to the density around its neighbors.

Images located in relatively sparse regions of feature space are flagged as potential outliers.

### 4. One-Class Support Vector Machine

One-Class SVM learns a boundary around the main body of the data.

Images falling outside this learned boundary are flagged as potential anomalies.

### 5. DBSCAN

DBSCAN identifies dense clusters of observations and treats points outside clusters as noise.

Images that do not belong to a dense feature-space cluster may be flagged as anomalous.

---

## Ensemble Anomaly Detection

Each anomaly-detection method captures a different kind of unusualness. Some methods are sensitive to global separation, others to local density, reconstruction difficulty, or cluster membership.

The ensemble approach combines these perspectives by counting how many methods flag each image as anomalous.

For each image, the workflow records:

- whether each individual method flagged the image
- the total number of anomaly flags
- whether the image meets the ensemble threshold

For example, an image may be considered an ensemble anomaly if it is flagged by two or more methods.

The ensemble results are saved in:

```text
image_analysis_with_ensemble_outliers.csv
```

This consensus-style approach helps prioritize images that are unusual across multiple analytical definitions, rather than relying on a single model.

---

## Practical Use Cases

This workflow is most useful when:

- images are grayscale or can be meaningfully converted to grayscale
- image differences are visible but difficult to score manually
- anomalous samples may arise from morphology, texture, defects, or processing variation
- labeled anomaly data are unavailable
- the goal is exploratory screening rather than supervised classification
- human review capacity is limited and samples need to be prioritized

Potential applications include:

- materials characterization
- biomaterials analysis
- mycelium morphology screening
- microscopy image screening
- surface inspection
- process-development diagnostics
- defect triage
- quality-control review
- image-based experimental screening
- anomaly detection in engineered morphology datasets

---

## Outputs

The workflow produces two main output files.

| File | Description |
|---|---|
| `image_analysis_results.csv` | Extracted image features, including blob statistics and texture features |
| `image_analysis_with_ensemble_outliers.csv` | Feature table with anomaly flags from individual methods and ensemble results |

These outputs can be used to:

- identify unusual images
- prioritize images for manual review
- compare anomaly methods
- visualize feature-space structure
- support clustering or downstream modeling
- document image-screening decisions

---

## Requirements

The workflow uses Python 3.x.

Required libraries include:

- `opencv-python`
- `numpy`
- `pandas`
- `scipy`
- `scikit-learn`
- `tensorflow`
- `matplotlib`

Install dependencies using pip:

```bash
pip install opencv-python numpy pandas scipy scikit-learn tensorflow matplotlib
```

Depending on the environment, TensorFlow installation may require additional configuration.

---

## Repository Contents

Typical repository contents include:

| File | Description |
|---|---|
| Notebook or Python workflow | Main image featurization and anomaly-detection workflow |
| `README.md` | Project documentation |
| Output CSV files | Generated feature and anomaly-detection results, if included |

If publishing this repository as an open technical workflow, consider adding:

- a clear example dataset or synthetic image set
- a `requirements.txt` file
- a `LICENSE` file
- a `CITATION.cff` file
- example output visualizations

---

## Interpretation

The ensemble anomaly score should be interpreted as a **screening signal**, not a definitive defect label.

An image flagged by multiple methods is not automatically “bad” or “failed.” It is an image whose feature profile differs from the rest of the dataset according to several unsupervised criteria. Domain knowledge and image review are still required to determine whether the anomaly is meaningful, artifactual, or irrelevant.

The workflow is therefore best used to support questions such as:

- Which images should be reviewed first?
- Are there unusual morphology states in this dataset?
- Do anomaly flags correspond to visible defects or process differences?
- Are some experimental conditions producing more unusual morphology?
- Are outliers driven by blob structure, texture, intensity, or preprocessing artifacts?

---

## Current Limitations

Several limitations should be considered.

1. **The workflow is unsupervised.**  
   It does not learn from confirmed labels of normal or abnormal images.

2. **Anomalies are relative to the dataset.**  
   A sample is flagged because it differs from the other images provided, not because it violates an absolute standard.

3. **Feature quality determines detection quality.**  
   The anomaly models can only detect differences represented in the extracted features.

4. **Image acquisition conditions matter.**  
   Lighting, focus, magnification, resolution, exposure, compression, and background can strongly affect feature values.

5. **Blob detection is parameter-dependent.**  
   Different parameter choices may change which structures are detected and how they are summarized.

6. **The ensemble threshold requires judgment.**  
   A lower threshold increases sensitivity but may flag more false positives. A higher threshold is more conservative but may miss subtle anomalies.

7. **Autoencoder performance depends on dataset size and model configuration.**  
   Small datasets may not support stable neural-network training without careful tuning.

8. **This is not a substitute for validation.**  
   For production or regulated use, anomaly flags would need to be validated against expert labels, process outcomes, or independent measurements.

---

## Future Development

Possible next steps include:

- adding a synthetic or public example image dataset
- adding configurable preprocessing and blob-detection parameters
- adding visualization of detected blobs and anomaly flags
- adding feature-space plots such as PCA, UMAP, or t-SNE
- adding method-by-method anomaly score summaries
- supporting image groups or experimental conditions
- adding batch reports for flagged images
- adding supervised validation when labels are available
- packaging the workflow into reusable Python functions
- benchmarking ensemble voting against individual methods

---

## Bottom Line

Ensemble Morphological Anomaly Detection provides a practical workflow for identifying unusual images based on morphology-derived features.

The workflow is designed for applied researchers, scientists, and engineers who need to screen image datasets for unusual texture, blob structure, intensity patterns, or morphology states. Its strength is not theoretical novelty, but the practical integration of image featurization and multiple unsupervised anomaly-detection methods into a transparent screening pipeline.

---

## License

This project is released under the **Apache License 2.0**.

The intent is to support open exploration, reproducibility, and extension of morphology-based anomaly-detection workflows while preserving a permissive license structure suitable for research, education, and applied development.
