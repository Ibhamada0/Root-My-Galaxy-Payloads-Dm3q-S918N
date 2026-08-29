# tools — SM-S918N notes

- `port-sm-s918b-afzf5.sh`, `f731u-to-dm3q-s918b-afzf5.spec.json`,
  `patch_payload.py` are the S918B-base tooling and are kept unchanged as
  reference.
- For this SM-S918N port the bundled pre-patched assets are already present:
  `app/src/main/assets/cve-2026-43499-app-s918n-fzg1.so` and
  `app/src/main/assets/ksud-f731u-kdp-s918n-fzg1` (taken from the base repo).
- To reproduce a patch spec for S918N, run `cmp -l` between
  `app/src/main/assets/cve-2026-43499-app.so` (S918B) and
  `app/src/main/assets/cve-2026-43499-app-s918n-fzg1.so` and record the
  differing offsets; then mirror `patch_payload.py` with a
  `s918n-fzg1.spec.json`.
