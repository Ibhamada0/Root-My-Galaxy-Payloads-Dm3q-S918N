# KernelSU build notes — SM-S918N (dm3q, S918NKSS8FZG1)

Same procedure and Samsung KDP/RKP/DEFEX patch as the S918B FZF5 profile.
Source tree: Samsung msm-kernel Kalama 5.15 opensource for SM-S918N.

Expected `modinfo` vermagic:

```text
5.15.189-android13-8-33413713-abS918NKSS8FZG1 SMP preempt mod_unload modversions aarch64
```

- Reconstruct `Module.symvers` from the recovered S918N `vmlinux.elf`
  (`kernelsu/tools/extract_target_symvers.py`).
- Strip debug sections; embed the KO in `ksud`.
- Publish as `android13-5.15.189_kernelsu-dm3q-S918NKSS8FZG1.ko` and
  `ksud-dm3q-S918NKSS8FZG1-kdp`, then reference them from
  `support/targets-v3.json`.
