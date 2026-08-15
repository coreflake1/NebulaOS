# NebulaOS

NebulaOS is a custom Linux + Klipper stack for the Creality Ender-3 V3 KE — a real kernel, real
Klipper, a proper touchscreen UI, replacing the stock firmware end to end.

This repo is the front door for the project. It's where releases, build provenance, and general
docs live — but it's not a source tree itself. The actual code lives in four other repos:

| Component | Repository | What's there |
|---|---|---|
| Integration / build | [`NebulaOS-firmware`](https://github.com/coreflake1/NebulaOS-firmware) | Pins every component below to an exact commit, runs the build, owns the build scripts |
| Kernel | [`NebulaOS-kernel`](https://github.com/coreflake1/NebulaOS-kernel) | X2000 kernel fork (`openke` branch), plus 8 accepted build-time variant patches tracked in `NebulaOS-firmware` |
| Klipper | [`NebulaOS-klipper`](https://github.com/coreflake1/NebulaOS-klipper) | Klipper fork; `klippy/extras/` is where PRTouch and Z-compensation live |
| GUI | [`NebulaOS-guppyscreen`](https://github.com/coreflake1/NebulaOS-guppyscreen) | GuppyScreen fork for the touch UI |

If you're after source code, config, or build scripts, you want one of those four. This repo only
holds release artifacts and the docs you'd need to install or update them.

## Installing / updating

There isn't a polished, one-click consumer installer yet — but you don't need one to actually run
NebulaOS. The first hardware-qualified developer nightly is up under
[Releases](https://github.com/coreflake1/NebulaOS/releases/tag/nightly-2026-08-15): the exact build
that went through real hardware testing (boot, Wi-Fi, Klipper/MCU, Moonraker/Mainsail, GuppyScreen,
camera, homing, PRTouch, Z offset calibration — all passed).

It's a developer build, not a "just works" download — read the release notes and the install docs
linked below before flashing it.

Once we start cutting official releases, each one will document the exact component revisions it
was built from (see `releases/RELEASE_TEMPLATE.md`), so a build is always traceable back to the
four repos above.

## Developer documentation

This repo doesn't keep its own copy of the install/build/recovery docs — `NebulaOS-firmware` is the
canonical source, and this repo just links to it:

- [`NebulaOS-firmware` wiki](https://github.com/coreflake1/NebulaOS-firmware/wiki) — good starting point
- [Build From Source](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/BUILD_FROM_SOURCE.md)
- [A/B Slot Model](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/A_B_SLOT_MODEL.md)
- [Developer Install From Stock](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_INSTALL_FROM_STOCK.md)
- [Developer Update](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_UPDATE.md)
- [Developer Recovery](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/DEVELOPER_RECOVERY.md)
- [Build Provenance](https://github.com/coreflake1/NebulaOS-firmware/blob/main/docs/BUILD_PROVENANCE.md) — how to verify what actually produced a given release

These are developer/advanced-testing docs — they'll have you on SSH, root, and raw partitions. This
isn't a supported consumer installer yet, and the docs don't pretend otherwise.

## How the pieces fit together

```
NebulaOS-kernel  ─┐
NebulaOS-klipper ─┼─► NebulaOS-firmware ─► produces artifacts ─► NebulaOS (this repo) ─► publishes releases
NebulaOS-guppyscreen ┘
```

`NebulaOS-firmware` is the only repo that pins and cross-checks all three component repos. This
repo just consumes its finished output — it never gets its own copy of the kernel/Klipper/GUI
source, and it's never going to turn into a second integration tree.
