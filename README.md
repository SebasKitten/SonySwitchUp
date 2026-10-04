# SonySwitchUp

SonySwitchUp is an ongoing hardware project with two connected goals:

1. **Modernize a Sony VAIO UX** by reusing the original device and integrating newer hardware.
2. **Make the Nintendo Switch OLED motherboard easier to reuse** through a custom adapter platform for other builds, including cyberdecks.

The VAIO UX build is one application of the broader motherboard-adapter work.

## Current status

**Work in progress.** This repository currently contains the MIPI adapter Rev D project. The adapter is unfinished, and other parts of SonySwitchUp have not yet been imported. This is not a complete or hardware-validated build package.

The imported Rev D change log records CAD validation results; those results do not establish powered hardware behavior. Repository setup did not rerun the electrical or CAD checks.

## MIPI adapter

The current files live in [`hardware/mipi-adapter/revD/`](hardware/mipi-adapter/revD/).

Open [`mipi_adapter_revD.kicad_pro`](hardware/mipi-adapter/revD/mipi_adapter_revD.kicad_pro) in **KiCad 10**. Local symbol, footprint, and 3D model libraries are included.

- [Assembly and power instructions](hardware/mipi-adapter/revD/IMPORTANT.txt)
- [Rev D design changes and recorded validation](hardware/mipi-adapter/revD/CHANGELOG.md)
- [Fabrication files](hardware/mipi-adapter/revD/fabrication/)
- [Validation reports](hardware/mipi-adapter/revD/validation/)
- [Reference material](hardware/mipi-adapter/revD/sources/)

Read the current assembly and power instructions before using the board files. Historical reference notes in `sources/` do not override the current Rev D instructions.

## Repository layout

```text
hardware/
  mipi-adapter/
    revD/             KiCad project, local libraries, BOM, fabrication and reports
```

Additional hardware, software, enclosure designs, and build documentation can be added as those parts become available. Keep each component in its own folder; add future adapter revisions alongside `revD`.

## Working on the project

Use the cloned repository as the working copy. Git tracks the project files, while KiCad locks, per-user settings, and automatic backups are ignored.

After editing and saving files, run these commands from the repository folder:

```powershell
git status
git add .
git diff --cached --stat
git commit -m "Describe what changed"
git push
```

Before working from a second PC, run `git pull --ff-only` there. Commit and push your work before switching computers.

For a change that you want to review separately, create a branch with `git switch -c your-change-name`, commit the changes, then run `git push -u origin HEAD`.

## Ownership and reference material

No project-wide license has been selected yet. Included third-party documents and libraries retain their respective ownership and license terms.
