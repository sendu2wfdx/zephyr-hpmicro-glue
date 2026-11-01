# Maintenance policy

This repository carries reusable HPMicro SoC integration and Zephyr drivers.
Board wiring, pin-route choices, and board-specific conflict checks remain in
the corresponding board repository.

## Remotes and branches

- `upstream`: `https://github.com/hpmicro/zephyr_sdk_glue.git`
- `origin`: `https://github.com/sendu2wfdx/zephyr-hpmicro-glue.git`
- `stable-3.7`: production branch based on Zephyr 3.7 LTS and HPM SDK 1.11.0
- `next-4.5`: migration branch for Zephyr 4.5 and a newer validated HPM SDK

The stable branch must stay buildable for the EmbedFire HPM6750 board. Upstream
changes are merged deliberately and verified before the manifest revision is
advanced.

## Code ownership boundary

The glue repository owns:

- HPMicro SoC devicetree nodes and bindings;
- reusable controller drivers and HPM SDK adaptation;
- common clock, interrupt, DMA, cache, and memory integration;
- compatibility fixes that apply to more than one HPMicro board;
- samples and tests for reusable drivers.

The board repository owns:

- physical pin routing and connector descriptions;
- enabled-by-default onboard devices;
- optional pin-route snippets and mutual-exclusion checks;
- board-specific reset, power, backlight, and card-detect signals.

## Driver roadmap

Work is introduced in small, independently testable changes:

1. RTC through the Zephyr RTC API.
2. GPTMR through the Zephyr counter API.
3. ACMP through the comparator API, with a documented 3.7 compatibility layer.
4. HPM I2S controller with DMA support.
5. WM8960 through the Zephyr audio codec API.
6. DAC support after the HPM6750 board routes and API requirements are verified.

Every driver change should include devicetree bindings, Kconfig/CMake wiring,
at least one build test, and a hardware-validation note when hardware testing is
not yet available.

## Current version matrix

| Branch | Zephyr | HPM SDK | Status |
| --- | --- | --- | --- |
| `stable-3.7` | 3.7 LTS (`zsg_v0.7.0`) | 1.11.0 | Supported baseline |
| `next-4.5` | 4.5 | To be validated | Planned migration |

Do not update only the manifest revision when migrating Zephyr. Driver APIs,
devicetree bindings, the HPM Zephyr fork patches, and HPM SDK compatibility must
be reviewed and tested together.
