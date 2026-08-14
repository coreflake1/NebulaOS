# Releases

No official NebulaOS release has been cut yet — this repository was created
as part of the 2026-08-14 repository canonicalization mission, ahead of the
first actual release. No version number is assigned here; inventing one
would misrepresent release history that doesn't exist yet.

## Golden baseline vs. current canonical source

These are two different things, and release wording should never blur them:

- **`nebulaos-canonical-baseline-2026-08-14-prtouch-qualified`** (`NebulaOS-firmware` commit
  `7328ef9`) is the **physically qualified golden baseline** — this is what's actually running on
  the reference printer, live-verified on real hardware.
- **Current canonical source** (`NebulaOS-firmware` `main`, commit `63dec1f...` at time of writing)
  descends from that baseline and additionally contains repository-canonicalization work plus a real
  GuppyScreen config/theme fix (`b15ad7f`, read-only-rootfs config loading) that was **not** part of
  what was physically qualified. That delta is undergoing clean-build/reproducibility verification —
  see `NebulaOS-firmware`'s own build log for the current status — and has not yet been re-verified
  on real hardware.

When a first version is officially assigned, its release manifest (see `releases/RELEASE_TEMPLATE.md`)
should state plainly which of these two states it was actually built from and whether it was
physically qualified on hardware, not just clean-build-verified.

## Release manifest schema

Every release gets one file under `releases/`, named `vX.Y.Z.md`, following
`releases/RELEASE_TEMPLATE.md`. It records, at minimum:

- NebulaOS release version
- `NebulaOS-firmware` commit/tag
- `NebulaOS-kernel` commit + the 8 accepted kernel variant identities, in order
- `NebulaOS-klipper` commit/tag
- `NebulaOS-guppyscreen` commit/tag
- rootfs image SHA256
- uImage (kernel image) SHA256
- build-manifest identity (from `NebulaOS-firmware`'s own `build-manifest.txt`)
- kernel.config identity
- build timestamp
- build provenance (who/what built it, from a fresh clone or not)

This is deliberately the same set of fields `NebulaOS-firmware`'s own
`manifests/dependencies.conf` already pins on the build side — a release
manifest here is a *record* of what was actually pinned and built, not a
second source of truth for what to pin.
