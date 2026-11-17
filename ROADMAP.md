# Roadmap - re-qbittorrent

## Phase 0 - Scope and baseline (complete)
Deliverables: README, licence, verified evidence baseline, ignore rules.
Exit criteria: target identity and digest recorded; scope and non-goals agreed.

## Phase 1 - Static architecture map
Ghidra import of the 43 MB image (expect long analysis); string and import
triage; locate the session/engine boundary and the HTTP/JSON control surface.
Deliverable: `docs/architecture.md`.
Exit criteria: documented evidence for engine, UI and API layers.

## Phase 2 - Metainfo and tracker
Bencode parser, `.torrent` model, HTTP tracker announce, peer list handling.
Exit criteria: successful announce against a local test tracker.

## Phase 3 - Peer wire and storage
Handshake, message framing, piece requests, SHA-1 verification, file mapping.
Exit criteria: download and verify a multi-piece torrent from a local peer.

## Phase 4 - Persistence and resume
Settings, fast-resume data, torrent queue state.

## Phase 5 - Control API and UI
JSON/HTTP endpoints mirroring the documented Web UI subset; minimal UI or CLI.

## Phase 6 - Hardening and documentation
Protocol fuzzing, resource limits, final write-up with open questions.
