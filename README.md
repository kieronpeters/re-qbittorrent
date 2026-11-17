# re-qbittorrent

An independent, clean-room reconstruction of a BitTorrent client modelled on
qBittorrent: session engine, persistence, and a JSON/HTTP control API, with a
minimal UI.

> **Not affiliated with, endorsed by, or derived from the source code of the
> qBittorrent project.** No third-party binaries, symbol files, disassembly
> listings, or copied source are included in this repository.

## Target under analysis

| Field | Value |
|---|---|
| Application | qBittorrent |
| Version | 5.2.3 |
| Publisher | The qBittorrent Project |
| Executable | `C:\Program Files\qBittorrent\qbittorrent.exe` |
| Size | 43,303,936 bytes |
| Format | PE32+ (`pei-x86-64`), x86-64 |
| SHA-256 | `f69360ae8545a64f4fc84fb6bacef03d77a6aa0793a4c14d4a28651ca26a27d1` |
| Shipped symbol file | `qbittorrent.pdb` (181,809,152 bytes) — present on disk, **not committed here** |
| Baseline captured | 2026-11-16 |

Identity, version, size and PE architecture were verified from registry data,
file metadata and a static `objdump -f` header read. The digest recorded before
and after the read was unchanged.

## Objective

Reconstruct the client's core behaviour:

1. A torrent session engine: metainfo parsing, tracker communication, peer wire
   subset, piece scheduling and verification.
2. Local persistence of session state and per-torrent resume data.
3. A documented JSON/HTTP control surface equivalent in spirit to the Web UI API.
4. A minimal UI or CLI that drives the engine.

## Non-goals

- Full protocol parity (DHT, PEX, uTP, encryption, RSS, search plugins) in the
  first milestone.
- Byte-identical behaviour.
- Committing the shipped PDB, binaries, or any extracted proprietary data.

## Method and evidence policy

Static-first: PE metadata, directory/marker inspection, Ghidra 12.1.4 headless
import, function discovery, string and cross-reference analysis. The target is
**not executed** during analysis; dynamic instrumentation (Frida) is used only
if separately authorised.

Evidence tags used in `docs/`: **Verified**, **Deduction**, **Hypothesis**,
**Unknown**.

## Technology baseline (verified markers)

| Fact | Evidence |
|---|---|
| Qt-based application | `qt.conf` present in install directory |
| Qt statically linked | No `Qt6Core.dll` beside the executable; single 43 MB image (deduction) |
| libtorrent linked in | Deduction from image size and product behaviour; to be confirmed by import/string analysis |
| Large binary | 43.3 MB — expect long Ghidra auto-analysis and noisy symbol recovery |

The bundled `qbittorrent.pdb` is a potential external asset, but the analysis
toolchain in use (REA/Ghidra) does not admit a caller-supplied PDB companion, so
it is **not** relied upon. It is referenced here only as an observed artifact and
must never be redistributed.

## Repository layout

```
README.md          this file
LICENSE            Apache-2.0
ROADMAP.md         phased plan and exit criteria
docs/              evidence, architecture notes, unknowns
src/               implementation (added in Phase 2)
tests/             automated tests (added in Phase 2)
tools/             analysis helper scripts (no binaries)
```

## Status

Phase 0 — scope, methodology and verified evidence baseline. No code yet.

## Licence

Apache-2.0. See `LICENSE`. The licence covers original work in this repository
only; it grants no rights to the original qBittorrent software.
