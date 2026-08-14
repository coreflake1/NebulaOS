# NebulaOS — draft release manifest (PENDING, not yet published)

**This is a prepared draft, not a release.** No version number is assigned — see `RELEASES.md` for
why. Filled in with everything known from the current repository-canonicalization mission; the
build-artifact and provenance fields are intentionally left `PENDING_PHASE_10` because the fresh
clean-room build (Phase 9) has not finished and the golden-printer comparison (Phase 10) has not
run yet. Do not copy this to a real `vX.Y.Z.md` until those fields are filled in with real values.

## Component identity

| Field | Value |
|---|---|
| NebulaOS release version | UNASSIGNED |
| NebulaOS-firmware commit/tag | `63dec1f0556a6861b0838dfbb03da0e8cd416d08` (main, at Phase 9 build start) |
| NebulaOS-kernel commit (openke branch) | `295b7101d751fd888ae39e6f1746a4a940664a5f` |
| Kernel variant 1 — preempt-variant.sh (R1) | applied, see NebulaOS-firmware `scripts/build/preempt-variant.sh` |
| Kernel variant 2 — wifi-sdio-variant.sh (W3) | applied |
| Kernel variant 3 — display-vsync-variant.sh (V1) | applied |
| Kernel variant 4 — pinctrl-ownership-fix-variant.sh (FIX1) | applied |
| Kernel variant 5 — backlight-final-controller-variant.sh (FINAL1) | applied |
| Kernel variant 6 — pwm-state-readback-variant.sh (GETSTATE1) | applied |
| Kernel variant 7 — touch-final-qualification-variant.sh (FINALQUAL1) | applied |
| Kernel variant 8 — wifi-roamoff-disable-variant.sh (ROAMOFF1) | applied |
| NebulaOS-klipper commit/tag | `9ccb2e5d2da1c94694d277a605e7e0144c5d6884` (master, KLIPPER_PIN) |
| NebulaOS-guppyscreen commit/tag | `b15ad7f6cce9c40b61c0e5f678e3895b989deb13` (main, GUPPYSCREEN_PIN) |

**Golden vs. canonical distinction (see `RELEASES.md`):** the physically-qualified golden baseline
(`nebulaos-canonical-baseline-2026-08-14-prtouch-qualified`, firmware commit `7328ef9`) predates
this canonicalization mission's own commits. The identity above is the *current canonical source*,
not yet re-verified on real hardware.

## Build artifacts

| Field | Value |
|---|---|
| rootfs image (`rootfs.ext2`) SHA256 | PENDING_PHASE_10 |
| uImage (kernel image) SHA256 | PENDING_PHASE_10 |
| build-manifest.txt identity | PENDING_PHASE_10 |
| kernel.config identity | PENDING_PHASE_10 |

## Build provenance

| Field | Value |
|---|---|
| Build timestamp | Phase 9 build started 2026-08-14 20:56:01 UTC |
| Built from a fresh clone (yes/no) | yes — `~/Documents/nebulaos-canonical-repro-test-20260814`, cloned directly from `coreflake1/NebulaOS-firmware`, no reuse of any existing worktree |
| Built by | automated (`build.sh`), this canonicalization mission |
| Reproducibility status | PENDING_PHASE_10 |

## Release notes

Not written — this draft exists to have the manifest shape and known identities on record ahead of
Phase 10, not to announce a release.
