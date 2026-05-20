### Ensemble Image Anomaly Detection

This Python-based project is designed to detect and analyze anomalies in image data by using a combination of image processing techniques, feature extraction, and multiple anomaly detection algorithms. The following describes each step of the code, including the image processing pipeline, feature extraction, and the anomaly detection methods used.

#### Image Processing and Feature Extraction:
1. **Directory Setup:**
   - The user specifies a directory containing the image files (e.g., `.jpg`, `.png`, `.tiff`). The images should be properly organized in this folder.

2. **Image Preprocessing:**
   - Each image is loaded, cropped to 70% of its original size to focus on the central portion, and converted to grayscale for further analysis.
   - Optionally, simple contrast and brightness adjustments can be applied (default values: alpha=1, beta=0).

3. **Blob Detection:**
   - The program uses `SimpleBlobDetector` to detect and extract regions of interest (blobs) within each image. Blob detection is based on criteria like size, circularity, convexity, and inertia, which define the shape characteristics of each blob.
   - The code extracts blob statistics, such as diameters, areas, and mean grayscale values for the blobs. These features are calculated for each blob and stored for later analysis.

4. **Texture Analysis:**
   - The program computes several texture features for the entire image, including:
     - **Contrast:** Standard deviation of pixel values.
     - **Energy:** Sum of squared pixel intensities.
     - **Homogeneity:** Uniformity of pixel intensities.
     - **Entropy:** Randomness in the image.
     - **Skewness and Kurtosis:** Shape of the pixel intensity distribution.
   - These texture statistics are crucial for representing the visual patterns of the images.

5. **Data Export:**
   - The extracted features (blob statistics and texture analysis results) are collected in a DataFrame and saved as a CSV file for further use, including machine learning models.

#### Anomaly Detection Methods:
The code implements multiple anomaly detection algorithms to flag unusual visual patterns and feature combinations:

1. **Isolation Forest:**
   - This algorithm isolates outliers by randomly selecting features and splitting the data. Anomalous points require fewer splits and are flagged as outliers.
   - It assigns an anomaly score to each observation, and images that are likely outliers based on the score are flagged.

2. **Autoencoder Neural Network:**
   - An autoencoder neural network is used to learn compressed representations of the image features. The reconstruction errors between the original and reconstructed features are used to flag outliers. High reconstruction errors suggest anomalies.

3. **Local Outlier Factor (LOF):**
   - LOF detects outliers by comparing the local density of a data point to that of its neighbors. Points that are significantly less dense are flagged as outliers.

4. **One-Class Support Vector Machine (One-Class SVM):**
   - This algorithm learns a boundary that separates normal data from anomalies. Data points falling outside this boundary are flagged as anomalies.

5. **DBSCAN (Density-Based Spatial Clustering of Applications with Noise):**
   - DBSCAN groups data points into clusters based on density. Points that don't belong to any cluster are treated as outliers. The code tunes the parameters (`eps` and `min_samples`) to optimize the clustering.

#### Ensemble Anomaly Detection:
- After applying all anomaly detection methods, the code combines their results using an ensemble approach:
   - Each detection method flags potential outliers, and the ensemble approach sums how many methods flag each observation as an outlier.
   - If an observation is flagged by a threshold number of methods (e.g., 2 or more), it is considered an ensemble outlier.
   - The ensemble results are saved in a CSV file, providing a comprehensive view of anomalous images across methods.

#### Outputs:
1. **CSV Files:**
   - The results of the feature extraction are saved in `image_analysis_results.csv`.
   - Anomaly results from individual methods and the ensemble are saved in `image_analysis_with_ensemble_outliers.csv`.

#### Requirements:
- Libraries: `cv2`, `numpy`, `os`, `pandas`, `scipy`, `sklearn`, `tensorflow`, `matplotlib`.
- Adjust image directory path as necessary.
