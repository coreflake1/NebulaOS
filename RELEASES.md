# Releases

No official NebulaOS release has been cut yet — this repository was created
as part of the 2026-08-14 repository canonicalization mission, ahead of the
first actual release. No version number is assigned here; inventing one
would misrepresent release history that doesn't exist yet.

## What's ready to become the first release

`NebulaOS-firmware`'s `main` branch currently sits at the tag
[`nebulaos-canonical-baseline-2026-08-14-prtouch-qualified`](https://github.com/coreflake1/NebulaOS-firmware/releases/tag/nebulaos-canonical-baseline-2026-08-14-prtouch-qualified)
(commit `63dec1f...`, descended from `7328ef9`), which is live-qualified on
real hardware. When a first version is officially assigned, its release
manifest (see `releases/RELEASE_TEMPLATE.md`) should reference that
baseline's exact component identities.

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
