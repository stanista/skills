# Forgekit

A small, auditable engineering toolkit for Codex, distributed as an installable plugin through a Git-backed marketplace.

## Skills

| Skill | Purpose |
| --- | --- |
| [`ml-experiment`](./skills/ml-experiment/) | Runs and debugs reproducible ML experiments locally or on Google Colab while preserving metrics, artifacts, and Git provenance. |
| [`safe-local-install`](./skills/safe-local-install/) | Installs or evaluates local software with minimal machine impact, appropriate isolation, source inspection, verification, and rollback guidance. |

## Install

Add this repository as a Codex plugin marketplace, then install the plugin:

```sh
codex plugin marketplace add stanista/skills
codex plugin add stanista-skills@stanista-skills
```

Start a new Codex chat after installation so the bundled skill is available. You can inspect installed plugins with:

```sh
codex plugin list
```

The plugin contains instructions only: it does not bundle hooks, executables, MCP servers, or background services.

## Update

Refresh the marketplace checkout on each machine:

```sh
codex plugin marketplace upgrade stanista-skills
```

Start a new chat after an update. Published plugin changes should increment the version in [`plugin.json`](./plugin.json).

If you installed version `0.2.0` or earlier under the original plugin name, migrate once:

```sh
codex plugin remove safe-local-install@stanista-skills
codex plugin marketplace upgrade stanista-skills
codex plugin add stanista-skills@stanista-skills
```

## Remove

Remove the plugin, then remove the marketplace if you no longer use any plugin from it:

```sh
codex plugin remove stanista-skills@stanista-skills
codex plugin marketplace remove stanista-skills
```

## Use

Codex may select a skill automatically when the request matches its description. You can also invoke one explicitly:

```text
$safe-local-install install the requested utility locally
$ml-experiment run and document this benchmark reproducibly
```

Review a skill before enabling it. A skill influences agent behavior and may include executable scripts or references in addition to its instructions.

## Repository layout

- [`plugin.json`](./plugin.json) is the portable Agent Plugins manifest.
- [`.agents/plugins/marketplace.json`](./.agents/plugins/marketplace.json) exposes the plugin to Codex from this Git repository.
- [`skills/`](./skills/) contains the bundled skill directories discovered by plugin hosts.

## Repository conventions

- Keep each skill focused on one workflow.
- Give every skill a clear `name` and trigger-oriented `description` in `SKILL.md`.
- Keep machine-specific files, credentials, generated output, and secrets out of the repository.
- Validate a skill after changing its structure or metadata.
- Bump the plugin version when publishing behavior changes.
