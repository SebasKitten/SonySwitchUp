# HEG-CPU-01 power-rail labels

These names are aliases for existing boardview nets. No component, pad, trace, net ID, or net membership is changed. A nominal value backed by a fitted-part datasheet or public firmware is still explicitly marked as not board-measured. `_EST` remains visible where the exact package/function proof is incomplete.

| Original net | Visible name | Build scope | Confidence | Voltage status | Basis |
|---|---|---|---|---|---|
| Net8 | `VSYS` | full + safe | confirmed-topology | Battery/system dependent; not a fixed nominal voltage. | BQ24193, MAX77812 and MAX77801 documented pin functions plus source-net membership. |
| Net1 | `VDD_3V3` | full + safe | strong-topology+firmware | 3.3 V nominal; not board-measured in this project. | MAX77801 OUT/FB topology and public Switch OLED firmware support naming it the 3.3 V regulator. |
| Net10 | `VDD_1V8` | full + safe | confirmed-datasheet+topology | 1.8 V nominal from the fitted Samsung LPDDR4X VDD1 specification; not board-measured. | Both K4U6E3S4AA VDD1 grids are on this net; Samsung specifies VDD1 typ. 1.8 V. |
| Net3 | `TOUCH_AVDD_2V9_EST` | full + safe | estimated-firmware+topology | Estimated 2.9 V; requires powered measurement or the exact MAX77620H package table. | Net reaches PMIC ball B5 and the touch power subcircuit; public firmware assigns touch/ALS to LDO6 at 2.9 V. |
| Net91 | `TOUCH_VDDIO_EST` | full + safe | estimated-topology | Unresolved; label intentionally does not claim a voltage. | Direct pull-up and touch-interface topology through unidentified N65662/N65093. |
| Net6 | `OLED_VO1` | full + safe | strong-pinout+topology | Unmeasured; likely a panel bias rail, but no Switch-specific value is assigned. | S2DOS04 pin table and exact three-channel switch/output topology. |
| Net7 | `OLED_VO2` | full + safe | strong-pinout+topology | Unmeasured; likely a panel bias rail, but no Switch-specific value is assigned. | S2DOS04 pin table and exact three-channel switch/output topology. |
| Net27 | `OLED_VO3` | full + safe | strong-pinout+topology | Unmeasured; function name does not assert a Switch-specific voltage. | S2DOS04 pin table and exact three-channel switch/output topology. |
| Net210 | `OLED_CTRL` | full + safe | strong-pinout+topology | Logic level unmeasured. | S2DOS04 pin table and unique source endpoint. |
| Net211 | `OLED_FD` | full + safe | strong-pinout+topology | Logic/analog level and precise FD behavior unresolved. | S2DOS04 pin table and unique source endpoint. |
| Net21 | `DVDD_PEX` | full/NDA only | confirmed-workbook-aggregate | Unmeasured; no nominal voltage is encoded in the alias. | Eight T214 workbook pad functions share the same source net. |

## Net10 / N65729.C2 cross-reference

Original Net10 is now `VDD_1V8`. It contains N65729.C2, both Samsung LPDDR4X VDD1 grids, OLED connector N65099.4, and 36 differently named T214 supply balls. Those workbook names are:

`AVDD_PLL_A1CC2C3`, `AVDD_PLL_AC4P`, `AVDD_PLL_D2DPD`, `AVDD_PLL_G`, `AVDD_PLL_M`, `AVDD_PLL_UEREFE`, `AVDD_PLL_X`, `HVDDIO_PEX`, `HVDD_PEX_PLL_E`, `L0_HVDDIO_PEX`, `L1_HVDDIO_PEX`, `L2_HVDDIO_PEX`, `L3_HVDDIO_PEX`, `L4_HVDDIO_PEX`, `L5_HVDDIO_PEX`, `VCLAMP_USB`, `VDD18_DP`, `VDDIO_AUDIO`, `VDDIO_AUDIO_HV`, `VDDIO_AUDIO_HV_BIAS`, `VDDIO_CAM`, `VDDIO_DEBUG`, `VDDIO_DMIC`, `VDDIO_GPIO_HV`, `VDDIO_GPIO_HV_BIAS`, `VDDIO_PEX_CTL`, `VDDIO_QSPI`, `VDDIO_SDMMC1_HVBIAS`, `VDDIO_SDMMC2`, `VDDIO_SDMMC3_HV`, `VDDIO_SDMMC4`, `VDDIO_SPI`, `VDDIO_SYS`, `VDDIO_UART`, `VDD_HDMI_DP_PLL`, `VPP_FUSE`.

This is why one long ball name is not used as the global net name: all of those functions are electrically tied in the source. `VDD_1V8` describes the shared rail without hiding the per-ball functions in the full build.

Original Net21 is labelled `DVDD_PEX` only in the full/NDA build. It aggregates: `DVDD_PEX_PLL`, `L0_DVDDIO_PEX`, `L1_DVDDIO_PEX`, `L2_DVDDIO_PEX`, `L3_DVDDIO_PEX`, `L4_DVDDIO_PEX`, `L5_DVDDIO_PEX`, `L6_DVDDIO_PEX`.

## S2DOS04 N65652 mapping

| Source pad | Manufacturer pin | Function | Visible net | Original net |
|---:|---:|---|---|---|
| 1 | 1 | LX1 | Net186 | Net186 |
| 2 | 2 | PGND1 | GND | GND |
| 3 | 3 | VO1 | OLED_VO1 | Net6 |
| 4 | 4 | FBS | GND | GND |
| 5 | 5 | FD | OLED_FD | Net211 |
| 11 | 6 | SET | GND | GND |
| 12 | 7 | AGND | GND | GND |
| 16 | 8 | EN_VO3 | VDD_3V3 | Net1 |
| 10 | 9 | CTRL | OLED_CTRL | Net210 |
| 6 | 10 | VO2 | OLED_VO2 | Net7 |
| 7 | 11 | LX2 | Net185 | Net185 |
| 8 | 12 | PVIN | VSYS | Net8 |
| 9 | 13 | VO3 | OLED_VO3 | Net27 |
| 14 | 14 | PGND2 | GND | GND |
| 13 | 15 | LX3 | Net187 | Net187 |
| 15 | 16 | AVIN | VSYS | Net8 |
| 19 | 17 | EP_GND | GND | GND |

The S2DOS04 manufacturer mapping is topology-anchored because the converted source IDs do not walk the physical perimeter in manufacturer order. Every LX/VO channel, input, ground-class pad, CTRL, FD and EN_VO3 endpoint is checked. VO1/VO2/VO3 are exact chip output names; their Switch-specific operating voltages remain unmeasured.

## OLED connector power/control contacts

| Source connector ID(s) | Visible net | Role / qualification |
|---|---|---|
| 3 | `TOUCH_VDDIO_EST` | Touch I/O/pull-up rail; voltage unresolved |
| 4 | `VDD_1V8` | Confirmed 1.8 V-class shared rail |
| 14 | `OLED_CTRL` | S2DOS04 CTRL |
| 18 | `VDD_3V3` | MAX77801 output rail |
| 20, 42 | `OLED_VO1` | S2DOS04 VO1 |
| 22, 43 | `OLED_VO2` | S2DOS04 VO2 |
| 25 | `TOUCH_AVDD_2V9_EST` | Likely MAX77620 LDO6/touch analog rail; exact ball mapping unresolved |
| 36 | `OLED_FD` | S2DOS04 FD |
| 40 | `OLED_VO3` | S2DOS04 VO3 |

There is no separate OLED WLED/backlight power, enable, or PWM contact. AMS699VC01 brightness is commanded over DSI.

## Sources

- [Samsung K4U6E3S4AA LPDDR4X datasheet](https://atta.szlcsc.com/upload/public/pdf/source/20240929/7BB50846226B2390715312BE2DCC442F.pdf) — VDD1 nominal 1.8 V and fitted-memory package functions.
- [MAX77801 datasheet](https://www.analog.com/media/en/technical-documentation/data-sheets/max77801.pdf) — VSYS input and OUT/FB topology.
- [CTCaer Switch L4T release notes](https://github.com/CTCaer/switch-l4t-kernel-4.9/releases) — MAX77801 identified as the Switch OLED 3.3 V regulator.
- [Switchbrew PCV services](https://switchbrew.org/wiki/PCV_services) — MAX77620 LDO6 touch-panel/ALS assignment at 2.9 V.
- [iFixit Switch OLED teardown](https://www.ifixit.com/News/53272/nintendo-switch-oled-teardown-more-than-screen-deep) — S2DOS04 identification on the Switch OLED motherboard.
- [Samsung SM-A530F service schematic](https://raw.githubusercontent.com/bizoonydb/db/main/SM-A530F_sch.pdf) — S2DOS04 16-pin plus exposed-pad function table on PDF page 8; secondary service-document evidence.
- [Hekate display source](https://raw.githubusercontent.com/CTCaer/hekate/master/bdk/display/di.c) — AMS699VC01 OLED brightness is sent over MIPI DCS; discrete LCD WLED PWM/enable paths are bypassed on Aula.

Generated 2026-09-10. Machine-readable evidence: [POWER_rail_labels.json](POWER_rail_labels.json).
