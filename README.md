# K-Means Clustering — Penguin Morphology Analysis

## Overview
This mini project applies K-means clustering to the Palmer Penguins dataset to uncover natural groupings in penguin body measurements — without using the `species` label during modeling — and then checks how well the discovered clusters line up with real species and sex.

## Dataset
`penguins.csv` — 344 penguins across 3 species (Adelie, Chinstrap, Gentoo) and 3 islands (Torgersen, Biscoe, Dream), with the following columns:
- `species`, `island`, `sex` (categorical)
- `bill_length_mm`, `bill_depth_mm`, `flipper_length_mm`, `body_mass_g` (numeric)

## Preprocessing
1. Replaced the literal `"NA"` strings in the raw CSV with true `NaN`, then coerced numeric columns to proper numeric dtype.
2. Removed categorical variables (`species`, `island`, `sex`) from the feature matrix used for clustering — K-means only works on numeric distance, and the labels are kept aside for interpretation afterward.
3. Dropped all rows with any missing value (344 → 333 rows).
4. Applied `StandardScaler` to the four numeric features. This is critical for K-means because it uses Euclidean distance — without scaling, `body_mass_g` (values in the thousands) would completely dominate `bill_depth_mm` (values in the tens).

## Choosing k
- **Elbow method:** SSE drops sharply through k=2–3 and flattens out after k=3–4 — the elbow points to k≈3.
- **Silhouette score:** peaks at **k=2 (≈0.53)** and decreases steadily afterward; k=3 scores ≈0.45.
- **Decision:** We selected **k=3**, trading a bit of statistical cleanliness (k=2 is technically tighter) for biological interpretability, since k=3 matches the three known species and still sits at the elbow bend with a solid silhouette score. The k=2 result is a real and interesting finding in its own right — it suggests Gentoo penguins are so morphologically distinct that "Gentoo vs. everyone else" is the single cleanest 2-way split in the data.

## Key Findings
- The k=3 clusters recovered the three penguin species almost cleanly from physical measurements alone. Gentoo (heaviest, longest flippers, shallowest bill) forms the most distinct cluster.
- Adelie and Chinstrap overlap more in flipper length and body mass, but separate more clearly on bill length/depth — visible in the bill-shape scatterplot.
- Within each cluster, males run larger than females (sexual dimorphism), adding spread inside each cluster without breaking the overall species-level grouping.

## Limitations
- K-means assumes roughly spherical, similarly-sized clusters and is sensitive to centroid initialization (mitigated with `n_init=10`).
- Rows with missing values were dropped rather than imputed, slightly reducing sample size.
- `island` was excluded per the project spec, even though it likely correlates with species and could sharpen cluster boundaries.
- The silhouette-optimal k=2 vs. the elbow/biology-driven k=3 is a genuine trade-off, not a clean-cut answer.

## Next Steps
- Try k=2 explicitly to confirm it isolates Gentoo from the other species.
- Try k=6 to see if clusters split further along sex within species.
- Compare against a Gaussian Mixture Model, which can capture elliptical (not just spherical) clusters — likely to handle the Adelie/Chinstrap overlap better.
- Use PCA for an alternate 2D visualization/validation of cluster structure.

## Files
- `kmeans_penguins.ipynb` — full analysis, all cells executed with Elbow plot, Silhouette plot, and cluster scatterplots rendered inline
- `penguins.csv` — source dataset
- `README.md` — this file
