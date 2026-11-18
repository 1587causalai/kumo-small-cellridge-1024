# TabArena-Lite evaluation of checkpoint 1,024

The submitted model defaults to `max_context_size=200000`, matching [TabArena Kumo Tabular Small](https://github.com/autogluon/tabarena/blob/cc507b84d1f32d2ca5b5704326bf4f086ee6519a/packages/tabarena/src/tabarena/models/kumo_tabular/model.py#L119). **The submitted default has no local full Lite score, measured peak memory or runtime.** The complete local runs used explicit support caps of 512 and 2,048 on the same frozen weights. The official full-default rerun is pending.

`python packages/tabarena/src/tabarena/models/kumo_cellridge/run_lite.py --max-context-size 2048` reproduces the larger measured configuration. Omitting the option selects the 200,000 cap. GPU query batches stay at 512 in both configurations.

The local run used an adapter fixed at 2,048 support rows. On GPU smoke inputs for all three task types, the public adapter with the explicit 2,048 override reproduced its support tensors and predictions exactly. A separate boundary test selected 200,000 of 200,001 rows without running a neural forward at that size.

The larger local run draws up to **2,048 labeled support rows per training fold**. Each GPU query batch contains 512 rows after padding, and padding outputs are discarded. The 512- and 2,048-support runs used identical inference weights, one preprocessing member, FP32, the same seeded support ordering and the same 51 official first outer splits. The original support rows form a prefix of the larger window. The outer full-refit support count increased on 49 datasets, and 33 provided at least 2,048 training rows.

| Configuration | Matched-pool Elo | Rank out of 94 |
|---|---:|---:|
| CellRidge-1024, default cap 200,000 | Unmeasured | — |
| CellRidge-1024, measured cap 2,048 | 1196.8 | 63 |
| CellRidge-1024, up to 512 support rows | 987.5 | 86 |
| Published Kumo Tabular Small, in the 2,048-support comparison | 1836.8 | 5 |

The two measured candidate rows were scored individually against the same 93 published configurations with observed results on all 51 first splits in the November 7 reference snapshot. The pool excludes imputed scores. The TabArena Elo implementation uses RF (default) at 1000, 200 bootstrap rounds and seed 42. Each rank is local to this comparison.

Increasing support changed Elo by **+209.3**. The larger window recorded **45 wins, 2 ties and 4 losses** on the 51 outer test splits. A tie is an absolute metric-error difference of at most 1e-9. The development-exposed measurements below compare fixed configurations of one checkpoint.

| Task | Wins | Ties | Losses |
|---|---:|---:|---:|
| Binary classification | 26 | 2 | 2 |
| Multiclass classification | 6 | 0 | 2 |
| Regression | 13 | 0 | 0 |

The official runner used TabArena a98c33817cec8f14b496f5a3194a5138697502a1 and AutoGluon 1.6.4b20261003, with one NVIDIA GB10 and four CPUs per model. Each run completed the official 8-fold, 1-repeat validation protocol and full refit. Across all 51 datasets, summed model fit time was 364.8 s at 512 and 739.3 s at 2,048. Summed outer inference time was 115.1 s and 282.4 s, respectively. Training rows, outer-test rows, labels and all eight ordered inner-fold assignments matched across the two runs. We checked 346,195 outer-test rows and 692,339 OOF rows against OpenML and recomputed the metrics from saved predictions.

Against the published Kumo Small default, the 2,048-support candidate recorded 4 wins, 0 ties and 47 losses. The published default uses its own context budget and eight preprocessing members.

All 51 first outer test splits were seen during final checkpoint selection at 512 support. We chose to evaluate and submit the larger support cap after observing that baseline and the earlier development evidence. The splits are development-exposed. Maintainer review and an independent full run determine any official board entry. [Development history](DEVELOPMENT.md) · [Previously submitted 512-support report](https://github.com/1587causalai/kumo-small-cellridge-1024/blob/03af50ab418bba343f2fc927e3099fa9c209aa86/LITE.md)

[Paired window results](paired-support.csv) · [Verified split metrics](verified-splits.csv) · [Per-task Kumo comparison](paired-kumo.csv) · [Matched leaderboard](matched-leaderboard.csv) · [Comparison metadata](comparison.json) · [Window summary](support-comparison.json)
