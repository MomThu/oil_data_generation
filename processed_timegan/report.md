# Report on Synthetic Augmentation Experiments

## Scope

This report summarizes the results already available in `processed_timegan/` for the oil classification benchmark.

Experiment setup:

- Seed-per-class grid: 3 to 20
- Test split: fixed 20%
- Models: 1D-CNN, GRU, LSTM, CNN-GRU, RandomForest
- Compared methods: Seed only, five classical augmentation methods, TimeGAN, and Conditional TimeGAN

## Main Findings

1. The best overall synthetic augmentation comes from the five-method benchmark in `five_method_seed_3_20_results.csv`.
2. For the neural models, synthetic generation usually lifts accuracy substantially compared with the seed-only baseline, both in absolute and relative terms.
3. RandomForest is already strong before generation, so its average result does not always improve, even though the best single generated setting still reaches the highest absolute accuracy overall.
4. TimeGAN and Conditional TimeGAN are included below for comparison; they are consistently weaker than the classical augmentation methods on the neural models, while RandomForest remains comparatively stable.

## Before vs After Generation by Model

The table below compares the average seed-only accuracy with the best average synthetic result across the five classical augmentation methods.

| Model | Avg seed-only | Best avg synthetic | Best method | Abs. improvement | Rel. improvement |
| --- | ---: | ---: | --- | ---: | ---: |
| 1D-CNN | 0.6324 | 0.8534 | Time Shift + Jitter | +0.2210 | +50.63% |
| GRU | 0.1103 | 0.3862 | Permutation + Jitter | +0.2759 | +268.71% |
| LSTM | 0.1094 | 0.2137 | Time Warping + Jitter | +0.1042 | +114.43% |
| CNN-GRU | 0.1452 | 0.5973 | Mixup + Jitter | +0.4522 | +359.41% |
| RandomForest | 0.9198 | 0.9089 | Magnitude Scaling + Jitter | -0.0109 | -1.17% |

Interpretation:

- The neural models gain the most from generation, especially CNN-GRU and GRU.
- RandomForest is the exception: its average accuracy is already high in the seed-only setting, so augmentation mainly changes the variance rather than the mean.
- If the goal is the best single run, RandomForest still benefits slightly from generation because its peak generated score is higher than its peak seed-only score.

## Best Accuracy by Model

| Model | Best seed-only | Best five-method synthetic | Best TimeGAN | Best Conditional TimeGAN |
| --- | ---: | ---: | ---: | ---: |
| 1D-CNN | 0.8609 | 0.9173 | 0.2895 | 0.2030 |
| GRU | 0.1617 | 0.2932 | 0.0865 | 0.1015 |
| LSTM | 0.1579 | 0.2218 | 0.0526 | 0.0526 |
| CNN-GRU | 0.3083 | 0.5301 | 0.1429 | 0.0752 |
| RandomForest | 0.9774 | 0.9850 | 0.9699 | 0.9511 |

## Best Five-Method Rows

| Model | Best method | Seed per class | Accuracy | Improvement vs seed-only best |
| --- | --- | ---: | ---: | ---: |
| 1D-CNN | Time Warping + Jitter | 20 | 0.9173 | +0.0564 |
| GRU | Time Shift + Jitter | 15 | 0.2932 | +0.1316 |
| LSTM | Magnitude Scaling + Jitter | 19 | 0.2218 | +0.0639 |
| CNN-GRU | Time Shift + Jitter | 20 | 0.5301 | +0.2218 |
| RandomForest | Mixup + Jitter | 19 | 0.9850 | +0.0075 |

## Before vs After Generation by Method

The following table compares the mean seed-only accuracy with the mean synthetic accuracy, aggregated over all seed-per-class settings and all models.

| Method | Avg seed-only | Avg after gen | Abs. improvement | Rel. improvement |
| --- | ---: | ---: | ---: | ---: |
| Time Shift + Jitter | 0.3834 | 0.5767 | +0.1933 | +146.54% |
| Mixup + Jitter | 0.3834 | 0.5762 | +0.1928 | +144.79% |
| Permutation + Jitter | 0.3834 | 0.5746 | +0.1912 | +147.14% |
| Magnitude Scaling + Jitter | 0.3834 | 0.5589 | +0.1755 | +137.20% |
| Time Warping + Jitter | 0.3834 | 0.5416 | +0.1582 | +128.68% |

## Before vs After Generation by TimeGAN Variants

The tables below compare seed-only against the two TimeGAN-based methods using the same seed-per-class settings.

### TimeGAN

| Model | Avg seed-only | Avg after gen | Abs. improvement | Rel. improvement |
| --- | ---: | ---: | ---: | ---: |
| 1D-CNN | 0.6324 | 0.1424 | -0.4900 | -74.84% |
| GRU | 0.1103 | 0.0545 | -0.0558 | -47.65% |
| LSTM | 0.1094 | 0.0526 | -0.0568 | -43.08% |
| CNN-GRU | 0.1452 | 0.0564 | -0.0888 | -54.87% |
| RandomForest | 0.9198 | 0.9158 | -0.0040 | -0.34% |

### Conditional TimeGAN

| Model | Avg seed-only | Avg after gen | Abs. improvement | Rel. improvement |
| --- | ---: | ---: | ---: | ---: |
| 1D-CNN | 0.6043 | 0.1245 | -0.4798 | -76.60% |
| GRU | 0.1039 | 0.0587 | -0.0451 | -39.90% |
| LSTM | 0.1114 | 0.0489 | -0.0625 | -51.27% |
| CNN-GRU | 0.1349 | 0.0555 | -0.0794 | -50.92% |
| RandomForest | 0.9065 | 0.8891 | -0.0174 | -1.80% |

## Method-Level Summary

### Five-method benchmark

- Best average 1D-CNN result: Time Shift + Jitter, 0.8214
- Best average GRU result: Permutation + Jitter, 0.3862
- Best average LSTM result: Time Warping + Jitter, 0.2137
- Best average CNN-GRU result: Mixup + Jitter, 0.5973
- Best average RandomForest result: Magnitude Scaling + Jitter, 0.9089

### Strongest observed single-run gains

- 1D-CNN: +0.5564 absolute improvement at seed 4 with Time Shift + Jitter
- GRU: +0.4248 absolute improvement at seed 19 with Magnitude Scaling + Jitter
- LSTM: +0.3609 absolute improvement at seed 16 with Time Warping + Jitter
- CNN-GRU: +0.5977 absolute improvement at seed 17 with Mixup + Jitter
- RandomForest: +0.0414 absolute improvement at seed 3 with Mixup + Jitter

### TimeGAN variants

- TimeGAN average accuracy is strongest for RandomForest at 0.9158, but it still drops slightly versus the seed-only baseline.
- Conditional TimeGAN shows the same pattern: weak neural results and a small decline for RandomForest.
- Compared with the classical augmentations above, both TimeGAN variants are consistently less effective on this benchmark.

## Conclusion

For this dataset and evaluation protocol, classical augmentation methods are the most useful synthetic strategy. Among them, Time Warping + Jitter and Time Shift + Jitter are the most reliable for neural models, while Mixup + Jitter gives the strongest RandomForest result. TimeGAN-based generation does not currently outperform the simpler augmentation pipeline.

If needed, this report can be turned into a notebook cell, a PDF-style summary, or a more detailed results appendix.

## DBA (DTW Barycentric Averaging) - Results Summary

- Source CSV: `processed_timegan/dba_vs_seed_only_results.csv`

### Mean accuracy by method

| Method | Mean Accuracy |
|---:|---:|
| DBA (DTW Barycentric Averaging) | 0.4485 |
| Seed Only (No Synthetic) | 0.3834 |

### DBA mean improvement vs Seed Only (by model)

| Model | Mean Improvement (%) |
|---|---:|
| 1D-CNN | 40.05 |
| CNN-GRU | 115.41 |
| GRU | 36.37 |
| LSTM | 12.14 |
| RandomForest | -1.97 |

- **DBA overall mean improvement:** 40.40% (averaged across models)

> Notes: DBA synthetic samples were generated per-class using DTW barycentric averaging; jitter was added to each barycenter. See the notebook `data_generation_experiment_2.ipynb` for generation parameters (series per barycenter, jitter scale, and iterations).