# Releases

No official NebulaOS release has been cut yet — this repository was created
as part of the 2026-08-14 repository canonicalization mission, ahead of the
first actual release. No version number is assigned here; inventing one
would misrepresent release history that doesn't exist yet.

## Golden baseline vs. current canonical source vs. current build environment

Three different things; release wording should never blur them.

- **Historical physical golden baseline**: `nebulaos-canonical-baseline-2026-08-14-prtouch-qualified`
  (`NebulaOS-firmware` commit `7328ef9`) — what was running on the reference printer before the
  Final Closure mission (2026-08-15), live-verified on real hardware at the time.
- **Current canonical source/build environment** (`NebulaOS-firmware` `main`, commit `9c0a811` at
  time of writing): descends from that baseline, adds the repository-canonicalization work, a real
  GuppyScreen config/theme fix (`b15ad7f`, read-only-rootfs config loading), and the Final Closure
  mission's unified build environment (`ghcr.io/coreflake1/nebulaos-build`, replacing the old
  `pellcorp/k1-bash-build` + `ghcr.io/coreflake1/guppydev` nested containers — see
  `NebulaOS-firmware`'s own `docs/NEBULAOS_BUILD_ENVIRONMENT.md`).
- **Current physical qualification state**: this exact `main` state **has now been physically
  qualified** on the reference printer (2026-08-15) — boot, WiFi, Moonraker, Mainsail,
  Klipper/MCU connection, GuppyScreen (including the `b15ad7f` fix's config/theme persistence
  across a real flash), camera, `G28` homing, PRTouch (real load-cell touch data), and
  `Z_OFFSET_CALIBRATION` all confirmed working, most via direct API/log verification rather than
  visual assumption. **Not explicitly exercised**: pause/resume/cancel workflow, a full
  representative print — noted honestly rather than claimed.
- **Reproducibility**: the unified build environment was verified two ways before promotion — a
  fresh-clone build compared against the frozen prior build
  (`SEMANTICALLY_IDENTICAL_WITH_NONDETERMINISTIC_METADATA` — no unexplained product differences),
  then a second, genuinely independent fresh-clone repeat build compared against the first
  (`SEMANTICALLY_IDENTICAL_WITH_KNOWN_NONDETERMINISM`). See `NebulaOS-firmware`'s own Phase 11 and
  Final Closure reports for the full evidence chain.

When a first version is officially assigned, its release manifest (see `releases/RELEASE_TEMPLATE.md`)
should state plainly which of these states it was actually built from and whether it was physically
qualified on hardware, not just clean-build-verified.

## Current identity (not yet a numbered release)

Recorded here as the provenance record for `main`'s current state, pending a first version being
officially assigned:

| Field | Value |
|---|---|
| `NebulaOS-firmware` commit | `9c0a811` |
| Build image | `ghcr.io/coreflake1/nebulaos-build@sha256:a6ba57c69fa1ea630b037a1d1f55cf0c044a7f5a403bde9b155ea54bca1cceba` |
| Qualified baseline tag | `nebulaos-canonical-baseline-2026-08-14-prtouch-qualified` |
| `NebulaOS-kernel` commit | `295b7101d751fd888ae39e6f1746a4a940664a5f` |
| `NebulaOS-klipper` commit | `9ccb2e5d2da1c94694d277a605e7e0144c5d6884` |
| `NebulaOS-guppyscreen` commit | `b15ad7f6cce9c40b61c0e5f678e3895b989deb13` |
| xImage SHA256 | `eb75c5b2e7b2e4d122db618628d07eda91553e66437b5986187506e141e61ba4` |
| rootfs.ext2 SHA256 | `e6e5e2431dbbe6cf301d96920bffa5ed084c82dbf97aeb2bb239e1070529123d` |
| rootfs.squashfs SHA256 | `bf965a4dbcea530d7fe69a6afb62caf9484e0a2c87e6f82d02d521e3a580a297` |
| Reproducibility | `SEMANTICALLY_IDENTICAL_WITH_KNOWN_NONDETERMINISM` |
| Hardware qualification | PASS (scope noted above) |

These hashes are from the Final Closure mission's own verified repeat build - the same artifacts
that were physically flashed and qualified above, not a separate unverified build.

## Release manifest schema

Every release gets one file under `releases/`, named `vX.Y.Z.md`, following
`releases/RELEASE_TEMPLATE.md`. It records, at minimum:

- NebulaOS release version
- `NebulaOS-firmware` commit/tag
- `NebulaOS-kernel` commit + the 8 accepted kernel variant identities, in order
- `NebulaOS-klipper` commit/tag
- `NebulaOS-guppyscreen` commit/tag
- rootfs image SHA256
- xImage (kernel image) SHA256
- build-manifest identity (from `NebulaOS-firmware`'s own `build-manifest.txt`)
- kernel.config identity
- build timestamp
- build provenance (who/what built it, from a fresh clone or not)

This is deliberately the same set of fields `NebulaOS-firmware`'s own
`manifests/dependencies.conf` already pins on the build side — a release
manifest here is a *record* of what was actually pinned and built, not a
second source of truth for what to pin.
