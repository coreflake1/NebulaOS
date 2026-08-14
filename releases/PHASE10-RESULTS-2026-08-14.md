# NebulaOS — Phase 10 fresh-clone reproducibility results (not yet an official release)

**This is a verification record, not a release.** No version number is assigned — see `RELEASES.md`
for why. All fields below are now filled with real values from a completed Phase 9 fresh-clone
build and Phase 10 golden-printer comparison (2026-08-14). Do not copy this to a real `vX.Y.Z.md`
without an explicit decision to cut a release from this exact source state.

## Component identity

| Field | Value |
|---|---|
| NebulaOS release version | UNASSIGNED |
| NebulaOS-firmware commit/tag | `63dec1f0556a6861b0838dfbb03da0e8cd416d08` (main) |
| NebulaOS-kernel commit (openke branch) | `295b7101d751fd888ae39e6f1746a4a940664a5f` |
| Kernel variant 1 — preempt-variant.sh (R1) | applied, verified: `CONFIG_PREEMPT_RT=y` |
| Kernel variant 2 — wifi-sdio-variant.sh (W3) | applied, verified: SDIO IRQ/highspeed caps present |
| Kernel variant 3 — display-vsync-variant.sh (V1) | applied, verified: `CONFIG_FB_INGENIC_PAN_VSYNC_GATE=y` |
| Kernel variant 4 — pinctrl-ownership-fix-variant.sh (FIX1) | applied |
| Kernel variant 5 — backlight-final-controller-variant.sh (FINAL1) | applied, verified: `CONFIG_NEBULAOS_BACKLIGHT_FINAL_CONTROLLER=y` + DT node present |
| Kernel variant 6 — pwm-state-readback-variant.sh (GETSTATE1) | applied, verified: `CONFIG_PWM_INGENIC_V2_GET_STATE=y` |
| Kernel variant 7 — touch-final-qualification-variant.sh (FINALQUAL1) | applied, verified: `CONFIG_TOUCHSCREEN_NS2009_FINAL_QUALIFICATION=y` |
| Kernel variant 8 — wifi-roamoff-disable-variant.sh (ROAMOFF1) | applied, verified: patch present in source tree |
| NebulaOS-klipper commit/tag | `9ccb2e5d2da1c94694d277a605e7e0144c5d6884` (master, KLIPPER_PIN, clean) |
| NebulaOS-guppyscreen commit/tag | `b15ad7f6cce9c40b61c0e5f678e3895b989deb13` (main, GUPPYSCREEN_PIN) |

**Golden vs. canonical distinction (see `RELEASES.md`):** the physically-qualified golden baseline
(`nebulaos-canonical-baseline-2026-08-14-prtouch-qualified`, firmware commit `7328ef9`) predates
this canonicalization mission's own commits. The identity above is the *current canonical source*
— confirmed clean-build-verified below, **not yet re-verified on real hardware**.

## Build artifacts (from the completed fresh-clone build)

| Field | Value |
|---|---|
| `rootfs.ext2` SHA256 | `b90a96bab8b1eff23275ddd1ec6c4493d4addec7d860efb43acdbdf2ce3e78ed` |
| `rootfs.squashfs` SHA256 | `805a3ec0a4a16de34dcb2ffed454c4ad95f83ef17ce3b78adae9238666cf314a` (120,008,704 bytes) |
| `xImage` (kernel image) SHA256 | `a8b5e6c441c27705d99b82b8cfda2610f613ae85103465fa5e699d24afe10af0` (5,496,896 bytes) |
| `build-manifest.txt` | present, all component pins recorded, matches `manifests/dependencies.conf` exactly |
| `kernel.config` SHA256 | `89086a7c71e80f79f91a93220876ed39aedd2695b8db3cb2f04192f0ad8d3083` |
| GuppyScreen binary SHA256 | `269461bf67cf50cbe411c9b64ea1621852e0a95bbb6f5b93aae27eef88b61dd0` (6,229,416 bytes) |
| WiFi firmware SHA256 | `82ed67a211877efa47aff4aab83d6d2d1ccf3d5d0f5c396df97f292ade01de9e` — **BYTE_IDENTICAL to golden printer** |
| WiFi CLM SHA256 | `1dbe1a396b68786bb189b7c255318ae546fd2e9d15f70ccc8ecbdc52b6cd4c47` — **BYTE_IDENTICAL to golden printer** |
| Klipper `klippy/extras/{z_compensate,prtouch_probe,prtouch_mcu,prtouch_v2}.py` | **BYTE_IDENTICAL to golden printer**, all 4 files |

## Build provenance

| Field | Value |
|---|---|
| Build timestamp | started 2026-08-14 20:56:01 UTC, completed ~2026-08-14 21:43 UTC (~47 min) |
| Built from a fresh clone (yes/no) | yes — `~/Documents/nebulaos-canonical-repro-test-20260814`, cloned directly from `coreflake1/NebulaOS-firmware`, no reuse of any existing worktree |
| Built by | automated (`./build.sh`), this canonicalization mission |
| Reproducibility status | see below |

## Phase 10 result

**Formally: MISMATCH** (not byte-identical against the golden-qualified printer, and one of the
differences is real source content, not just build metadata) — **but every single difference found
is either expected-by-design or a pre-existing tooling bug, with zero unexplained deltas**:

1. **GuppyScreen binary differs** (`269461bf...` vs. the golden printer's `00890a63...`) — expected
   and correct: current canonical source includes commit `b15ad7f` (the config/theme ifstream fix),
   which post-dates the golden baseline. Binary size is nearly identical (6,229,416 vs 6,229,424
   bytes, an 8-byte difference), consistent with the underlying 4-line source change, not something
   unrelated. This is the intentional delta `RELEASES.md` already documents.
2. **The build's own `build-qualified-baseline.sh` self-check failed** one assertion:
   "kernel.config differs from pinned baseline tag `nebulaos-display-baseline-vsync-pwm-sleep-2026-08-03`".
   Root cause found: `scripts/build/assert-baseline-config.sh:161` and
   `scripts/build/baseline-difference-gate.sh:26` both hardcode that August 3rd tag as their
   comparison reference, and were never updated as later kernel variants (ROAMOFF1, backlight/PWM/
   touch final-qualification, the entire PRTouch baseline) were accepted. Every individual Kconfig
   check the same run performed (`PREEMPT_RT`, `HZ`, backlight controller, touch qualification, PWM
   readback, DISPLAY-V1, W3, ROAMOFF1) **passed**; DTS and `buildroot.config` were confirmed
   byte-identical to that same old tag. This is a real, pre-existing bug in the project's own
   verification tooling (comparing against a stale reference), not evidence of an actual build
   regression. **Not fixed as part of this comparison** — flagged for a separate, deliberate fix.
3. **A transient network hiccup**, self-corrected: `squashfs-4.6.1.tar.gz` failed its SHA256 check
   on the first download attempt ("Incomplete download, or man-in-the-middle (MITM) attack" —
   Buildroot's own hash-verification working as designed), retried automatically per Buildroot's own
   `wget -t 3`, and the retry matched the expected hash exactly. Zero impact on final artifacts.

Everything with no reason to differ **did not differ**: kernel commit, Klipper commit, Moonraker
commit, WiFi firmware + CLM (byte-identical), and all four checked `klippy_extras/` PRTouch/
Z-compensate source files (byte-identical) — closing the loop on this project's own previously-
documented "klippy_extras mirror vs. real build input" risk.

## Release notes

Not written — no release is being cut from this. This file exists to have the verification record
on file, not to announce anything.
