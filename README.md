# DEAM Song-Level Acoustic Features

Companion data release for the
[music emotion regression](https://github.com/asher0913/music-emotion-regression)
project. The release asset contains one row per song with valence/arousal
annotations and aggregated openSMILE acoustic descriptors.

## Download

The 18.7 MB CSV is stored in [GitHub Release 1](https://github.com/asher0913/deam-song-level-features/releases/tag/1):

```bash
curl -L \
  https://github.com/asher0913/deam-song-level-features/releases/download/1/deam_song_level_features.csv \
  -o deam_song_level_features.csv
```

## Contents

| | |
|---|---|
| Rows | 1,802 songs (DEAM song ids 2–2058), no missing values |
| Targets | `valence_mean`, `arousal_mean`: mean rating on a 1–9 scale; `valence_std`, `arousal_std`: disagreement between annotators |
| Predictors | 520 columns = 65 openSMILE low-level descriptors × value or first difference × window mean or std × song-level mean or std |
| File | `deam_song_level_features.csv`, 18.7 MB, SHA-256 `f09044497e415fe22b999051cdee59d479db2486f73f896603ae4f7fe73c0ff6` |

The 65 low-level descriptors are:

- 26 auditory-spectrum bands (`audSpec_Rfilt[0..25]`) and 14 MFCCs;
- loudness (`audspec_lengthL1norm`, `audspecRasta_lengthL1norm`), RMS energy and zero-crossing
  rate;
- pitch and voicing (`F0final`, `voicingFinalUnclipped`), jitter, shimmer and harmonics-to-noise
  ratio;
- band energies, spectral roll-off at 25/50/75/90%, flux, centroid, entropy, variance, skewness,
  kurtosis, slope, sharpness and harmonicity.

A column name reads from the inside out:

```text
pcm_fftMag_spectralFlux_sma_de_stddev__mean_over_time
└──── low-level descriptor ───┘ │   │        └ aggregated over the song: mean (or std) of the windows
                                │   └ within each 0.5 s window: std (or amean = mean)
                                └ "_de": first difference (delta); absent for the raw value
```

`columns.csv` lists every column with those four parts split out, so feature groups can be
selected without parsing names.

| Target | Mean | Std | Min | Max | Mean annotator std |
|---|---:|---:|---:|---:|---:|
| Valence | 4.90 | 1.17 | 1.6 | 8.4 | 1.50 |
| Arousal | 4.81 | 1.28 | 1.6 | 8.1 | 1.46 |

```python
import pandas as pd

df = pd.read_csv("deam_song_level_features.csv")
columns = pd.read_csv("columns.csv")
loudness = columns.query("low_level_descriptor.str.contains('lengthL1norm')", engine="python").column
X, y = df[loudness], df["arousal_mean"]
```

The companion notebook performs its train/test split and all preprocessing after loading this
table. The release contains no fitted models. On a fixed 20% test split, gradient-boosted trees
reach R² 0.60 for arousal and 0.43 for valence
([results](https://github.com/asher0913/music-emotion-regression#results)).

## Source, terms, and citation

This is a derived table based on the
[Database for Emotional Analysis in Music (DEAM)](https://cvml.unige.ch/databases/DEAM/).
DEAM's manual specifies Creative Commons Attribution–NonCommercial terms for
dataset use. This repository does not redistribute audio.

If you use the data, follow the source terms and cite:

> Aljanaki, A., Yang, Y.-H., & Soleymani, M. (2017). Developing a benchmark for
> emotional analysis of music. *PLOS ONE*, 12(3), e0173392.

The feature representation is based on openSMILE; also cite Eyben et al.,
“Recent developments in openSMILE,” ACM Multimedia 2013.
