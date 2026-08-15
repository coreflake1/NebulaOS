# NebulaOS

NebulaOS is a from-scratch custom OS/firmware for the Creality Ender-3 V3 KE,
replacing the stock kernel, Klipper, GuppyScreen, and boot/update tooling.

**This repository is the end-user release repository.** It holds production
firmware images, checksums, release notes, and install/update instructions —
it is **not** a development source tree. Component source lives in:

| Component | Repository | Role |
|---|---|---|
| Integration / build | [`NebulaOS-firmware`](https://github.com/coreflake1/NebulaOS-firmware) | Pins every component below by exact commit/tag, orchestrates the build, owns build scripts and overlay config. The sole integration authority. |
| Kernel | [`NebulaOS-kernel`](https://github.com/coreflake1/NebulaOS-kernel) | X2000 kernel fork (`openke` branch), every OpenKE kernel change as a real commit, plus 8 accepted build-time variant patches tracked in `NebulaOS-firmware`. |
| Klipper | [`NebulaOS-klipper`](https://github.com/coreflake1/NebulaOS-klipper) | Klipper fork; `klippy/extras/` owns PRTouch, Z-compensation, and related NebulaOS-specific functionality. |
| GUI | [`NebulaOS-guppyscreen`](https://github.com/coreflake1/NebulaOS-guppyscreen) | GuppyScreen fork for the NebulaOS touch UI. |

If you're looking for source code, config, or build scripts, you want one of
those four repos. This one only ever contains release *artifacts* and the
documentation a user needs to install or update them.

## Installing / updating

Releases will be published under [Releases](../../releases) once the first
official version is cut. Each release documents the exact component
revisions it was built from (see `releases/RELEASE_TEMPLATE.md`) so a build
is always fully reproducible from the four repos above.

No official release has been cut here yet — the procedures that exist and are actually exercised
today are the developer/advanced-testing ones in `NebulaOS-firmware` (see below). Nothing in this
repo or that one is a supported consumer installer.

## Developer documentation

This repo doesn't host its own copy of install/build/recovery procedures — `NebulaOS-firmware` is
the canonical source for those, and this repo links back to it rather than duplicating:

- [`NebulaOS-firmware` wiki](https://github.com/coreflake1/NebulaOS-firmware/wiki) — navigation entry point
- [Build From Source](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/BUILD_FROM_SOURCE.md)
- [A/B Slot Model](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/A_B_SLOT_MODEL.md)
- [Developer Install From Stock](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_INSTALL_FROM_STOCK.md)
- [Developer Update](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_UPDATE.md)
- [Developer Recovery](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_RECOVERY.md)
- [Build Provenance](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/BUILD_PROVENANCE.md) — how a given release artifact's origin can be verified

These are developer / advanced testing documentation: they expose raw firmware images, partitions,
A/B boot slots, and SSH/root access, and document the current development workflow rather than a
supported consumer installer.

## Dependency model

```
NebulaOS-kernel  ─┐
NebulaOS-klipper ─┼─► NebulaOS-firmware ─► produces artifacts ─► NebulaOS (this repo) ─► publishes releases
NebulaOS-guppyscreen ┘
```

`NebulaOS-firmware` is the only repo that pins and cross-verifies all three
component repos (`manifests/dependencies.conf`). This repo consumes its
finished, tagged output — it never gains its own copy of kernel/Klipper/GUI
source, and never becomes a second integration tree.
