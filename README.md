# SonySwitchUp

SonySwitchUp aims to put modern hardware inside a **Sony VAIO UX**, replacing its original motherboard with a **Nintendo Switch OLED motherboard**.

The project also explores using a Switch motherboard as the foundation for a cyberdeck or other custom device, much as you might use a Raspberry Pi or LattePanda. The VAIO UX build is a practical example of that idea, with the aim of showing how adaptable Switch hardware can be.

The goal is to develop both a **VAIO UX conversion kit** and **example designs others can build on**. We hope these files inspire people to reuse, optimize, or modify the adapters for their own projects. A key motivation is putting secondhand Switch hardware to work in affordable custom builds.

## MIPI display adapter

The MIPI adapter **repins the Switch OLED motherboard's display FPC connector** to match the **Startek KD050HDOIN116-C000-a replacement AMOLED screen** used in this build. It routes the MIPI DSI display signals to the panel's pinout and provides the associated power switching and control connections.

This revision is designed for that specific screen, whose [specification is included](hardware/mipi-adapter/revD/sources/panel_spec.pdf).

- [Rev D design files](hardware/mipi-adapter/revD/) — open `mipi_adapter_revD.kicad_pro` in KiCad 10.
- [Assembly and power notes](hardware/mipi-adapter/revD/IMPORTANT.txt)
- [Design changes and recorded checks](hardware/mipi-adapter/revD/CHANGELOG.md)

## Status

**Work in progress.** Only the MIPI adapter Rev D files are included so far. The adapter is unfinished and has not been validated on powered hardware; the complete VAIO UX kit and other project components will follow as development progresses.

A project license has not been selected yet. Included third-party documents and libraries retain their respective ownership and license terms.
