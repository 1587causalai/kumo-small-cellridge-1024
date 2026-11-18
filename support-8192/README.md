# TabArena-Lite comparison at 8,192 support rows

The same frozen Kumo-CellRidge-1024 checkpoint completed 51 first outer splits at support caps of 2,048 and 8,192. The runs used one NVIDIA GB10, one preprocessing member, FP32, a 512-row GPU query batch, and the same seeded training-fold support order. We checked that the outer-test rows, out-of-fold rows, labels, and eight ordered inner splits matched, and recomputed the errors from saved predictions for 346,195 outer-test rows and 692,339 out-of-fold rows per run. Both runs used the official 8-fold, 1-repeat validation protocol and full refit.

| Support cap | Local Elo | Rank in matched pool | Summed test inference |
|---|---:|---:|---:|
| 2,048 | 1196.8 | 63/94 | 282.4 s |
| 8,192 | 1297.6 | 49/94 | 996.9 s |

The candidate gained 100.8 Elo in the same pool of 93 fully observed public configurations. On the outer-test splits the 8,192 cap produced **30 wins, 18 ties, and 3 losses**. On 33 datasets the full-refit support count increased, and on 18 it reached 8,192. The 18 ties were the datasets with unchanged support sizes. Ties use an absolute metric-error difference of at most 1e-9. The summed outer-test inference time was 3.5 times the 2,048 run.

| Task | Wins | Ties | Losses |
|---|---:|---:|---:|
| Binary classification | 19 | 9 | 2 |
| Multiclass classification | 4 | 4 | 0 |
| Regression | 7 | 5 | 1 |

The results use development-exposed splits. The registered 200,000-row cap awaits a full Lite run. The 8,192 measurement used the public model code from adapter commit `40121fdf066eaaf768f99a84d253397191c66545` as an explicit override.

```sh
python packages/tabarena/src/tabarena/models/kumo_cellridge/run_lite.py --max-context-size 8192
```

[Paired split metrics](paired-support.csv) · [Comparison summary](support-comparison.json) · [Reference pool and calibration](../LITE.md) · [TabArena PR #735](https://github.com/autogluon/tabarena/pull/735)
