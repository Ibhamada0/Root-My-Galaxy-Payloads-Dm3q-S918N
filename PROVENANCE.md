# PROVENANCE — SM-S918N S918NKSS8FZG1

Source: user-provided `S918N.zip` (Google Drive), containing `boot.img`, `kernel`, `kallsyms.txt`.

| file | size | SHA-256 |
|---|---|---|
| boot.img | 100663296 | `85e3ad7e4a68674370eab147c4335b2828c1f52f315313b82fe4408db39ce0c4` |
| kernel | 46860800 | `53f7e2dac1a1feca2e78259de63b2484698485a88558c965b14d422c281f2343` |
| kallsyms.txt | 5750181 | `2908ecf38591e9509c77c3e0736b08402f04a6a284fbe1e965213cac9475a794` |

BTF blob: single validated candidate at image offset `0x21ef2ac`, 6094556 bytes (magic 0xeb9f, v1).
ARM64 Image: text_offset 0, flags 0xa; kernel region inside boot.img matches raw `kernel` exactly.
KASLR slide at dump time: 0x0 (`_text == 0xffffffc008000000`), so kallsyms offsets are image offsets.

Derived with: PORTING.md procedure (BTF extraction, ftrace event index, qword scans).