# RAFE

RAFE (Retrieval-Augmented Forecasting Evaluation) is a memory-aware benchmark for retrieval-augmented time series forecasting. It treats each method–memory pair as an experimental unit and controls physical rows, unique history records, reference slots, weight concentration, and temporal eligibility. RAFE integrates controlled memory interventions with forecast commitment, paired scoring, and portable replay. Its evaluations span forecasting methods and memory policies across domains, backbones, horizons, and sampling frequencies, providing empirical evidence and practical guidance for memory selection and forecaster design.

This repository is the public source artifact for the VLDB 2027 / PVLDB
Volume 20 Experiment, Analysis & Benchmark manuscript:

> Yang Li and Yijie Shi.
> *RAFE: A Memory-Aware Evaluation Framework for Retrieval-Augmented Time
> Series Forecasting* [Experiment, Analysis & Benchmark].

The published tool is **rafe-eval 0.2.0**.

## Downloads

- [paper.pdf](./paper.pdf) — manuscript PDF rebuilt from the latest section
  sources (13 pages; abstract 201 words).
- [rafe-benchmark-source-20261126.tar.gz](./rafe-benchmark-source-20261126.tar.gz)
  — slim source pack (code, paper sources, official 0.2.0 packages, figures,
  and tables).
- [generalization-bundle-20261126](https://github.com/stefanieCD/rafe-benchmark/releases/tag/generalization-bundle-20261126)
  — GitHub Release with the rebuilt generalization archive of 539 verified
  members, rafe-eval 0.2.0 portable snapshots, and the verification records.

SHA256 of the files in this repository:

```
d83ba743f5d2a0648b90684d0af9a9f5ad3b0e6759906a443d2cc3645e8c7b82  paper.pdf
700b328f645fb2c4ec575aec75fd841f1a7790862f5b4ec6a0e4eaf46f21b1b2  rafe-benchmark-source-20261126.tar.gz
```

Checksums for the Release assets are in
[SHA256SUMS_v2.txt](https://github.com/stefanieCD/rafe-benchmark/releases/download/generalization-bundle-20261126/SHA256SUMS_v2.txt)
on that Release (the original checksum file is also retained there).

### Publication status

| Material | Status |
| --- | --- |
| Manuscript PDF and slim source pack | Published in this repository |
| Generalization bundle (539 verified members, 0.2.0 snapshots, verification records) | Published in [generalization-bundle-20261126](https://github.com/stefanieCD/rafe-benchmark/releases/tag/generalization-bundle-20261126) |
| Expansion bundle | Import in progress |
| Earlier policy-evaluation archives (base / supplement) | Pending publication |

Download links will be added here when those remaining packs are published.

## Extract and run the source pack

```sh
tar -xzf rafe-benchmark-source-20261126.tar.gz
cd rafe-benchmark
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
python -m pip install -e .
# or: python -m pip install dist/rafe_eval-0.2.0-py3-none-any.whl
```

The archive unpacks to a top-level `rafe-benchmark/` directory (92 files),
with `rafe/` (0.2.0 library and CLI), `tests/`, `dist/` (official 0.2.0
wheel and sdist), and `paper/` (section sources, figures, tables, and the
same PDF as `paper.pdf`). It does not include a `.git` directory.

The package core depends only on Python 3.10+ and NumPy. Foundation-model
adapters, when used, manage their own inference dependencies and weights;
those weights are not included here. A short Chinese overview is included
inside the source pack as `README.zh-CN.md`.

These commands use only the extracted source pack and NumPy. They run a
synthetic integration example (three series, two dependency-free plugins)
and check the 0.2.0 tool pipeline:

```sh
rafe demo --out demo_data
rafe freeze demo_data/config.json --run-dir demo_run
rafe predict --run-dir demo_run
rafe score --run-dir demo_run
rafe report --run-dir demo_run
rafe verify --run-dir demo_run
rafe replay --run-dir demo_run
```

On the assembly machine this example produced 12 profiles and 28
comparisons with maximum numeric difference 0.0. See `rafe/README.md`
inside the pack for the plugin API, prepared-bank format, and how to
extend a frozen comparison with a new method.

`paper/build_new_figures_and_scaling.py` and
`paper/retrieval_scaling_results.json` record the retrieval-scalability
figure. Regenerating the timings requires the paper’s method-14 input
arrays, which are not in this source pack; wall-clock numbers will not
bit-match across machines.

## Extract and run the generalization Release

Download `rafe_generalization_bundle-rebuild-20261126.tar.gz` from the
[Release](https://github.com/stefanieCD/rafe-benchmark/releases/tag/generalization-bundle-20261126),
then:

```sh
tar -xzf rafe_generalization_bundle-rebuild-20261126.tar.gz
cd rafe-generalization
python -m venv .venv
.venv/bin/python -m pip install numpy==2.3.5 rafe_eval-0.2.0-py3-none-any.whl
.venv/bin/python -m rafe verify --run-dir primary_snapshot
.venv/bin/python -m rafe verify --run-dir trained_snapshot
```

`primary_snapshot` and `trained_snapshot` are the published 0.2.0 portable
snapshots. The same Release also provides
`rafe-generalization-rebuild-20261126-records.tar.gz`, which holds the
member-difference manifest, official `rafe replay` exact-equality records,
and the independent cross-platform check (`independent_cross_platform_verify.py`,
with explicit `rtol = atol = 1e-12`). Start from the notes on that Release
(`RELEASE_NOTES_v2.md`) for the measured results and the listed unpack
paths.

The snapshots can be moved; paths inside them are relative. Official
`rafe replay` is a read-only exact-equality check of stored scores and does
not rerun foundation-model inference. Foundation-model weights and author
checkpoints are not redistributed.

## License

No license has been selected yet. See `NOTICE` inside the source pack.
Nothing here grants permission to reuse the code or manuscript beyond what
the authors later specify in a `LICENSE` file.

## Citation

Please cite the PVLDB Volume 20 / VLDB 2027 EAB paper once the official
venue bibliographic data are available. The current title and authors are
given at the top of this README.
