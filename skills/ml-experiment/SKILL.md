---
name: ml-experiment
description: Run, debug, and document reproducible machine-learning experiments, including notebooks, benchmarks, training or evaluation loops, local or Google Colab execution, and artifact capture. Use when the user wants an ML experiment actually executed or iterated; do not use for conceptual ML questions or production deployment alone.
---

# ML Experiment

Own the experiment loop from a stated question to an executed, inspectable result. Treat generated code as unverified until it has run, and distinguish measured results from plans, estimates, and inferences.

## Establish the experiment

Inspect the repository instructions, documentation, environment files, existing experiment entrypoints, notebooks, dataset configuration, result conventions, and relevant prior runs. Check `git status`, the current branch, and the current revision before editing. Preserve unrelated changes and prefer the project's existing structure and tooling.

Translate the request into a small run contract:

- the scientific or engineering question;
- baseline and proposed change;
- dataset and split;
- primary metric and success or stopping criterion;
- expected compute, duration, and material external transfers;
- experiment ID, entrypoint, configuration, seed, and artifact location.

Infer routine details when the repository makes them clear. Ask before choices that materially alter the scientific objective, evaluation methodology, privacy boundary, expected cost, or comparability with earlier runs.

## Choose the execution backend

Use the least complex environment that satisfies the experiment:

1. local execution for smoke tests, preprocessing, analysis, small models, or hardware the machine supports;
2. an existing project-configured remote backend;
3. Google Colab when accelerator compute is useful and its CLI is available or the user approves setting it up.

Do not silently substitute a paid compute provider. Probe actual capabilities rather than assuming that CUDA, MPS, a particular GPU, or a Colab entitlement is available. Validate the selected device from inside the runtime before the substantive run.

For Colab execution, read [references/colab.md](references/colab.md). If `colab` is absent and local execution is inadequate, explain the missing prerequisite and ask before installing or configuring it. Authentication that needs user interaction is a legitimate pause; never inspect or expose credential files.

## Implement for reproducibility

Prefer a normal Python entrypoint plus explicit configuration for reusable training and evaluation logic. Use notebooks for exploration, orchestration, visualization, and narrative when they improve the work. Keep substantial reusable logic in modules when practical.

Make notebooks executable from top to bottom with explicit setup and no accidental hidden-state dependency. If an existing notebook is the requested deliverable, repair and execute that notebook rather than replacing it solely to fit a preferred structure.

Record the inputs needed to reproduce a meaningful run: code revision and dirty state, model, dataset identity and split, preprocessing, hyperparameters, random seed when relevant, dependency information, hardware, duration, and metric definitions. Add deterministic settings only when they are useful and feasible; document remaining nondeterminism rather than pretending it does not exist.

Establish or reproduce a baseline before claiming an improvement. Change one major variable at a time unless the experiment explicitly studies interactions. Preserve scientifically useful negative results.

## Execute and iterate

Run the cheapest useful validation first, then scale up:

- import or syntax check;
- tiny batch, small fixture, or short smoke run;
- baseline or resume check;
- full experiment.

Inspect logs, metrics, samples, plots, and generated files, not only the exit code. Diagnose ordinary technical failures and iterate autonomously when the fix preserves the run contract. Common examples include dependency, path, device, dtype, tensor-shape, notebook-order, checkpoint, and configuration errors.

After each fix, rerun enough to verify the cause was resolved. Do not convert a failed run into an apparent success by weakening assertions, changing the dataset or metric, skipping failed samples, or silently shrinking the workload. If the scientifically meaningful contract must change, record the new experiment separately.

For long runs, stream or capture logs, checkpoint when supported, and inspect progress at sensible intervals. Prefer safe resume over restarting expensive work. Stop and ask before unexpectedly large downloads, sustained or costly compute, uploading data outside the approved boundary, overwriting valuable checkpoints, or deleting artifacts.

## Preserve artifacts and provenance

Use an existing project convention when one exists. Otherwise use the compact artifact bundle described in [references/artifacts.md](references/artifacts.md). Important remote outputs must be copied to durable project storage before an ephemeral runtime is released.

Git is for code, configuration, small notebooks, and human-readable experiment records—not datasets, large checkpoints, bulk predictions, caches, or copied environments. Respect existing ignore rules and add narrow exclusions only when needed.

Use Git for provenance without changing repository history by default:

- record the starting revision and whether the tree was dirty;
- keep unrelated user changes intact;
- summarize the experiment's diff and final working state;
- do not automatically create branches, commits, tags, stashes, pushes, merges, or rebases unless the user requested that Git workflow.

Never push results or code unless explicitly authorized.

## Completion

An experiment is complete when the intended code ran successfully, or a concrete blocker is understood; outputs and metrics were sanity-checked; important artifacts were saved outside ephemeral compute; reproduction metadata was recorded; and the repository state is understood.

Report:

- what actually ran, where, and on what hardware;
- the baseline, changed variable, dataset or split, and seed;
- measured metrics with units or definitions;
- artifact and log paths;
- code and configuration changes;
- failures, caveats, nondeterminism, or unverified assumptions;
- the most informative next experiment.

Never present a planned, partial, selected, or inferred result as a completed measurement.
