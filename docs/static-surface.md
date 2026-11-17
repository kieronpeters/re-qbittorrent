# Static surface baseline — qbittorrent.exe

**Target:** `C:\Program Files\qBittorrent\qbittorrent.exe`
**Version:** 5.2.3 · **Publisher:** The qBittorrent Project
**SHA-256:** `f69360ae8545a64f4fc84fb6bacef03d77a6aa0793a4c14d4a28651ca26a27d1`
**Captured:** 2026-11-17 · **Tooling:** GNU objdump 2.47 via REA `inspect_with_objdump`

## Verified section map

Image base `0x140000000`. Eight sections, `pei-x86-64`.

| # | Name | Virtual size | VMA | File offset | Align | Flags |
|---|---|---|---|---|---|---|
| 0 | `.text` | `0x016A4928` (23,741,736) | `0x140001000` | `0x400` | 16 | CONTENTS, ALLOC, LOAD, READONLY, CODE |
| 1 | `.rdata` | `0x01110706` (17,893,126) | `0x1416A6000` | `0x16A4E00` | 16 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 2 | `.data` | `0x0006BC00` (441,344) | `0x1427B7000` | `0x27B5600` | 16 | CONTENTS, ALLOC, LOAD, DATA |
| 3 | `.pdata` | `0x000CE520` (845,088) | `0x142847000` | `0x2821200` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 4 | `_RDATA` | `0x0000015C` (348) | `0x142916000` | `0x28EF800` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 5 | `.qtmimed` | `0x0000B34D` (45,901) | `0x142917000` | `0x28EFA00` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA, **SHARED** |
| 6 | `.rsrc` | `0x000254E8` (152,808) | `0x142923000` | `0x28FAE00` | 4 | CONTENTS, ALLOC, LOAD, READONLY, DATA |
| 7 | `.reloc` | `0x0002BF3C` (180,028) | `0x142949000` | `0x2920400` | 4 | CONTENTS, LOAD, READONLY, DATA |

The post-read digest matched the pre-read digest (`artifact_changed: false`).

## Observations

- `.text` is 22.6 MiB and `.rdata` 17.1 MiB — a monolithic, statically linked
  image, not a thin loader over shared Qt DLLs.
- A `.qtmimed` section is present and marked `SHARED`. This is Qt's compiled
  MIME-type database embedded in the image.
- A 348-byte, uppercase `_RDATA` section exists separately from `.rdata`.
- Total virtual footprint (~43 MiB) matches the on-disk image size.

## Deductions

- **Qt is statically linked** — the `.qtmimed` section is a Qt-generated
  artifact and no `Qt6Core.dll` is deployed beside the executable. This upgrades
  the README's "static Qt" statement from weak to strong evidence.
- libtorrent is very likely compiled into the same image; the absence of
  external qBittorrent libraries in the install directory and the image size
  support this, but the import table has not yet been read.
- `_RDATA` is a non-standard, read-only section whose purpose is **unknown**;
  candidates are linker/CRT metadata or toolchain-generated data.

## Unknowns

- Import table, exported Qt metaobjects and HTTP-API registration sites.
- Whether the shipped `qbittorrent.pdb` matches this exact build.
- Function-level architecture: session engine boundary, QWidget composition and
  the Web UI controller classes remain unmapped.

## Provenance

Sections and sizes come directly from `objdump -h` output returned by REA
(evidence `ev_5f9404b5…49b`). No code was executed; the file was read only.
