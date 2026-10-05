# Google Colab backend

Use Colab as disposable compute, not as the canonical source tree or the only copy of results. CLI behavior and account entitlements can change, so inspect the installed interface with `colab --help`, subcommand help, and—when available—`colab AGENT` or `colab README` before relying on a remembered flag.

Official references:

- <https://github.com/googlecolab/google-colab-cli>
- <https://github.com/googlecolab/google-colab-cli/blob/main/skills/colab-operator/SKILL.md>
- <https://research.google.com/colaboratory/faq.html>

## Preflight

1. Check `command -v colab` and `colab --help`.
2. Run `colab sessions` to test CLI authentication and discover active sessions. The global auth flag, when needed, precedes the subcommand, for example `colab --auth=adc sessions`.
3. If interactive authentication is required, ask the user to complete it. Do not read token files or place credentials in the repository, notebook, logs, or command arguments.
4. Do not confuse CLI authentication with `colab auth`, which injects separate VM-side Google Cloud credentials for code running inside the runtime.
5. Reuse a suitable session when safe. Track whether the agent created the session so a pre-existing user runtime is never stopped automatically.

GPU availability, runtime duration, and supported accelerator requests depend on the account, current capacity, and Colab policy. Treat allocation or reclamation as an infrastructure condition, not proof that experiment code is wrong. Do not repeatedly request increasingly expensive hardware without a technical reason and user approval for possible cost.

## Prefer job mode

Use the CLI's provision, execute, retrieve, and stop workflow. Representative commands are:

```sh
colab new -s <session> --gpu <accelerator>
colab install -s <session> -r requirements.txt
colab exec -s <session> -f experiments/run.py
colab exec -s <session> -f notebooks/experiment.ipynb
colab log -s <session> -o artifacts/<experiment-id>/logs/colab.md
colab download -s <session> /content/<remote-artifact> <local-path>
colab stop -s <session>
```

Confirm exact syntax locally. Prefer a project environment setup that is repeatable over a sequence of undocumented interactive installs. Verify the assigned hardware from inside the runtime with a small diagnostic appropriate to the framework, such as `nvidia-smi` and `torch.cuda.is_available()` for a PyTorch CUDA run.

Kernel state can persist across executions in one session. Experiments must not depend on that hidden state: reset or recreate the runtime when validating top-to-bottom reproducibility.

## Files and artifacts

Keep code edits local, then send only the files and data required for the run. Obtain authorization before uploading sensitive or private data when remote transfer was not already part of the request.

Know which outputs are written back locally and which exist only on the VM. Explicitly download remote logs, metrics, figures, predictions, and checkpoints that matter. Verify the local files are readable and nonempty before considering retrieval successful.

Do not mount Google Drive by default. Drive mounting and VM-side cloud authentication can require separate interactive consent and broaden the data-access boundary.

## SSH is an escape hatch

Prefer `colab exec`, `upload`, `download`, `install`, and `log`. Use `colab ssh` or its OpenSSH proxy mode only when shell-level access is genuinely useful, the installed CLI supports it, and the account's policy permits remote control. Free managed runtimes may restrict SSH or other remote-control patterns; never use tunneling or keep-alive tricks to bypass Colab policy.

Do not modify the user's SSH configuration unless explicitly asked. If SSH is used, consult `colab ssh --help`; key support, flags, and runtime behavior may differ by CLI release.

## Teardown

Before stopping a session:

- ensure no intended run is still active;
- retrieve and verify important artifacts;
- capture logs and environment or hardware metadata;
- preserve useful partial checkpoints;
- confirm that the session was created for this task or obtain user approval to stop it.

Stop a task-created session when work is complete and no requested follow-up needs it. If teardown fails, report the session name so the user can locate it.
