# Personal Codex Skills

A small collection of reusable skills for Codex. Each top-level skill directory is self-contained and includes a `SKILL.md` entry point.

## Skills

| Skill | Purpose |
| --- | --- |
| [`safe-local-install`](./safe-local-install/) | Installs or evaluates local software with minimal machine impact, appropriate isolation, source inspection, verification, and rollback guidance. |

## Install a skill

Clone this repository somewhere stable, then symlink the skills you want into your user-scoped Codex skills directory:

```sh
git clone <repository-url> "$HOME/.local/share/personal-codex-skills"
mkdir -p "$HOME/.agents/skills"
ln -s "$HOME/.local/share/personal-codex-skills/safe-local-install" \
  "$HOME/.agents/skills/safe-local-install"
```

Codex discovers skills from `$HOME/.agents/skills`. If a newly linked skill does not appear, restart Codex.

To install the skill only for one repository, place the symlink under that repository instead:

```sh
mkdir -p .agents/skills
ln -s /absolute/path/to/personal-codex-skills/safe-local-install \
  .agents/skills/safe-local-install
```

## Update

Pull the repository on each machine:

```sh
git -C "$HOME/.local/share/personal-codex-skills" pull --ff-only
```

Because installations use symlinks, pulled changes are available without copying the skill again. Avoid editing the same branch concurrently on multiple machines; make changes in one clone, commit and push them, then pull elsewhere.

## Use

Codex may select a skill automatically when the request matches its description. You can also invoke one explicitly:

```text
$safe-local-install install the requested utility locally
```

Review a skill before enabling it. A skill influences agent behavior and may include executable scripts or references in addition to its instructions.

## Repository conventions

- Keep each skill focused on one workflow.
- Give every skill a clear `name` and trigger-oriented `description` in `SKILL.md`.
- Keep machine-specific files, credentials, generated output, and secrets out of the repository.
- Validate a skill after changing its structure or metadata.
