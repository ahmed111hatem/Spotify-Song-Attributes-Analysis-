# Spotify Song Attributes — Exploratory Data Analysis

A Jupyter notebook that explores a personal Spotify listening history joined with Spotify Web API audio features. It covers data quality checks, feature distributions, genre and artist breakdowns, and the relationship between listening time and audio characteristics.

- **Notebook:** `Spotify_Song_Attributes_EDA.ipynb`
- **Dataset:** `Spotify_Song_Attributes.csv` (10,080 rows × 22 columns; not included, see [Setup](#setup))

## Questions the analysis answers

1. What are the distributions of core audio features (danceability, energy, valence, tempo)?
2. Which genres dominate the library, and how do their audio profiles differ?
3. Is listening time (`msPlayed`) associated with specific audio characteristics?
4. Which artists and tracks appear most often, and are there data quality issues (missing values, duplicates, outliers)?
5. How strongly are audio features correlated with each other?

## Dataset

Each row is a track with:

| Group | Columns |
|-------|---------|
| Metadata | `trackName`, `artistName`, `genre`, `type`, `id` |
| Listening behavior | `msPlayed`, `duration_ms` |
| Audio features | `danceability`, `energy`, `key`, `loudness`, `mode`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, `time_signature` |
| API links | `uri`, `track_href`, `analysis_url` |

## Notebook structure

| # | Section | What it does |
|---|---------|--------------|
| 1 | Environment & Setup | Imports, plot styling, and a CSV loader that falls back across `utf-8`, `latin-1` and `cp1252` encodings |
| 2 | Data Health Checklist | Head, dtypes, descriptive stats, missing values, duplicates, IQR outlier detection |
| 3 | Feature Engineering | Column groups plus derived `minutes_played`, `duration_min`, `play_ratio`, `key_name`, `mode_name` |
| 4 | Numerical Analysis | Summary stats with skew, feature histograms, box plots, correlation heatmap |
| 5 | Categorical Analysis | Top genres, average audio profile per genre, key/mode distribution, top artists and tracks by minutes played |
| 6 | Listening vs. Audio Features | Energy–valence scatter (colored by danceability), high vs. low play-time comparison (median split), tempo by genre |
| 7 | Executive Summary | Key findings and recommended next steps |

## Key findings

- **Completeness:** 550 rows (5.5%) lack audio features and 1,500 rows (14.9%) lack a genre.
- **Audio profile:** mean danceability ≈ 0.60, energy ≈ 0.56, valence ≈ 0.43, tempo ≈ 119 BPM.
- **Genres:** 523 unique genres, with micro-genres such as `alt z` (656 tracks) leading.
- **Listening time:** `msPlayed` is heavily right-skewed; a small set of artists and tracks dominates total minutes.
- **Correlations:** energy and loudness correlate positively; acousticness correlates negatively with both.
- **Duplicates:** exactly 50% of rows (5,040) are exact duplicates, and 4,794 Spotify IDs repeat. The summary cell in the notebook says "~47%"; the figure computed in the code is 50%. Deduplicate before modeling.

## Setup

**Requirements:** Python 3.9+ and

```
pandas
numpy
matplotlib
seaborn
jupyter
```

```bash
pip install pandas numpy matplotlib seaborn jupyter
jupyter notebook Spotify_Song_Attributes_EDA.ipynb
```

Place `Spotify_Song_Attributes.csv` in the same folder as the notebook. If it lives elsewhere, edit `DATA_PATH` in the setup cell. The loader raises a `FileNotFoundError` with the resolved path if the file is missing.

## Suggested next steps

- **Cleaning:** drop or flag rows without audio features; consolidate the 500+ genres into broader categories.
- **Outliers:** cap or log-transform `msPlayed` before regression; check whether extreme values are real repeat listens or aggregation artifacts.
- **Feature engineering:** mood quadrants (energy × valence), BPM buckets, artist-level aggregates.
- **Modeling:** K-Means or GMM clustering on audio features; gradient boosting to predict high-replay tracks.
- **Validation:** split by artist (or time, if available) to avoid leakage from repeated track–artist rows.
