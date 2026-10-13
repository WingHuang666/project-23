# project-23

# Music Success Analysis

## Project Overview

This project explores how musical audio characteristics and track
metadata relate to commercial success. Three track-level datasets are
integrated to examine chart longevity, viral-hit classification, peak
chart position, streaming volume, and audio-based song profiles.

The analysis combines exploratory data analysis (EDA), supervised
machine learning, permutation feature importance, and K-Means clustering
in a reproducible Python/Quarto workflow.

## Research Questions

1.  **Chart Longevity:** Can audio and release metadata predict a
    track's `longevity_score`?
2.  **Viral Hits:** Can the same features distinguish viral hits from
    non-viral tracks?
3.  **Peak Chart Rank:** Can track characteristics predict
    `peak_chart_rank`?
4.  **Streaming Volume:** Can audio and metadata features predict
    `total_streams_mil`?
5.  **Audio Profiles:** Can K-Means identify distinct groups of songs
    based on audio characteristics, and how do their commercial outcomes
    compare?

## Datasets

The project uses three CSV files, joined by the unique `track_id`
identifier:

  -----------------------------------------------------------------------
  File                                Role
  ----------------------------------- -----------------------------------
  `track_metadata.csv`                Track metadata, genre, release
                                      information, duration, and
                                      explicit-content indicator

  `audio_features.csv`                Musical characteristics such as
                                      tempo, danceability, energy,
                                      valence, acousticness, and liveness

  `chart_longevity_panel.csv`         Commercial outcomes, including
                                      chart rank, chart longevity,
                                      streams, and viral-hit labels
  -----------------------------------------------------------------------

The workflow checks identifier uniqueness, reports unmatched IDs, and
performs a validated one-to-one inner merge. The resulting analysis
dataset contains **1,200 tracks**.

## Project Structure

``` text
music-success-analysis/
├── music_success_project.qmd
├── README.md
├── track_metadata.csv
├── audio_features.csv
└── chart_longevity_panel.csv
```

The Quarto notebook includes data integration, EDA, model comparisons,
supplementary feature importance analyses, clustering, and a
cross-question summary. The supplementary Ridge analysis chunks and the
updated Section 11 should be kept in the working `.qmd` file.

## Tools and Libraries

-   **Language:** Python
-   **Report format:** Quarto (`.qmd`) with Jupyter
-   **Data manipulation:** pandas, NumPy
-   **Visualization:** Matplotlib, Seaborn
-   **Machine learning:** scikit-learn
-   **Development environment:** VS Code or a compatible Jupyter/Quarto
    environment

## Methodology

### Data Preparation and EDA

The project integrates the three datasets, checks missing values, and
derives release year, release month, and track duration in minutes. EDA
examines target distributions, viral-hit class balance, streaming
skewness, genre differences, and relationships among audio
characteristics and commercial outcomes.

### Supervised Learning (RQ1--RQ4)

Each supervised task compares five candidate models, including a simple
baseline:

  -----------------------------------------------------------------------
  Task                    Candidate models        Main evaluation
  ----------------------- ----------------------- -----------------------
  Regression (RQ1, RQ3,   Dummy Median, Ridge,    Cross-validation MAE;
  RQ4)                    Random Forest, Extra    RMSE as secondary
                          Trees,                  
                          HistGradientBoosting    

  Binary classification   Dummy Prior, Logistic   PR-AUC and ROC-AUC;
  (RQ2)                   Regression, Random      threshold-based metrics
                          Forest, Extra Trees,    
                          HistGradientBoosting    
  -----------------------------------------------------------------------


### Unsupervised Learning (RQ5)

K-Means clustering uses six standardized audio features:

-   Tempo
-   Danceability
-   Energy
-   Valence
-   Acousticness
-   Liveness

Candidate solutions with **k = 2--10** are evaluated using inertia
(Elbow Method), silhouette score, and minimum cluster size. The
selection heuristic prefers solutions with no cluster smaller than 2% of
tracks and a silhouette score within 0.02 of the best eligible
candidate, then chooses the candidate closest to the estimated elbow.

Commercial outcomes are compared **after** clustering and do not
determine cluster membership.

