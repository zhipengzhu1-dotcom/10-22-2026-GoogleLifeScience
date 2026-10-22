# OASE work happens in a private Working Copy, and Ports go to a fork

OASE contributions are built in a plain copy of `applications/open-agentic-science-engine/` kept at `oase/` in this private repo, not in a fork of GoogleCloudPlatform/LifeSciences. This keeps drafting private and next to the campaign's plans, ADRs and PR text. The cost is manual Ports and re-imports.

## Mechanics

- Each import is a single commit (`import: OASE @ <sha>`) with no changes of ours, and the upstream SHA is recorded in `oase/UPSTREAM`. Google's licence headers stay untouched.
- `git diff <import>..HEAD -- oase/` is exactly the patch for a PR. To Port, apply it to a branch off latest upstream `main` in the fork with `git am --directory=applications/open-agentic-science-engine`, run upstream's checks, and push.
- Re-import upstream before every Port, and at least weekly while a PR is open, with our work rebased on top.
- The fork carries no work of its own. Do not "fix" this by moving development into the fork.

## Considered Options

- **Develop directly in a fork**: the conventional path. Rejected so that drafts stay private alongside the campaign material.
- **Fork as a git submodule**: rejected as fiddly on Windows.
