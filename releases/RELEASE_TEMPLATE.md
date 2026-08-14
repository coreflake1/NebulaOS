# NebulaOS vX.Y.Z

Copy this file to `releases/vX.Y.Z.md` for each real release and fill in
every field. Leave nothing blank — use `UNKNOWN` explicitly rather than
omitting a field, so a reader can tell "not recorded" apart from "empty."

## Component identity

| Field | Value |
|---|---|
| NebulaOS release version | |
| NebulaOS-firmware commit/tag | |
| NebulaOS-kernel commit (openke branch) | |
| Kernel variant 1 — preempt-variant.sh (R1) | |
| Kernel variant 2 — wifi-sdio-variant.sh (W3) | |
| Kernel variant 3 — display-vsync-variant.sh (V1) | |
| Kernel variant 4 — pinctrl-ownership-fix-variant.sh (FIX1) | |
| Kernel variant 5 — backlight-final-controller-variant.sh (FINAL1) | |
| Kernel variant 6 — pwm-state-readback-variant.sh (GETSTATE1) | |
| Kernel variant 7 — touch-final-qualification-variant.sh (FINALQUAL1) | |
| Kernel variant 8 — wifi-roamoff-disable-variant.sh (ROAMOFF1) | |
| NebulaOS-klipper commit/tag | |
| NebulaOS-guppyscreen commit/tag | |

## Build artifacts

| Field | Value |
|---|---|
| rootfs image SHA256 | |
| uImage (kernel image) SHA256 | |
| build-manifest.txt identity | |
| kernel.config identity | |

## Build provenance

| Field | Value |
|---|---|
| Build timestamp | |
| Built from a fresh clone (yes/no) | |
| Built by | |
| Reproducibility status | one of: BYTE_IDENTICAL / SEMANTICALLY_IDENTICAL_WITH_NONDETERMINISTIC_METADATA / MISMATCH / NOT_FULLY_VERIFIED |

## Release notes

<!-- What changed since the last release, in user-facing terms. -->
