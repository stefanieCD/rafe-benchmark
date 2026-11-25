# RAFE

**RAFE** (Retrieval-Augmented Forecasting Evaluation) is a memory-aware
benchmark for retrieval-augmented time series forecasting. It treats the
method–memory pair as a first-class experimental unit: after the forecaster
is frozen, policies are compared under explicit budgets for physical rows,
unique history records, returned slots, and weight concentration, together
with time-legal eligibility (`e + H ≤ t`). The contribution is this
controlled, auditable evaluation protocol and the accompanying interventions,
not a new forecasting architecture, and not a claim that one memory policy
stably dominates the others.

This repository is the public source artifact for the VLDB 2027 / PVLDB
Volume 20 Experiment, Analysis & Benchmark manuscript:

> Yang Li and Yijie Shi.
> *RAFE: A Memory-Aware Evaluation Framework for Retrieval-Augmented Time
> Series Forecasting* [Experiment, Analysis & Benchmark].

The published tool in the source pack is **rafe-eval 0.2.0**.

## Files in this first upload

These three files are the current public entry point. Download them from
the same directory as this README:

- [paper.pdf](./paper.pdf) — manuscript PDF rebuilt from the latest section
  sources (13 pages; abstract 201 words).
- [rafe-benchmark-source-20261126.tar.gz](./rafe-benchmark-source-20261126.tar.gz)
  — slim source pack (code, paper sources, official 0.2.0 packages, figures,
  and tables).
- [README.md](./README.md) — this file.

SHA256:

```
d83ba743f5d2a0648b90684d0af9a9f5ad3b0e6759906a443d2cc3645e8c7b82  paper.pdf
700b328f645fb2c4ec575aec75fd841f1a7790862f5b4ec6a0e4eaf46f21b1b2  rafe-benchmark-source-20261126.tar.gz
```

## Source pack versus later reproduction bundles

`rafe-benchmark-source-20261126.tar.gz` is only the slim source tree. It is
enough to read the paper, install rafe-eval 0.2.0, and run the synthetic
tool-pipeline example below. It does **not** contain committed-forecast
archives, foundation-model weights, author checkpoints, or the large
portable bundles needed to regenerate every table in the paper.

Larger reproduction packs (including an expansion bundle, a 0.2.0
generalization bundle, and earlier policy-evaluation archives) are planned
for a later GitHub Release. They are not part of this first upload, and
this README does not link them until those assets are actually published.
A short Chinese overview is included inside the source pack as
`README.zh-CN.md`.

## Extract the source pack

```sh
tar -xzf rafe-benchmark-source-20261126.tar.gz
cd rafe-benchmark
```

The archive unpacks to a top-level `rafe-benchmark/` directory (92 files),
with `rafe/` (0.2.0 library and CLI), `tests/`, `dist/` (official 0.2.0
wheel and sdist), and `paper/` (section sources, figures, tables, and the
same PDF as `paper.pdf`). It does not include a `.git` directory.

## Install rafe-eval 0.2.0

```sh
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
python -m pip install -e .
```

Or install the official wheel bundled in the pack:

```sh
python -m pip install dist/rafe_eval-0.2.0-py3-none-any.whl
```

The package core depends only on Python 3.10+ and NumPy. Foundation-model
adapters, when used, manage their own inference dependencies and weights;
those weights are not included here.

## A verifiable rerun

The following commands use only the extracted source pack and NumPy. They
run a synthetic integration example (three series, two dependency-free
plugins). This is a tool-pipeline check, not experimental evidence for the
paper, and it does not by itself reproduce the paper tables.

```sh
rafe demo --out demo_data
rafe freeze demo_data/config.json --run-dir demo_run
rafe predict --run-dir demo_run
rafe score --run-dir demo_run
rafe report --run-dir demo_run
rafe verify --run-dir demo_run
rafe replay --run-dir demo_run
```

`replay` recomputes committed statistics read-only and requires exact
numeric agreement. On the assembly machine this example produced 12
profiles and 28 comparisons with maximum numeric difference 0.0.

See `rafe/README.md` inside the pack for the plugin API, prepared-bank
format, and how to extend a frozen comparison with a new method.

`paper/build_new_figures_and_scaling.py` and
`paper/retrieval_scaling_results.json` record the retrieval-scalability
figure. Regenerating the timings requires the paper’s method-14 input
arrays, which are not in this source pack; wall-clock numbers will not
bit-match across machines.

## License

No license has been selected yet. See `NOTICE` inside the source pack.
Nothing here grants permission to reuse the code or manuscript beyond what
the authors later specify in a `LICENSE` file.

## Citation

Please cite the PVLDB Volume 20 / VLDB 2027 EAB paper once the official
venue bibliographic data are available. The current title and authors are
given at the top of this README.
