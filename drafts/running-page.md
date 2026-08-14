# Draft: the "Running" page

**Status:** Draft, not published
**Recorded:** 2026-08-14

`real-box.mdx` was deleted from the site on 2026-08-14. Henry's call: the
page described a workflow the platform had moved past, and rewriting it is
worth doing properly rather than in the same pass as the CLI reference.
This file holds what the page said, why it went, and what should replace it.

## Why it was deleted

The platform shipped self-managing boxes (`ca1dbd9`, "PR C: self-managing
boxes — --remote submission, --push, the watchdog"). The page still
described the manual world that came before it.

| The page said | The platform has |
| --- | --- |
| find a host that passes a contract | `monte box up \| status \| down` — ephemeral Lambda boxes with an on-box watchdog |
| run `scripts/setup-remote.sh` by hand | `--remote` ships the code and launches the job |
| SSH to the box and work there | `monte eval/train --remote` submits and returns |
| two storage tiers | plus the results bucket and the local Mirror (`monte sync`, `monte push`) |

The page's closing note said box addresses and SSH stay out of the docs on
purpose. `monte box up` is what makes that true, and the page never
mentioned it.

## What should replace it

A Getting-started page titled **Running**, sitting after the Quickstart.

The reason it is its own page and not a Quickstart section: `--remote`
needs a git checkout. `reproducibility_gate` runs `git -C <repo_root>`
with `repo_root` derived from the installed module path
(`src/monte/core/remote.py:453`), so a `uv tool install` has no repo and
`--remote` refuses with `E_REMOTE` — *"run --remote from inside the monte
repo checkout"*. The Quickstart's whole premise is `uv tool install` with
no clone. Folding these together would make it contradict itself.

Two audiences, two pages:

| | Quickstart reader | Running reader |
| --- | --- | --- |
| Install | `uv tool install` | git checkout |
| Runs | mock only | real |
| Commands | `init`, `eval`, `train`, `status` | plus `box up`, `--remote`, `sync`, `push`, `promote` |

Proposed shape:

1. **Get a GPU.** `monte box up` launches an ephemeral Lambda box, waits
   for it to go active, and arms the watchdog. Default type
   `gpu_1x_h100_pcie`. `--region` pins a region, `--cold` skips the NFS.
   `monte box status` lists the account's instances and flags any over
   TTL. `monte box down <ref>` terminates one through the API.
2. **Submit a run.** `monte eval <m> --remote` and `monte train <m>
   --remote`. State the checkout requirement here, first thing. Cover the
   flag algebra: `--remote` refuses with `--push` and with `--dry-run`.
   A real remote train needs a Hugging Face token.
3. **Watch it land.** The job is detached. `monte sync <m>` pulls the
   bucket into the local Mirror, then `monte status <m>` reads it. The box
   terminates itself at job end.
4. **Bring your own box.** The contract table below, kept, plus the
   storage tiers. Drop `scripts/setup-remote.sh` — the Quickstart says you
   never clone the repo, so pointing at a script inside it does not work
   for the reader who arrives from there.

Open questions to settle before writing:

- How `monte box up` relates to the old bootstrap. Does the NFS replace
  the cold-box story, or does a cold box still need `setup-remote.sh`?
- Whether the Mirror makes the storage table three tiers instead of two.
- Whether credential setup (Lambda key, GCS key, HF token, and the
  1Password path) belongs on this page or its own. There are ten-plus env
  vars behind it; the CLI reference now lists them, which may be enough.

## The deleted page, verbatim

````mdx
---
title: "Run on a real box"
description: "The box contract, bootstrap, and the two storage tiers"
---

Real evals and real training run on a single-GPU Linux box. Monte does not know any box by name. Any host that passes the contract below works.

## The box contract

- One GPU with about 80 GB of VRAM and a working NVIDIA driver.
- At least 350 GB of free disk on the home filesystem.
- Persistent storage for the durable data root.
- Outbound network to GitHub, PyPI, Hugging Face, NVIDIA NGC (`nvcr.io`), and `astral.sh` for the uv installer.
- For training: Docker with the NVIDIA container toolkit. The training stack runs only inside the pinned NeMo-RL container. Bare-metal training is a dead end, not a shortcut.

`scripts/setup-remote.sh` asserts this contract before it changes anything.

## Bootstrap

Bootstrap is idempotent. Run it again at any time. It asserts the contract, installs uv and the repo, clones the pinned NeMo Gym, downloads the base-model snapshot, and runs `monte env install --all`. A cold box takes about 25 minutes. A warm one takes about 7.

## Two storage tiers

| Tier | Root | Holds | Survives a box loss |
|---|---|---|---|
| Disposable | `MONTE_LOCAL` (default `~/monte`) | repo, venvs, caches, scratch rollouts | no. Bootstrap rebuilds it |
| Durable | `MONTE_ROOT` data root | Measurements, frozen Splits, the Ledger, Run artifacts | yes. This is the record |

`monte config` prints the resolved data root and where the value came from.

## Crossing from laptop to box

A Run is real when the serving or container stack is present on the box, and mock when it is not. Monte refuses to guess silently:

- On a GPU machine without the stack, a Run refuses instead of silently mocking. Bootstrap installs the stack.
- Once a Measurement holds real evidence, Monte refuses a mock Run into it.
- Training preflight checks free disk and refuses below its floors. The refusal names the exact floor and which filesystem is short.

Two knobs help imperfect boxes. `MONTE_GPU_MEM_UTIL` caps the fraction of GPU memory the eval server can claim (default `0.9`). `MONTE_DOCKER` overrides the docker command, for example `sudo docker`.

<Note>
Box addresses, SSH configuration, and host names are operational details. They stay out of these docs on purpose.
</Note>
````

## What survived the deletion

The CLI reference gained the commands the page should have covered:
`monte box up/status/down`, `monte sync`, `monte push`, `--remote`,
`--push`, and every environment variable behind them. So nothing is
undocumented — what is missing is the page that explains the workflow
they belong to.

The mock-versus-real rule moved nowhere: `troubleshooting.mdx` already
carries it under "My Run mocked, or refused to mock".
