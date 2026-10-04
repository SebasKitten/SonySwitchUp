# Rev C to Rev D: exact design changes

Source: the top-level project in the supplied `mipi_adapter_revC.zip`. The revision was saved as a separate `mipi_adapter_revD` project.

## Electrical changes

| Reference | Change |
|---|---|
| U1 | Retained TPS22917DBVR and its existing VBAT switching function, position, orientation, VIN/VOUT/ground pad assignments, and QOD-to-VOUT connection. Its ON pin now shares the named PANEL_EN net with U2. |
| C7 | Retained 10 nF / 50 V, footprint and position. Pin 1 remains on U1 CT. Changed pin 2 from GND to external VBAT_IN, correcting the capacitor connection to CT-to-VIN. |
| C8 | Added 1 uF / 50 V, 0603, LCSC C15849, between U1 VIN (VBAT_IN) and GND. |
| U2 | Added TPS22917DBVR, LCSC C2681320, in the same footprint as U1. Pin 1 = VCI_IN; pin 2 = GND; pin 3 = PANEL_EN; pin 4 = VCI_CT; pins 5 and 6 = VCI_PANEL. Pin 5 is tied to the output to enable quick output discharge. |
| R2 | Kept the existing footprint, position, orientation, host-side connection and manual/DNP status. Changed its panel-side net from VCI_PANEL to VCI_IN, so it supplies U2's input rather than bypassing U2. Changed its value text from `3V3 Jumper` to `VCI input jumper`. |
| C9 | Added 1 uF / 50 V, 0603, LCSC C15849, between U2 VIN (VCI_IN) and GND. |
| C10 | Added 10 nF / 50 V, 0603, LCSC C57112. Pin 1 connects to U2 CT; pin 2 connects to U2 VIN (VCI_IN). |
| C3, C4 | Retained their values (1 uF / 50 V and 100 nF / 50 V) and electrical connections between VCI_PANEL and GND. Repositioned them near U2's output and rerouted VCI distribution. |
| R5 | Retained its footprint, position, orientation, HOST_3V3_CAND connection and manual/DNP status. Its switched-side net now drives both ON pins. Renamed value `VBAT_EN_J` to `PANEL_EN_J`. |
| TP7 | Retained its footprint, position and electrical connection to the enable net. Renamed value `VBAT_EN` to `PANEL_EN`. The existing physical EN silkscreen label is retained. |

Power paths after the change:

```
External VBAT -> TP1 -> U1 -> VBAT_PANEL -> J2 pins 2-6
Switch 3.3 V -> R2 -> U2 -> VCI_PANEL -> J2 pin 30

Switch 3.3 V -> R5 -> PANEL_EN -> U1 ON and U2 ON
                         |
                        TP7
```

VBAT_IN is still supplied only through TP1. Neither VCI_IN nor host 3.3 V is connected to it. C7 and C10 each reference their respective switch's own input supply. The common enable does not impose a guaranteed relative power-up order between the independently supplied switches.

## PCB placement and routing

Positions use KiCad board coordinates in millimeters; angles follow KiCad's convention.

| Reference | Previous X, Y, angle | New X, Y, angle |
|---|---|---|
| U2 | New | 73.000, 70.500, 90 deg |
| C8 | New | 58.500, 65.300, 90 deg |
| C9 | New | 74.200, 67.300, 0 deg |
| C10 | New | 70.400, 70.900, 90 deg |
| C3 | 73.000, 68.000, 0 deg | 73.000, 74.500, 0 deg |
| C4 | 73.000, 71.000, 0 deg | 76.500, 74.500, 0 deg |

All other pre-existing footprints retain their original positions and orientations.

- Removed the 16 old VCI trace segments and replaced the direct feed with the U2 input/output routes.
- Removed three local ground-tail segments: the old C7 ground connection and the former C3/C4 ground tails. Removed the two ground vias at the previous C3/C4 positions.
- Added 37 trace segments and 11 through vias for the power changes: six power/control vias and five local ground vias. The new through vias use 0.4 mm pads and 0.2 mm drills.
- Added the shared-enable branch from U1's existing enable route to U2. The crossing of the existing reset routing uses B.Cu east of the DSI routes.
- Routed the two CT-to-VIN reference connections partly on B.Cu within their respective power areas.
- Set only U2's GND pad to a solid zone connection and added a nearby ground via; this avoids a starved thermal at its closely spaced pins.
- Refilled the existing ground zone to clear the new power copper. Its outline, layers, clearance and thermal settings are unchanged.
- Eight existing copper objects changed net names only: the original U1 VIN, ON and CT trace segments. Their geometry did not change.

The exact per-object UUID inventory is in `validation/copper_changes.json`.

## Names, drawings, and files

- Renamed `Net-(U1-VIN)` to `/VBAT_IN`, `Net-(U1-ON)` to `/PANEL_EN`, and `Net-(U1-CT)` to `/VBAT_CT`.
- Added `/VCI_IN` and `/VCI_CT`. Existing panel output rail names remain unchanged.
- Reorganized the schematic's U1, C7, TP1, TP7 and R5 block below the connectors, added U2/C8/C9/C10 and named connections, and changed R2's input-side label accordingly.
- Changed the board revision silkscreen from REV. C to REV. D. Replaced the old standalone C3/C4 reference text with reference fields at their new positions; added visible references for U2/C8/C9/C10.
- Updated the schematic title-block date/revision/comment and moved its REVISION D annotation to the top margin.
- Updated BOM quantities to U1/U2 = 2, C3/C5/C8/C9 = 4, and C7/C10 = 2. Other BOM items and their DNP status remain as supplied.
- Regenerated the full placement file, populated-component placement CSV, Gerbers, Excellon drill file, netlist and visual previews.
- Added this change log, current assembly/power instructions, and validation reports. Source reference files and footprint/symbol libraries are retained unchanged. Historical projects, backup directories and old fabrication archives were not copied into this revision.

## Verified unchanged

- All **265 DSI copper objects** are identical to Rev C: **185 straight segments, 60 arcs and 20 vias**. This comparison includes their full stored data, not just lengths.
- Both J1 and J2 retain their original positions, orientations and complete pad data, including pad numbers, nets and geometry. The user's confirmed Switch mapping is preserved.
- Stackup, layer definitions, board outline, project rules, net classes and empty custom-rule file are unchanged. No impedance or length-tuning changes were made.
- Existing VDDIO, reset, host 1.8 V, host 3.3 V, TE and VBAT output trace geometry is unchanged.

## Validation

KiCad 10.0.6, with zones refilled:

- PCB DRC: **0 violations**.
- Connectivity: **0 unconnected items**.
- Schematic/PCB parity: **0 issues**.
- ERC: **0 errors, 14 warnings**. All are the imported TPS22917/timing-capacitor symbols' existing Unspecified pin-type warning pattern, now present for two switches. No checks were disabled to obtain these results.
- Explicit netlist assertions verify external-only VBAT, shared enable, both CT capacitor connections, both bypass capacitors and the new VCI path.
- Board and schematic previews were visually inspected.

This is a CAD-verified revision; no powered measurements were performed. The TPS22917 wiring follows the [TI datasheet](https://www.ti.com/lit/ds/symlink/tps22917.pdf). Retaining 10 nF on both CT pins provides controlled ramps; firmware must still hold reset and command timing appropriately for the actual supply ramps.
