# Kumo Small + CellRidge, checkpoint 1,024

This release contains separate classification and regression inference weights for a derivative of NVIDIA Kumo Tabular Small. Four additional row blocks transform the intermediate cell and ICL representations. An affine ridge fit on labeled support rows produces the prediction for each task. The classification readout uses a softmax temperature of 0.25. The native single-member preprocessing recipe uses seed 104.

The [v1.0 release](https://github.com/1587causalai/kumo-small-cellridge-1024/releases/tag/v1.0) contains both safetensors files. [weights.json](weights.json) records their SHA-256 digests, byte sizes, and source checkpoint digests. Both exports were verified tensor by tensor against the source. Use the classification file for binary or multiclass tasks and the regression file for numerical targets. Both the backbone and additional blocks were fine-tuned. The release contains the inference tensors only.

## Development and benchmark exposure

The continuation used independent synthetic training tables, 2,048 labeled support rows and 64 query rows per table. Every 16 tables contributed one optimizer update, with a backbone learning rate of 3e-6 and a tail learning rate of 3e-5. The 1,024-table checkpoint is the output of 64 continuation updates, using the original learning rates throughout this lineage. It resumes from the original-rate replay at 512 tables.

## Development and benchmark exposure

**We compared checkpoint 448 with checkpoint 1,024 on the 51 first official outer test splits before choosing this submission.** Earlier development also repeatedly scored up to 512 query rows drawn from 48 of those test splits (22,807 labels in total). These labels informed checkpoint and submission selection. The continuation gradient updates used synthetic tables. Scores on these 51 splits are development-exposed and cannot establish independent generalization. [Previously used datasets](DEVELOPMENT.md)

The restricted 48-table development evaluation gave 993.2 Elo. On the complete Lite test partitions, our comparison against 93 fully observed public configurations gave 987.5 Elo (86th of 94). The two evaluations use different inference protocols and reference pools. The official leaderboard requires an independent maintainer run.

[TabArena-Lite results and per-task metrics](LITE.md)

## Source and license

The base weights come from NVIDIA Kumo Tabular, revision 3c3e10bbdb590ace29e7026847f92db3c603096d. The structured-data-models implementation used by this derivative is revision b780fc92b2cd50df5525575f0ddd1ae9038102bc. The weight license is OpenMDW-1.1. [NVIDIA model](https://huggingface.co/nvidia/Kumo-Tabular) · [Pinned implementation](https://github.com/NVIDIA/structured-data-models/tree/b780fc92b2cd50df5525575f0ddd1ae9038102bc) · [License and copyright notice](LICENSE)
