# SM-S918N — S918NKSS8FZG1

Galaxy S23 Ultra (Korea, `dm3q`) on firmware `S918NKSS8FZG1`
(`BP4A.251205.006.S918NKSS8FZG1`, July 2026 patch), kernel
`5.15.189-android13-8-33413713-abS918NKSS8FZG1`.

Status: **profile source generated from device-supplied kernel + kallsyms;
NOT yet hardware-validated.** Same 33413713 kernel-build family as the
hardware-verified `dm3q-S918BXXSAFZF5` profile; several .data symbols were
re-measured and differ from both S9180 and S918B builds, so this is a
dedicated profile, not a byte-identical port.

## Firmware identity

```text
model: SM-S918N
device: dm3q
One UI 8.5 / Android 16 (SDK 36); GMS update 2026-07-01
kernel release: 5.15.189-android13-8-33413713-abS918NKSS8FZG1
kernel build: #1 SMP PREEMPT Wed Jul 8 02:52:55 UTC 2026
build number: BP4A.251205.006.S918NKSS8FZG1
baseband: S918NKSS8FZF1
SE for Android: Enforcing SEPF_SM-S918N_13_0001
```

## Source material

`S918N.zip` (boot.img, kernel, kallsyms.txt) provided by the device owner.
Hashes and sizes: `PROVENANCE.md` / `PROVENANCE.json`. The kernel region
inside `boot.img` matches the raw `kernel` file exactly
(SHA-256 `53f7e2da...`).

- ARM64 Image: `text_offset 0`, `flags 0xa`, base `0xffffffc008000000`.
- BTF: single validated blob found at image offset `0x21ef2ac`
  (6,094,556 bytes) — layout re-verification is a required step.
- kallsyms: `_text == base` at dump time (slide 0), so every symbol
  offset is an exact image offset.

## Measured vs inherited constants

Every constant lives in `src/targets/dm3q-S918NKSS8FZG1/target.h` and is
tagged `MEASURED`, `DERIVED`, or `INHERITED`. Summary in `OFFSETS.md`.

Key deltas vs the S918B FZF5 profile (all MEASURED from this build):

| constant | S918B FZF5 | SM-S918N FZG1 |
| --- | ---: | ---: |
| `KMALLOC_CACHES_OFF` | 0x02064578 | 0x020642f8 |
| `ANON_PIPE_BUF_OPS_OFF` | 0x01e7f560 | 0x01e7f2e0 |
| `ASHMEM_FOPS_OFF` | 0x0200d5b8 | 0x0200d338 |
| `ASHMEM_MISC_FOPS_OFF` | 0x02bfcf28 | 0x02bfcf18 |
| `SLIDE_NFULNL_LOGGER_NAME_OFF` | 0x01d5dd96 | 0x01d5db2e |
| `SLIDE_RANDOM_TABLE_BOOT_ID_DATA_PTR_OFF` | 0x02bba9c8 | 0x02bba9c8 (same) |
| `SLIDE_TRACEFS_EVENT_ID` | 108 | 108 (derived: 20 + 88) |

## P0 fingerprint

`p0_fingerprint.h` — 64 rows, 0x8000 steps (0x000000..0x1f8000), 8 qwords
per row at page offsets 0x000..0xe00, generated from and readback-verified
against the exact S918N raw Image.

## Build

```sh
make TARGET=dm3q-S918NKSS8FZG1 ANDROID_NDK_HOME=/path/to/android-ndk release
```

Publish `build/dm3q-S918NKSS8FZG1/cve-2026-43499-app.release.so` as
`artifacts/dm3q-S918NKSS8FZG1/cve-2026-43499-app.so`, then set its real
size in `support/targets-v3.json` (placeholder currently holds the
same-family S918B size 133344). If the release build exceeds the Makefile's
fixed-size gate (`APP_RELEASE_SIZE 104128`), raise it for this target —
several published dm3q artifacts exceed 104128.

## KernelSU

Follow `kernelsu/README.md`. Expected module identity:

```text
vermagic: 5.15.189-android13-8-33413713-abS918NKSS8FZG1 SMP preempt mod_unload modversions aarch64
```

Name artifacts `android13-5.15.189_kernelsu-dm3q-S918NKSS8FZG1.ko` and
`ksud-dm3q-S918NKSS8FZG1-kdp`. The kernel exports no `commit_creds` /
`selinux_state`, so expect the same missing-export/CRC pattern as the S918B
profile, bypassed at runtime through the CVE-2026-43499 read/write primitive.

## Remaining verification (before any on-device run)

1. Derive `P0_KERNEL_PHYS_LOAD` from the SM-S918N `sboot.bin` (BL archive) —
   currently inherited (0xa7e58000) from the S918B FZF5 build, UNCONFIRMED.
2. Re-validate struct layouts (task_struct cred, waiter, pool/workqueue)
   against the S918N BTF blob — currently inherited from S918B FZF5.
3. Confirm `SLIDE_TRACEFS_WORKER_CALLER_OFF` / VFORK caller by disassembling
   the recovered S918N ELF (text offsets are the same family, but verify).
4. Fill real artifact sizes in `support/targets-v3.json`.
5. Run on hardware (Shizuku mode). Expect retries and per-boot probability —
   the same reliability notes as S918B FZF5 apply (run close to boot;
   after a stack-writer attempt reboot before retrying).

Use only on a device you own.
