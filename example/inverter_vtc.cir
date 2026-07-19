# Examples

Ready-to-run netlists using the flat SKY130 model. Both work in **LTspice** and
**ngspice**.

| File | What it shows |
|------|---------------|
| `idvd_nmos.cir` | NMOS Id–Vd family (Vgs = 0.6 … 1.8 V) |
| `inverter_vtc.cir` | CMOS inverter DC transfer curve (VTC) |

## Run in LTspice
1. `File > Open` the `.cir` file (set file type to "All Files" if needed).
2. `Simulate > Run` (or the Run button).
3. Add a trace: `Id(M1)` for the Id–Vd example, `V(out)` for the inverter.

> The `.include ../sky130_ptm_style.lib` path assumes you run from the
> `examples/` folder. If LTspice cannot find it, edit the path to an absolute
> path or copy `sky130_ptm_style.lib` next to the netlist.

## Run in ngspice
```
cd examples
ngspice idvd_nmos.cir
```
Then, at the prompt: `run` and `plot -i(Vd)` (Id–Vd) or `plot v(out)` (inverter).
A ready-made `.control` block is included but commented out — uncomment it to
get automatic plotting in ngspice batch mode.

## Reminder
The model is frozen at **L = 0.15 µm, W = 1.0 µm**. These examples stay near that
geometry on purpose. See the top-level `README.md` for the L/W accuracy tables.
