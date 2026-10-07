# digger-small-management-repo

A deliberately heavy repository used as `DIGGER_MANAGEMENT_REPO` when testing
Digger's `git_timeout`. A `git clone --depth 1 --single-branch` of `main` is
meant to take well over Digger's default 30 second per-command git timeout on a
GitHub-hosted runner, so a policy check that clones this repo only succeeds when
the configured `git_timeout` actually reaches the clone.

This is the *small* twin of digger-heavy-management-repo: the same `policies/access.rego`
and ~10k dummy terragrunt files but no random blobs, so a depth-1 clone is ~1.1 MB and
20k objects. Use it when the slowness comes from the runner's network (tc or a throttling
proxy) rather than from the amount of data. The clone needs ~12 HTTPS round trips, so
either rate-limit it (150 kbit/s gives ~60 s) or add ~2.5 s one-way delay per round trip.
