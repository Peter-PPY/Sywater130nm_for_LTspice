# SKY130 1.8 V Flat BSIM4 Model for LTspice (PTM-style)

A single, flat `.model` card for the SkyWater **SKY130** 1.8 V core NMOS and
PMOS devices — distilled from the official open PDK into a PTM-style,
drop-in model you can use directly in **LTspice**, **ngspice**, or **HSPICE**
without the PDK's subcircuit / binning / statistical wrapper.

> **TL;DR** — Great for quick hand-analysis, teaching, and first-pass
> circuit sketches at the **minimum channel length (L ≈ 0.15 µm)**.
> **Not** a foundry sign-off model. Read the *Limitations* section before use.

---

## What this is

The real SKY130 device models (`sky130_fd_pr__nfet_01v8`, `pfet_01v8`) are
distributed as `.subckt` wrappers around **micro-binned**, **corner-based**,
**statistically-parameterised** BSIM4 cards. That is exactly what you want for
tape-out, but it is heavy for back-of-the-envelope work and does not drop into
LTspice as a plain model.

This library **collapses** that structure into one flat BSIM4 (level 54) card
per device:

| Setting            | Value                                         |
|--------------------|-----------------------------------------------|
| Corner             | `tt` (typical)                                |
| Statistics / MC    | **off** (mismatch and process variation = 0)  |
| Frozen geometry    | **L = 0.15 µm, W = 1.0 µm** (device bin 6)    |
| Model type         | BSIM4, `level = 54`, `version = 4.5`          |

Every binned expression, `*_mult`, and corner delta is resolved to a fixed
number at the nominal operating point, so the result is a static, portable
`.model` card.

**Model names**

- `sky130_nmos_130` — NMOS, `level=54`
- `sky130_pmos_130` — PMOS, `level=54`

---

## How to use

```spice
* --- include the library ---
.include sky130_ptm_style.lib
.option scale=1u          ; model is drawn in microns

* --- instantiate as ordinary BSIM4 devices (M-prefix, NOT X) ---
M1 nd ng ns nb sky130_nmos_130 L=0.15 W=1
M2 pd pg ps pb sky130_pmos_130 L=0.15 W=1
```

Notes:
- Call the devices with **`M`** (plain `.model`), not `X` — there is no
  subcircuit here.
- `.option scale=1u` is **required**: `W`/`L` are entered in microns.
- In **LTspice**, add the include as a SPICE directive
  (`.include sky130_ptm_style.lib`) or place the two `.model` cards directly on
  the schematic. LTspice maps `level=54` to BSIM4.

---

## Validation

Each flat card was checked against the **real SKY130 subcircuit** (the actual
binned PDK model, with the simulator performing its own bin selection and
expression evaluation) in **ngspice 42**, at the extraction geometry
`L = 0.15 µm, W = 1.0 µm`.

| Test (vs. real SKY130 subckt) | NMOS | PMOS |
|-------------------------------|------|------|
| Id–Vd sweep, max relative error (76 pts) | **0.00 %** | **0.00 %** |
| Id at Vgs = Vds = 1.8 V | 501.05 µA (exact) | 200.75 µA (exact) |
| Id–Vg incl. sub-threshold, max error | 0.47 %¹ | — |

¹ The only non-zero deviation is in deep sub-threshold (fA-level current),
where a tiny absolute difference is a large percentage; the on / near-threshold
region matches to all printed digits.

> **Important:** validation was performed in **ngspice**. LTspice ships its own
> BSIM4 implementation, whose defaults and equation details differ slightly, so
> results in LTspice may not be bit-identical. Please spot-check one Id–Vd curve
> in LTspice itself before relying on absolute numbers. LTspice's BSIM4 also has
> known quirks (e.g. it does not always echo Cgs/Cgd in the `.op` log); DC and
> transient behaviour is generally fine.

---

## Limitations (please read)

This is a **single-geometry snapshot**, not a scalable compact model. It is
faithful *at* the extraction point and degrades as you move away from it.

**Channel length (L) — sensitive.** The card is frozen at L = 0.15 µm. Because
the real device uses length binning that a flat card cannot reproduce, error
grows quickly with L (measured vs. the real subckt, W = 1 µm, Vgs = Vds = 1.8 V):

| L | 0.15 µm | 0.25 µm | 0.5 µm | 1 µm | 2 µm |
|---|---------|---------|--------|------|------|
| Error | 0 % | ~11 % | ~19 % | ~39 % | ~60 % |

→ **Keep L at (or very close to) 0.15 µm.** Small excursions to ~0.25 µm are
tolerable; longer devices need a card re-extracted for that length.

**Channel width (W) — asymmetric.** Widening is fine; narrowing is not
(L = 0.15 µm):

| W | 0.42 µm | 0.5 µm | 1 µm | 2 µm | 5 µm |
|---|---------|--------|------|------|------|
| Error | ~8.6 % | ~8.6 % | 0 % | ~0.1 % | ~1.1 % |

→ **W ≥ 1 µm scales cleanly (≤ ~1 %).** For **W < 1 µm** (e.g. minimum-width
devices in current mirrors) expect **~8–9 %** error from narrow-width effects
the frozen card cannot capture.

**Other constraints**

- **`tt` corner only** — no `ff` / `ss` / `fs` / `sf`.
- **No statistics** — no mismatch, no process Monte-Carlo.
- **No binning** — one card for all sizes (that is the whole point, and the
  source of the L/W errors above).
- Some subcircuit-level parasitics outside the intrinsic BSIM4 device are not
  reproduced.
- **Not for sign-off.** For verification, DRC-clean layout, or tape-out, use the
  full `sky130_fd_pr` PDK models with a supported simulator.

---

## Regenerating for other geometries / corners

The flattening is deterministic. A card can be re-extracted for a different bin
(e.g. a longer L for an analog gain stage, or a specific W), or for a different
process corner, which restores ~0 % accuracy *at that new operating point*.
If you need a scalable model that tracks L and W correctly, use the real binned
PDK subcircuit rather than a flat card.

---

## Source, license, and attribution

This library is a **derived work** of the SkyWater Open Source PDK.

- **Upstream source:** SkyWater `sky130_fd_pr` primitive device models
  — https://github.com/google/skywater-pdk
- **Upstream license:** **Apache License 2.0**
- Copyright © SkyWater Technology / Google LLC and PDK contributors.

Because the upstream models are Apache-2.0, redistribution of this derived work
must also comply with Apache-2.0. This repository therefore:

1. includes the full `LICENSE` (Apache-2.0) text,
2. retains this attribution notice, and
3. **states the change made:** the original binned/statistical `.subckt` models
   were collapsed to a flat, single-geometry `.model` card at the `tt` corner,
   L = 0.15 µm, W = 1.0 µm, with statistics disabled.

The extraction tooling and this documentation are provided by **VirtuChip.AI**.
No warranty of any kind; see *Limitations* and the Apache-2.0 disclaimer.

---

## Disclaimer

Provided **as-is** for education and preliminary design. It is **not** an
official SkyWater / foundry model and carries no accuracy guarantee for
tape-out. Always validate against the official PDK before committing silicon.
