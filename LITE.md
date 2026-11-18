# TabArena-Lite evaluation of checkpoint 1,024

The single fixed configuration completed all 51 first outer splits on one NVIDIA GB10. Each fit used the official 8-fold, 1-repeat validation protocol and full refit. The GPU allocation was one device, with four CPUs per model, FP32 inference, one native preprocessing member, and a 512-row support cap. The weights stayed frozen. The benchmark runner was TabArena revision a98c33817cec8f14b496f5a3194a5138697502a1 with AutoGluon 1.6.4b20261003.

The submission adapter pads each GPU query batch to a fixed shape with a training row and discards padding outputs. It completed a fresh full Lite evaluation. Both outer-test and out-of-fold errors were recomputed from saved predictions. Row IDs and target values were checked against OpenML for 346,195 outer-test rows and 692,339 out-of-fold validation rows.

| Model or configuration | Matched-pool Elo | Rank out of 94 |
|---|---:|---:|
| Kumo Small + CellRidge-1024 | 987.5 | 86 |
| Published Kumo Tabular Small (default) | 1838.3 | 5 |

The comparison uses the 93 reference configurations with directly observed results on all 51 first splits in the November 7, 2026 reference snapshot. It includes multiple configurations of some methods. We excluded configurations with missing or imputed entries. We used the TabArena Elo implementation with RF (default) calibrated to 1000, 200 bootstrap rounds, and seed 42. These ranks describe this local comparison. Maintainers independently rerun a contribution before they publish a leaderboard entry.

The candidate beat published Kumo Small on 2 of 51 tasks and lost on 49. The wins were binary classification tasks. Kumo Small's published default uses its own larger context budget and 8 preprocessing members. This comparison changes the pipeline as well as the checkpoint and readout.

In the earlier checkpoint selection run, both variants used identical unpadded GPU batching. On precisely matched splits, checkpoint 1,024 beat checkpoint 448 on 26 datasets and lost on 25. The record by task type was 11 wins and 19 losses in binary classification, 5 wins and 3 losses in multiclass classification, and 10 wins and 3 losses in regression. The difference in matched-pool Elo was +3.6, with the same rank for both checkpoints.

The final rerun retained 987.5 Elo and rank 86/94. The verified split metrics, Kumo comparison and leaderboard below are from the submission adapter. The checkpoint comparison CSV is from the earlier paired experiment.

**All 51 first test splits were seen when choosing between 448 and 1,024 for submission.** This includes 48 earlier development datasets. [Development exposure](DEVELOPMENT.md) describes both stages.

[Verified split metrics](verified-splits.csv) · [Per-task comparison with Kumo Small](paired-kumo.csv) · [Matched leaderboard](matched-leaderboard.csv) · [Checkpoint comparison](paired-checkpoints.csv) · [Comparison metadata](comparison.json)
