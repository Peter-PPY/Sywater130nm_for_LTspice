Skywater130nm_for_LTspice
Copyright 2026 VirtuChip.AI

This product includes device models derived from the SkyWater Open Source PDK
(https://github.com/google/skywater-pdk), Copyright The SkyWater PDK Authors,
licensed under the Apache License, Version 2.0.

Modification statement:
The original SKY130 primitive device models (sky130_fd_pr__nfet_01v8 and
sky130_fd_pr__pfet_01v8) are distributed as .subckt wrappers around
micro-binned, corner-based, statistically-parameterised BSIM4 cards. In this
derived work those models were COLLAPSED into flat, single-geometry BSIM4
(level 54) .model cards:
  - corner: tt (typical)
  - statistics / Monte-Carlo: disabled (mismatch and process variation = 0)
  - frozen geometry: L = 0.15 um, W = 1.0 um (device bin 6)
See README.md for validation results and usage limitations.

You may obtain a copy of the Apache License, Version 2.0, in the LICENSE file
or at http://www.apache.org/licenses/LICENSE-2.0
