# Experiment artifacts

Use the project's established tracking system when it already has one. Do not introduce MLflow, Weights & Biases, or another external service merely to record a small experiment. A plain local bundle is the default fallback.

## Suggested bundle

```text
artifacts/<experiment-id>/
├── summary.md
├── manifest.json
├── metrics.json
├── logs/
├── figures/
├── predictions/
└── checkpoints/
```

Create only the directories the run actually needs. Large subdirectories normally belong in ignored local or object storage; keep the small manifest and summary easy to review and commit when the user wants a Git record.

`summary.md` should state the question, baseline, changed variable, result, interpretation, caveats, and next experiment in human-readable form.

`metrics.json` should contain machine-readable measured values plus enough labels to avoid ambiguity. Include units, averaging method, threshold, class mapping, or confidence interval when those change the meaning of a metric.

`manifest.json` should record applicable fields such as:

```json
{
  "experiment_id": "segmentation-baseline-001",
  "status": "completed",
  "started_at": "2026-10-06T12:00:00Z",
  "duration_seconds": 540,
  "git_revision": "8eab132",
  "git_dirty": true,
  "entrypoint": "experiments/evaluate.py",
  "config": "configs/baseline.yaml",
  "backend": "colab",
  "hardware": "NVIDIA T4",
  "dataset": "WorldFloods",
  "split": "test",
  "seed": 42,
  "primary_metric": "iou",
  "metrics_file": "metrics.json"
}
```

Use real observed values only. Omit inapplicable fields instead of inventing them. For a dirty working tree, record that fact and preserve a small diff or patch reference when necessary to reproduce the run.

## Run registry

If the project has multiple experiments but no tracker, add a simple append-only index such as `experiments/index.jsonl` or `experiments/results.csv`. Record the experiment ID, time, revision, status, primary metric, configuration, and artifact path. Avoid parallel sources of truth: if the project already has a registry, use it.

## Validation

Before declaring the bundle complete:

- parse JSON files;
- confirm referenced paths exist;
- confirm metrics match the executed logs or evaluator output;
- open or inspect representative figures and predictions;
- ensure secrets, access tokens, private URLs, raw credentials, and unnecessary personal data are absent;
- state whether large artifacts are local-only, remote, ignored, or missing.
