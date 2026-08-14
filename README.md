# User Authentication Using Biometric Features

## 1. Objective

This assignment evaluates the performance of a biometric authentication system using biometric feature vectors.

The dataset contains:
- 100 users
- 10 image samples per user
- 144 features per image
- 1000 total image samples

For every user, the first 5 image samples are used for training/enrollment and the remaining 5 samples are used for testing.

Two matching methods are evaluated:
1. Euclidean Distance
2. Cosine Similarity

The following are calculated:
- Genuine Distribution
- Imposter Distribution
- False Acceptance Rate (FAR)
- False Rejection Rate (FRR)
- Receiver Operating Characteristic (ROC)
- Equal Error Rate (EER)
- Decidability Index

## 2. Dataset Structure

The original data is stored as:

```text
144 rows × 1000 samples
```

Each row represents one biometric feature and each column represents one image sample.

The implementation transposes the data to:

```text
1000 samples × 144 features
```

After transposing:
- Each row represents one biometric image.
- Each column represents one of the 144 biometric features.

The samples are arranged user-wise, with 10 images belonging to each user.

## 3. Training and Testing Split

For every user:

```text
Images 1–5  → Training / Enrollment
Images 6–10 → Testing
```

Therefore:

```text
Training samples = 100 × 5 = 500
Testing samples  = 100 × 5 = 500
```

The five training samples of each user are averaged feature-by-feature to create one representative biometric template.

This produces:

```text
100 user templates × 144 features
```

## 4. User Template Creation

For each user, the five training feature vectors are averaged to create the user's biometric template.

The resulting template contains 144 features.

Thus, there is one template for each of the 100 users.

## 5. Matching Process

Every test image is compared against all 100 user templates.

### Euclidean Distance

Euclidean distance measures the straight-line distance between the test feature vector and a user's template.

A smaller Euclidean distance indicates a better match.

The implementation uses:

```python
np.linalg.norm(template - test_image)
```

### Cosine Similarity

Cosine similarity measures the similarity between two feature vectors based on the angle between them.

A larger cosine similarity indicates a better match.

## 6. Genuine and Imposter Comparisons

There are 500 test images.

For each test image:
- 1 comparison is made with the correct user's template.
- 99 comparisons are made with the other users' templates.

Therefore:

```text
Genuine comparisons
= 100 × 5
= 500
```

and:

```text
Imposter comparisons
= 100 × 5 × 99
= 49,500
```

The genuine and imposter scores are stored separately for both matching methods.

## 7. Genuine Distribution

The genuine distribution contains scores obtained when a test image is compared with the template belonging to the same user.

For Euclidean Distance:

```text
Smaller distance → better match
```

For Cosine Similarity:

```text
Larger similarity → better match
```

Histograms are used to visualize the distributions.

## 8. Imposter Distribution

The imposter distribution contains scores obtained when a test image is compared with templates belonging to other users.

For Euclidean Distance, smaller distances are more likely to result in an incorrect acceptance.

For Cosine Similarity, larger similarities are more likely to result in an incorrect acceptance.

The genuine and imposter distributions are plotted to observe their separation.

## 9. FAR and FRR

### False Acceptance Rate (FAR)

FAR measures how often an imposter is incorrectly accepted.

For Euclidean Distance:

```text
Distance <= Threshold → Accepted
```

For Cosine Similarity:

```text
Similarity >= Threshold → Accepted
```

The proportion of incorrectly accepted imposter comparisons is the FAR.

### False Rejection Rate (FRR)

FRR measures how often a genuine user is incorrectly rejected.

For Euclidean Distance:

```text
Distance > Threshold → Rejected
```

For Cosine Similarity:

```text
Similarity < Threshold → Rejected
```

FAR and FRR are calculated for a range of thresholds and plotted.

## 10. ROC Curve

The Receiver Operating Characteristic (ROC) curve shows the relationship between False Acceptance Rate and True Acceptance Rate.

For Euclidean Distance, smaller distances represent better matches. Therefore, negative Euclidean distance is used as the ROC score so that larger scores represent stronger genuine matches.

For Cosine Similarity, the original similarity score is used because larger values already represent better matches.

The Area Under the ROC Curve (ROC-AUC) is also calculated.

A higher ROC-AUC indicates better discrimination between genuine and imposter comparisons.

## 11. Equal Error Rate (EER)

Equal Error Rate is the point where FAR and FRR are approximately equal:

```text
FAR ≈ FRR
```

A lower EER indicates better biometric authentication performance.

EER is calculated separately for Euclidean Distance and Cosine Similarity.

## 12. Decidability Index

The Decidability Index measures how well the genuine and imposter distributions are separated.

The implementation uses:

```text
d' = |μg - μi| / sqrt((σg² + σi²) / 2)
```

where:
- μg = genuine mean
- μi = imposter mean
- σg = genuine standard deviation
- σi = imposter standard deviation

A higher Decidability Index indicates better separation.

## 13. Results

The implementation produced approximately the following results:

| Metric | Euclidean Distance | Cosine Similarity |
|---|---:|---:|
| ROC-AUC | 0.9496 | 0.9689 |
| EER | 0.1156 | 0.0846 |
| Decidability Index | 1.7619 | 1.9254 |

Exact values can vary slightly depending on numerical details and threshold selection.

## 14. Comparison of Matching Criteria

Cosine Similarity gives:
- Higher ROC-AUC
- Lower EER
- Higher Decidability Index

Therefore, it provides better separation between genuine and imposter comparisons for this dataset.

## 15. Final Conclusion

Based on ROC-AUC, EER, and Decidability Index, Cosine Similarity performs better than Euclidean Distance for the given biometric dataset.

The conclusion is based on:
- Higher ROC-AUC
- Lower EER
- Higher Decidability Index
- Better separation between genuine and imposter scores

Therefore, Cosine Similarity is selected as the better matching criterion for this experiment.

## 16. Implementation Tools

The implementation is done in Python using Jupyter Notebook.

Libraries used:

```text
NumPy
Pandas
Matplotlib
Scikit-learn
```

Their purposes are:
- **NumPy** – numerical calculations and feature-vector operations
- **Pandas** – loading and handling the dataset
- **Matplotlib** – plotting distributions, FAR/FRR, and ROC curves
- **Scikit-learn** – ROC curve and ROC-AUC calculation

No machine learning classification model is used because this assignment focuses on biometric matching and performance evaluation rather than supervised classification.

## 17. Notebook Workflow

```text
Load Dataset
      ↓
Transpose Dataset
      ↓
Arrange 100 Users × 10 Images × 144 Features
      ↓
Split First 5 Images for Training
      ↓
Split Last 5 Images for Testing
      ↓
Create One Template per User
      ↓
Compare Test Images with All Templates
      ↓
Generate Genuine Scores
      ↓
Generate Imposter Scores
      ↓
Calculate FAR and FRR
      ↓
Generate ROC Curve
      ↓
Calculate EER
      ↓
Calculate Decidability Index
      ↓
Compare Euclidean and Cosine
      ↓
Select Better Matching Criterion
```

## 18. Files

```text
Biometric_Performance_Evaluation.ipynb
biomet_data.csv
README.md
```
