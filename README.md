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

- 1,802 songs
- song identifier plus valence/arousal means and standard deviations
- 520 predictor columns derived from 260 base openSMILE descriptors
- mean and standard-deviation temporal aggregation for every base descriptor

The companion notebook performs its train/test split and all preprocessing
after loading this table; the release does not contain fitted model artifacts.

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
