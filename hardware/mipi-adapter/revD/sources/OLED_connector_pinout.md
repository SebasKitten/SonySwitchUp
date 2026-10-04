# Switch OLED connector pinout - full/NDA edition

HEG-CPU-01 / N65099 - prepared 2026-09-19 for Startek KD050HDOIN116-C000-a / RM67295.

Open `HEG-CPU-01_OLED_connector_NDA.pcb` with its matching `.obdata` alongside it. Search `N65099`. The full board and NDA/T214 labels are retained; only this connector's 45 pad labels and relevant net comments are changed from the latest named base.

**All Switch numbers here are source solder-pad IDs, NOT verified flex/mating-contact numbers.** Rows are IDs 23-43 at smaller source Y and IDs 1-22 at larger source Y, increasing left to right in source X. Anchors 44/45 are mechanical. The drawing is in source XY with Y down: board top/bottom, mating face, latch direction and physical contact 1 remain unverified. Do not infer an odd/even contact permutation.

![Source solder-pad diagram](OLED_connector_pad_map.png)

## Power at the connector

| Source ID(s) | Original net | Function | Voltage status | Startek display-only use |
|---|---|---|---|---|
| 4 | Net10 | VDD_1V8 | 1.8 V nominal; not measured | Candidate -> contact 13 VDDIO |
| 18 | Net1 | VDD_3V3 | 3.3 V nominal; not measured | Candidate -> contact 30 VCI |
| 20, 42 | Net6 | S2DOS04 VO1 | Voltage and polarity not verified | Leave isolated |
| 22, 43 | Net7 | S2DOS04 VO2 | Voltage and polarity not verified | Leave isolated |
| 40 | Net27 | S2DOS04 VO3 | Voltage and polarity not verified | Leave isolated |
| 25 | Net3 | Touch AVDD candidate | 2.9 V estimate | Leave isolated |
| 3 | Net91 | Touch I/O supply candidate | Unknown | Leave isolated |

**Net10 is a shared rail.** The T214 source ties 36 differently named I/O/PLL supply balls to it, and the fitted-memory reference maps both VDD1 grids to the same net. The memory datasheet specifies 1.80 V nominal (M, page 44, section 8.1/Table 17). That supports VDD_1V8; copying one VDDIO ball name would hide other consumers. The datasheet's DRAM operating range is not a measurement or tolerance guarantee for the connector. Net1 reaches MAX77801 OUT (R); the Switch-specific 3.3 V designation is corroborated by L.

`_EST` means inferred; `_VUNK` means voltage unknown. Existing `.obdata` diode-test values, including 0.308 on VDD_1V8, are preserved and **are not operating supply voltages**. No available-current budget or transient tolerance has been established for either shared rail.

## All pads: functions and endpoints

| Source ID | New pin label | Original -> current net | Function / main endpoint | Evidence |
|---:|---|---|---|---|
| 1 | `1:TOUCH_RST_N` | Net491 -> DAP4_SCLK | Touch reset on PJ7; DAP4_SCLK is the Tegra mux-pad name. N65106.AG31 / DAP4_SCLK; T214!AG29 | B,W,H2 |
| 2 | `2:TOUCH_I2C_SCL` | Net290 -> GEN3_I2C_SCL | Touch I2C3 clock; tied pull-ups do not establish a voltage. N65106.J31 / GEN3_I2C_SCL; T214!AG11 | B,W,H2 |
| 3 | `3:TOUCH_VDDIO_EST` | Net91 -> TOUCH_VDDIO_EST | Touch I/O/pull-up supply candidate; N65662.1 and N65093.4. N65662.1; N65093.4 | B,A |
| 4 | `4:VDD_1V8` | Net10 -> VDD_1V8 | Shared rail; candidate Startek VDDIO supply. VDDIO names describe consumers, not a unique Tegra rail name. N65104/N65105 VDD1; 36 T214 supply balls | B,W,M |
| 5 | `5:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 6 | `6:DSI_A_D3_N` | Net466 -> DSI_A_D3_N | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AF2 / DSI_A_D3_N; T214!D28 | B,W,P |
| 7 | `7:DSI_A_D0_P` | Net465 -> DSI_A_D0_P | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AD6 / DSI_A_D0_P; T214!H26 | B,W,P |
| 8 | `8:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 9 | `9:DSI_A_CLK_N` | Net468 -> DSI_A_CLK_N | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AC1 / DSI_A_CLK_N; T214!C25 | B,W,P |
| 10 | `10:DSI_A_D1_P` | Net462 -> DSI_A_D1_P | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AC6 / DSI_A_D1_P; T214!H25 | B,W,P |
| 11 | `11:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 12 | `12:DSI_A_D2_N` | Net470 -> DSI_A_D2_N | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AC9 / DSI_A_D2_N; T214!K25 | B,W,P |
| 13 | `13:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 14 | `14:OLED_CTRL` | Net210 -> OLED_CTRL | S2DOS04 CTRL, source pad 10 / manufacturer pin 9. Direction and exact protocol unresolved. N65652 source 10 / mfr 9 CTRL | B,S |
| 15 | `15:OLED_STATUS_EST` | Net490 -> WIFI_EN | Tegra WIFI_EN / PH0. OLED-status role comes from commented firmware; not a verified TE signal. N65106.AL9 / WIFI_EN; T214!K33 | B,W,H1 |
| 16 | `16:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 17 | `17:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 18 | `18:VDD_3V3` | Net1 -> VDD_3V3 | MAX77801 output; candidate Startek VCI supply. Older alias AVDD_USB names one consumer. N65075 source E3/E4 / mfr C1/D1 OUT | B,R,L |
| 19 | `19:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 20 | `20:OLED_VO1_VUNK` | Net6 -> OLED_VO1 | S2DOS04 VO1, source/manufacturer pad 3; same net as source ID 42. N65652 source 3 / mfr 3 VO1 | B,S |
| 21 | `21:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 22 | `22:OLED_VO2_VUNK` | Net7 -> OLED_VO2 | S2DOS04 VO2, source pad 6 / manufacturer pin 10; same net as source ID 43. N65652 source 6 / mfr 10 VO2 | B,S |
| 23 | `23:TOUCH_I2C_SDA` | Net289 -> GEN3_I2C_SDA | Touch I2C3 data; preserve separate net from SCL. N65106.L34 / GEN3_I2C_SDA; T214!AJ13 | B,W,H2 |
| 24 | `24:TOUCH_INT` | Net489 -> TOUCH_INT | Touch interrupt. Active level not established by the ballout. N65106.AD34 / TOUCH_INT; T214!AJ26 | B,W |
| 25 | `25:TOUCH_AVDD_2V9_EST` | Net3 -> TOUCH_AVDD_2V9_EST | PMIC B5 / touch circuit. Firmware sets LDO6 to 2.9 V; exact B5-to-LDO6 package proof remains incomplete. N65658.B5; N65662.3/.4 | B,A,H2 |
| 26 | `26:LCD_RST_N` | Net361 -> LCD_RST | PV2 panel reset; logical candidate for Startek RESX pin 12. Verify level against target VDDIO. N65106.AC34 / LCD_RST; T214!AJ25 | B,W,H1,P |
| 27 | `27:DSI_A_D3_P` | Net467 -> DSI_A_D3_P | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AF3 / DSI_A_D3_P; T214!E28 | B,W,P |
| 28 | `28:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 29 | `29:DSI_A_D0_N` | Net464 -> DSI_A_D0_N | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AD5 / DSI_A_D0_N; T214!G26 | B,W,P |
| 30 | `30:DSI_A_CLK_P` | Net469 -> DSI_A_CLK_P | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AD1 / DSI_A_CLK_P; T214!C26 | B,W,P |
| 31 | `31:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 32 | `32:DSI_A_D1_N` | Net463 -> DSI_A_D1_N | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AC7 / DSI_A_D1_N; T214!I25 | B,W,P |
| 33 | `33:DSI_A_D2_P` | Net471 -> DSI_A_D2_P | Preserve lane number, pair polarity and return path; no GPIO level shifter. D0 protocol turnaround is not excluded. N65106.AC8 / DSI_A_D2_P; T214!J25 | B,W,P |
| 34 | `34:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 35 | `35:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 36 | `36:OLED_FD` | Net211 -> OLED_FD | S2DOS04 FD, source/manufacturer pad 5. No expansion of FD behavior without exact functional documentation. N65652 source 5 / mfr 5 FD | B,S |
| 37 | `37:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 38 | `38:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 39 | `39:GND` | GND -> GND | Ground in supplied boardview. Preserve a continuous DSI return plane. Source ground | B |
| 40 | `40:OLED_VO3_VUNK` | Net27 -> OLED_VO3 | S2DOS04 VO3, source pad 9 / manufacturer pin 13. N65652 source 9 / mfr 13 VO3 | B,S |
| 41 | `41:NC_SOURCE` | NC -> NC | Unconnected in source. Not a hardware-proven NC, reserved or DNC classification. Source NC marker (not an electrical connection) | B |
| 42 | `42:OLED_VO1_VUNK` | Net6 -> OLED_VO1 | S2DOS04 VO1; duplicate of source ID 20, not a separate supply. Same source net as ID 20 | B,S |
| 43 | `43:OLED_VO2_VUNK` | Net7 -> OLED_VO2 | S2DOS04 VO2; duplicate of source ID 22, not a separate supply. Same source net as ID 22 | B,S |
| 44 | `44:GND_ANCHOR` | GND -> GND | Grounded mechanical anchor; not a flex contact. Source ground | B |
| 45 | `45:GND_ANCHOR` | GND -> GND | Grounded mechanical anchor; not a flex contact. Source ground | B |

## Direction, voltage domain and polarity

Direction is relative to the Switch host unless stated. P/N denotes differential polarity, not DC rail sign. DSI uses its physical-layer signaling; do not treat a DSI conductor as a 1.8 V power/GPIO pin.

| Source ID | Type | Direction | Voltage/domain | Active polarity | Startek candidate |
|---:|---|---|---|---|---|
| 1 | Touch | Host -> touch | Logic voltage unknown | Active low (firmware) | Leave isolated for display-only adapter |
| 2 | Touch | Host clock; open drain | Pull-up supply unresolved | I2C clock | Leave isolated for display-only adapter |
| 3 | Touch power | Host supply -> touch (inferred) | UNKNOWN | Unknown | Leave isolated for display-only adapter |
| 4 | Power | Host supply -> panel | 1.8 V nominal; unmeasured | Positive | 13 / VDDIO |
| 5 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 6 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Negative conductor | 28 / D3N |
| 7 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Positive conductor | 24 / D0P |
| 8 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 9 | DSI | Host -> panel clock | MIPI D-PHY; not a DC supply | Negative conductor | 22 / CLKN |
| 10 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Positive conductor | 18 / D1P |
| 11 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 12 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Negative conductor | 16 / D2N |
| 13 | NC | None assigned | N/A | N/A | Leave unconnected |
| 14 | Bias control | Unresolved | UNKNOWN logic level | Unknown | Leave isolated for display-only adapter |
| 15 | Control | Panel -> host (inferred) | UNKNOWN logic level | Unknown | Leave isolated for display-only adapter |
| 16 | NC | None assigned | N/A | N/A | Leave unconnected |
| 17 | NC | None assigned | N/A | N/A | Leave unconnected |
| 18 | Power | Host supply -> panel | 3.3 V nominal; unmeasured | Positive | 30 / VCI |
| 19 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 20 | Bias power | Host supply -> stock panel | UNKNOWN Switch bias voltage | Unverified | Leave isolated for display-only adapter |
| 21 | NC | None assigned | N/A | N/A | Leave unconnected |
| 22 | Bias power | Host supply -> stock panel | UNKNOWN Switch bias voltage | Unverified | Leave isolated for display-only adapter |
| 23 | Touch | Bidirectional; open drain | Pull-up supply unresolved | I2C data | Leave isolated for display-only adapter |
| 24 | Touch | Touch -> host (role) | UNKNOWN logic level | Unknown | Leave isolated for display-only adapter |
| 25 | Touch power | Host supply -> touch (inferred) | 2.9 V ESTIMATE; unmeasured | Positive (inferred) | Leave isolated for display-only adapter |
| 26 | Reset | Host -> panel | UNKNOWN host logic level | Active low | 12 / RESX |
| 27 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Positive conductor | 27 / D3P |
| 28 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 29 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Negative conductor | 25 / D0N |
| 30 | DSI | Host -> panel clock | MIPI D-PHY; not a DC supply | Positive conductor | 21 / CLKP |
| 31 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 32 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Negative conductor | 19 / D1N |
| 33 | DSI | DSI data (HS host -> panel) | MIPI D-PHY; not a DC supply | Positive conductor | 15 / D2P |
| 34 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 35 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 36 | Bias control | Unresolved | UNKNOWN level | Unknown | Leave isolated for display-only adapter |
| 37 | NC | None assigned | N/A | N/A | Leave unconnected |
| 38 | NC | None assigned | N/A | N/A | Leave unconnected |
| 39 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 40 | Bias power | Host supply -> stock panel | UNKNOWN Switch bias voltage | Unverified | Leave isolated for display-only adapter |
| 41 | NC | None assigned | N/A | N/A | Leave unconnected |
| 42 | Bias power | Host supply -> stock panel | UNKNOWN Switch bias voltage | Unverified | Leave isolated for display-only adapter |
| 43 | Bias power | Host supply -> stock panel | UNKNOWN Switch bias voltage | Unverified | Leave isolated for display-only adapter |
| 44 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |
| 45 | Ground | Return | 0 V reference | N/A | Common GND; anchors remain mechanical |

## Startek adapter mapping

These are logical nets to the specified Startek contacts. Switch physical contacts remain unresolved. Touch is excluded from this display-only mapping.

| Switch source ID | Signal | Startek contact | Panel signal |
|---:|---|---:|---|
| 7 | DSI_A_D0_P | 24 | D0P |
| 29 | DSI_A_D0_N | 25 | D0N |
| 10 | DSI_A_D1_P | 18 | D1P |
| 32 | DSI_A_D1_N | 19 | D1N |
| 33 | DSI_A_D2_P | 15 | D2P |
| 12 | DSI_A_D2_N | 16 | D2N |
| 27 | DSI_A_D3_P | 27 | D3P |
| 6 | DSI_A_D3_N | 28 | D3N |
| 30 | DSI_A_CLK_P | 21 | CLKP |
| 9 | DSI_A_CLK_N | 22 | CLKN |
| 26 | LCD_RST_N | 12 | RESX |
| 4 | VDD_1V8 | 13 | VDDIO |
| 18 | VDD_3V3 | 30 | VCI |

Startek grounds: contacts **1, 7, 8, 9, 14, 17, 20, 23, 26, 29, 31, 39** to the ground plane. Switch ground contact IDs: **5, 8, 11, 19, 28, 31, 34, 35, 39**, plus mechanical anchors 44/45. Do not assume these lists specify one-to-one cable order.

**Startek VBAT contacts 2-6 need a separately engineered supply.** No N65099 net is directly VSYS/Net8 or battery VBAT. A controlled VSYS-derived feed elsewhere on the motherboard is a candidate; range, inrush, current capacity, shutdown and isolation must be established. Do not connect VO1/VO2/VO3 to the Startek supply inputs, and do not assume the existing 3.3 V connector rail can supply VBAT's load.

Panel specification P page 12, section 6.2: VBAT 2.9-4.5 V, VCI 2.5-4.8 V (typical 3.3 V), VDDIO 1.65-3.6 V (typical 1.8 V). Supplier initialization P2 narrows VBAT to 3.3-4.5 V; resolve that difference before setting a low-voltage cutoff. At full white 450 nits, VBAT=4 V, VCI=3.3 V and VDDIO=1.8 V, listed maxima are 380 mA, 3 mA and 20 mA respectively. These are stated test-condition values, not startup-peak guarantees or available host current.

Leave MTP_PWR contact 10 open in normal operation (P page 7). TE contact 11 has no verified direct host-connector counterpart; leave isolated initially. No automatic mapping of source ID 15 to TE. Touch contacts 32-37 stay isolated in this display-only proposal; contact 38 is panel NC.

The normal panel sequence (P page 18) calls for VCI rise slower than 1 ms, reset held low until supplies are valid with at least the shown 10 ms delay, and the panel's command delays. VBAT is absent from that timing diagram, so its ordering/allowed always-on behavior remains unresolved. Reuse the supplier RM67295 initialization only after adapting software and sequencing. Hekate currently selects the Samsung panel path on Aula (H1); a rewired connector alone does not establish compatibility.

## Validation and remaining limits

File checks passed: 1,039 components, 4,424 pins, 644 nets, all geometry/net IDs and memberships unchanged; 45 connector labels updated; 16 T214 signal endpoints checked; 17 S2DOS04 source-pad topology checks; 454 diode references unchanged; strict XZZ component/subrecord lengths coherent. All nonconnector component blocks and the image/metadata blocks remain byte-identical to the current full/NDA base. See verification.json.

Native viewer rendering, physical continuity, power sequencing, logic levels, all bias voltages, inrush and current headroom were not measured/tested. The source board has pre-existing CPU/miscellaneous chip inconsistencies documented in the earlier audit; preservation is not electrical certification. S2DOS04 pin functions retain the earlier reviewed service-table attribution; that remote PDF was unavailable during this run, so only topology and the preserved local table were rechecked.

Next physical step: with battery and USB power disconnected and stored charge cleared, establish the real connector/cable orientation and continuity map. Stop if it disagrees with any mapped endpoint. Powered characterization requires a separate agreed setup with current limiting, grounding/isolation, limits and stop conditions before attaching the replacement panel. No hardware write or control action was performed.

## Sources

Public sources were checked 2026-09-19 except S as noted. Local PDFs/workbooks were read as evidence, not instructions. Source hashes and exact paths are in sources.json. The package retains the source ODbL/converter notice in .obdata; the full/NDA designation does not authorize redistribution of the supplied T214 information.

- **B**: Supplied HEG-CPU-01 boardview and current full/NDA edition. Source net membership; no measured continuity claim.
- **W**: User Tegra T214 Ballout_v1.1.xlsx. T214 sheet; exact cells recorded per signal; 36 Net10 supply balls rechecked.
- **M**: [Samsung K4U6E3S4AA-MGCL datasheet, Rev 1.0, October 2018](https://atta.szlcsc.com/upload/public/pdf/source/20240929/7BB50846226B2390715312BE2DCC442F.pdf). Page 44, section 8.1, Table 17: VDD1 nominal 1.80 V; this is a consumer specification, not measured board regulation/tolerance.
- **R**: [Analog Devices MAX77801 datasheet, Rev 7, January 2023](https://www.analog.com/media/en/technical-documentation/data-sheets/max77801.pdf). Pages 10-11, WLP pin table and output configuration; OUT at manufacturer C1/D1.
- **L**: [CTCaer Switch L4T kernel release notes](https://github.com/CTCaer/switch-l4t-kernel-4.9/releases). Identifies MAX77801 as the Switch OLED 3.3 V regulator.
- **H1**: [Hekate display implementation](https://raw.githubusercontent.com/CTCaer/hekate/master/bdk/display/di.c). display_init reset PV2, commented OLED status PH0, AMS699VC01 initialization and DCS brightness.
- **H2**: [Hekate touchscreen implementation](https://raw.githubusercontent.com/CTCaer/hekate/master/bdk/input/touch.c). touch_power_on: PJ7 reset, I2C3, LDO6 configured to 2900000 uV.
- **S**: [Prior exact S2DOS04 function audit; Samsung SM-A530F service schematic page 8](https://raw.githubusercontent.com/bizoonydb/db/main/SM-A530F_sch.pdf). Retained previously reviewed pin table; 17 source-pad topology checks rerun. Online schematic could not be fetched on 2026-09-19. No phone-specific voltage or control behavior imported.
- **I**: [iFixit Nintendo Switch OLED teardown, October 13, 2021](https://www.ifixit.com/News/53272/nintendo-switch-oled-teardown-more-than-screen-deep). Independent S2DOS04 identification; memory population in this teardown differs from the supplied reference board.
- **A**: Local HEG-CPU-01 power-rail audit. Estimated touch supplies and exact regulator endpoints; uncertainty retained.
- **P**: Startek KD050HDOIN116-C000-a specification V1.0. Pages 7-8 contact assignment; page 12 operating supplies/currents; pages 18-19 sequencing.
- **P2**: Startek KD050HDOIN116-C000-4LANE initialization text. VBAT 3.3-4.5 V, VCI 3.3 V, IOVCC 1.8 V and panel-specific initialization; read as data, not executed.
