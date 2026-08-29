# CHANGES.md — SM-S918N port vs soumarcelino/Root-My-Galaxy-SM-S918B

Base: `soumarcelino/Root-My-Galaxy-SM-S918B` @ main (fetched 2026-08-29,
175 files, verified 175/175 by size).

## New files (device-specific additions)

| file | why |
|---|---|
| `src/targets/dm3q-S918NKSS8FZG1/target.h` | S918N profile: 27 offsets MEASURED from the device kallsyms (kernel 5.15.189-android13-8-33413713-abS918NKSS8FZG1), rest INHERITED from S918B FZF5 / DERIVED (tracefs event id). Evidence: OFFSETS.md. |
| `src/targets/dm3q-S918NKSS8FZG1/p0_fingerprint.h` | P0 oracle fingerprint table generated from the exact S918N raw Image (64 rows, 0x8000 steps, readback-verified). Evidence: PROVENANCE.md. |
| `docs/SM-S918N-S918NKSS8FZG1.md` | Full S918N port record. |
| `docs/TARGET-S918N.md` | Target/firmware identity (device-reported). |
| `OFFSETS.md`, `PROVENANCE.json`, `PROVENANCE.md` | Offset evidence + firmware SHA-256 (boot.img / kernel / kallsyms.txt). |
| `kernelsu/S918N-NOTE.md` | KernelSU build notes for the S918N FZG1 vermagic. |

## Modified files

| file | change | why |
|---|---|---|
| `Makefile` | default `TARGET ?=` -> `dm3q-S918NKSS8FZG1`; added `APP_TARGET_CFLAGS` block (`-DSLIDE_STACK_WRITER=1` for this target) wired into the `-DAPP_PAYLOAD=1` compile lines. | The S918N stack-writer engine requires `SLIDE_STACK_WRITER`; without it the APP payload build fails. |
| `README.md` | Title/target/specs -> SM-S918N; added FLASHING RISKS section + change-list pointers. | New device identity + risk disclosure. |
| `PROJECT-MANIFEST.txt` | Target line -> SM-S918N / S918NKSS8FZG1. | Manifest accuracy. |
| `support/targets-v3.json` | Added `dm3q-S918NKSS8FZG1` payload entry (SM-S918N / 5.15.189). | Feed selects S918N. |
| `support/targets-v2.json` | Inserted S918N target entry at head (device-reported values). | v2 feed for released clients. |
| `app/src/main/assets/targets-v3.json` | Added S918N payload entry pointing at bundled `cve-2026-43499-app-s918n-fzg1.so` + `ksud-f731u-kdp-s918n-fzg1`. | App selects the S918N assets bundled in this repo. |
| `app/src/main/assets/support/targets-v2.json` | Same as support v2 (S918N entry). | Bundled app feed parity. |

## Values CHANGED and their verified source

| value | new | source |
|---|---|---|
| model | SM-S918N | device (owner-reported specs) |
| device | dm3q | device |
| build display | BP4A.251205.006.S918NKSS8FZG1 | device |
| kernel release | 5.15.189-android13-8-33413713-abS918NKSS8FZG1 | device |
| kernel build | #1 SMP PREEMPT Wed Jul 8 02:52:55 UTC 2026 | device |
| baseband | S918NKSS8FZF1 | device |
| SE | Enforcing SEPF_SM-S918N_13_0001 | device |
| exploit asset | `cve-2026-43499-app-s918n-fzg1.so` (131072 B, sha256 measured) | already present in the S918B base repo |
| ksud asset | `ksud-f731u-kdp-s918n-fzg1` (6756208 B, sha256 measured) | already present in the S918B base repo |

## Values UNVERIFIED / REQUIRED FROM DEVICE OWNER (placeholders in files)

| value | placeholder | needed from |
|---|---|---|
| `ro.build.fingerprint` product field | `<VERIFY-PRODUCT>` | `adb shell getprop ro.build.fingerprint` |
| P0_KERNEL_PHYS_LOAD | inherited 0xa7e58000 (S918B) | derive from S918N `sboot.bin` / BL firmware |
| struct layout offsets (task_struct cred etc.) | inherited from S918B FZF5 | verify against S918N BTF (blob extracted) |
| compiler string inside `Linux version` | `<COMPILER-STRING-UNKNOWN>` | full uname/dmesg on device |
| feed URLs | `<YOUR-GITHUB-USER>` | your GitHub username once the repo exists |
