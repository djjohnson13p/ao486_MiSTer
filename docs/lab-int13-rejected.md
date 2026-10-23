# INT13 corruption candidate: rejected after baseline audit

Research date: 2026-10-23. Local investigation branch:
`lab/int13-zero-count-corruption`. No source changes made.

The severe corruption described in [issue 243](https://github.com/MiSTer-devel/ao486_MiSTer/issues/243)
was a plausible high-impact candidate: an extended INT13 read/write request
with block count zero could reach the ATA driver, whose sector-count register
interprets zero as 256. However, the issue being open was misleading.

Current default branch and this local clone are based on
`628ddbcb101f836812981b06367b5080161fad4f` (2026-10-06),
[BIOS: Reject bad sector count in int13 extended read/write](https://github.com/MiSTer-devel/ao486_MiSTer/commit/628ddbcb101f836812981b06367b5080161fad4f),
merged through [PR 244](https://github.com/MiSTer-devel/ao486_MiSTer/pull/244).
The production `sw/sysbios/rombios.c` now rejects counts zero or above 127,
zeros the DAP transfer count, and returns through the BIOS failure path before
issuing ATA I/O. This is the exact issue under consideration.

The lab therefore made no duplicate fix and does not claim to have improved
ao486. No full-core simulation or BIOS build was needed to reject this already
fixed candidate. The compiled release ROM and user hardware were not verified.

The local `upstream` remote retains its HTTPS fetch URL, has a disabled push
URL, and a local `pre-push` hook refuses remote writes. No remote fork, issue,
PR, comment, or author contact was created.
