# Root My Galaxy SM-S918N

Root My Galaxy port for the Samsung Galaxy S23 Ultra `SM-S918N` (`dm3q`,
Korea) on firmware `S918NKSS8FZG1`, based on the
[Root-My-Galaxy-SM-S918B](https://github.com/soumarcelino/Root-My-Galaxy-SM-S918B)
port by soumarcelino (which itself derives from
[youyoudezhuzhu/rmg-f731u](https://github.com/youyoudezhuzhu/rmg-f731u)).

This repository contains the Android app, target configuration, porting
sources, patches, and build tools, adapted for the `SM-S918N` /
`S918NKSS8FZG1` target.

**Use this only on devices you own or are explicitly authorized to test.**

## Validated Target (device-reported)

```text
model: SM-S918N
device: dm3q
One UI: 8.5
Android: 16 (SDK 36)
Google Play system update: 2026-07-01
build display: BP4A.251205.006.S918NKSS8FZG1
baseband: S918NKSS8FZF1
kernel release: 5.15.189-android13-8-33413713-abS918NKSS8FZG1
kernel build: #1 SMP PREEMPT Wed Jul 8 02:52:55 UTC 2026
SE for Android: Enforcing SEPF_SM-S918N_13_0001
```

> TODO before shipping: confirm `ro.build.fingerprint` product name on the
> device (`samsung/dm3q/<product>/dm3q:16/BP4A.251205.006/S918NKSS8FZG1:user/release-keys`).
> KernelSU binary/version: unchanged from the S918B base (see kernelsu-next/README.md).

## Every Change vs the S918B Base

See [CHANGES.md](CHANGES.md) for the complete diff-style change list (every
file touched, what changed, and why) plus
[docs/TARGET-S918N.md](docs/TARGET-S918N.md) and
[docs/SM-S918N-S918NKSS8FZG1.md](docs/SM-S918N-S918NKSS8FZG1.md) for the full
port record and offset evidence.

## ⚠️ FLASHING / ROOT RISKS (read before anything)

- **Knox warranty**: rooting via kernel exploit + KernelSU can trip the Knox
  e-fuse / set the warranty bit (0x1) permanently; Samsung Pay, Secure
  Folder, Samsung Pass and some banking apps may stop working. Usually NOT
  reversible.
- **Bootloader / KG lock**: the Korean `SM-S918N` SKU ships with the
  bootloader locked (KG state `LOCKED` / `PRECHECK`). Do NOT attempt to
  unlock it unless you fully understand the consequences — unlocking wipes
  the device and some carriers require a KG unlock code that may be
  unavailable.
- **Panic / reboot / freeze**: the exploit stage is probabilistic; failures
  can panic-reboot or hard-freeze the phone (hold power to recover). Run
  close to boot and expect retries.
- **No official support**: community exploit port, not endorsed by Samsung.
  You are responsible for your own device.

## Prerequisites (same as the S918B base)

1. Developer options + USB debugging.
2. **"Disable child process restrictions"** in Developer options.
3. Install [Shizuku](https://shizuku.rikka.app/).
4. Reboot the phone (clean boot).
5. Close every other app; keep only Shizuku and Root My Galaxy.
6. Start Shizuku, open Root My Galaxy, grant permission.

## Bundled S918N payloads

| asset | size | purpose |
|---|---|---|
| `app/src/main/assets/cve-2026-43499-app-s918n-fzg1.so` | 131072 | closed-engine exploit payload (pre-patched, from the S918B base repo) |
| `app/src/main/assets/ksud-f731u-kdp-s918n-fzg1` | 6756208 | KernelSU daemon (KDP-excluded build for S918N FZG1) |
| `app/src/main/jniLibs/arm64-v8a/libcve43499root.so` | - | root helper |

## Open-source payload build (alternative path)

```sh
make TARGET=dm3q-S918NKSS8FZG1 ANDROID_NDK_HOME=/path/to/android-ndk release
```

`src/targets/dm3q-S918NKSS8FZG1/target.h` holds 27 offsets MEASURED from the
device kallsyms (rest INHERITED/DERIVED — see OFFSETS.md) and
`p0_fingerprint.h` was generated from the exact device raw Image. Note: the
Makefile release gate (`APP_RELEASE_SIZE`) is sized for the small
open-source payload; the bundled closed-engine `.so` (131072 B) is a
pre-patched binary, NOT built by this Makefile.

## Documentation

- [CHANGES.md](CHANGES.md) — this port's full change list.
- [docs/TARGET-S918N.md](docs/TARGET-S918N.md) — target identity + status.
- [docs/SM-S918N-S918NKSS8FZG1.md](docs/SM-S918N-S918NKSS8FZG1.md) — port record.
- [OFFSETS.md](OFFSETS.md) — measured vs inherited constants.
- [PROVENANCE.md](PROVENANCE.md) / `PROVENANCE.json` — firmware hashes.
- [docs/BUILD_INSTALL_ADB.md](docs/BUILD_INSTALL_ADB.md) — build/install manual (S918B base wording).
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) — common failures.

## Credits And Base Repository

Base port: [soumarcelino/Root-My-Galaxy-SM-S918B](https://github.com/soumarcelino/Root-My-Galaxy-SM-S918B)
→ original: [youyoudezhuzhu/rmg-f731u](https://github.com/youyoudezhuzhu/rmg-f731u)
→ exploit base: [BuSung-dev/Root-My-Galaxy-Payloads](https://github.com/BuSung-dev/Root-My-Galaxy-Payloads).
S918N offset data measured from the device-owned kernel/kallsyms provided by
the owner of this port.


## CI / GitHub Actions

A workflow (`.github/workflows/build.yml`) builds the APK automatically on
every push (and on PRs / manual runs):

- Runs on `ubuntu-latest` with JDK 21 and the checked-in Gradle 9.5.1
  wrapper. NDK/CMake are resolved by AGP (auto-download; SDK licenses are
  accepted in the runner).
- Builds `assembleDebug` and `assembleRelease`.
- Uploads workflow artifacts:
  - `RootMyGalaxy-debug-apk` - debug-signed, installable (`adb install`).
  - `RootMyGalaxy-release-apk` - unsigned by default.
- **Signed release APK (optional):** add repository secrets
  `KEYSTORE_BASE64` (base64 of your `.jks`), `KEYSTORE_PASSWORD`,
  `KEY_ALIAS`, `KEY_PASSWORD`. When present, the workflow signs
  `app-release-unsigned.apk` with `apksigner` and uploads
  `app-release-signed.apk`.
- **Pushing a tag `v*`** creates a GitHub Release with the APK attached
  (signed if available, otherwise the debug APK).

To build the same thing locally:

```bash
./gradlew :app:assembleDebug
./gradlew :app:assembleRelease
```
