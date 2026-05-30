# minecraft-concept-to-bbmodel

Convert text descriptions or reference images into editable Blockbench `.bbmodel` files through a structured asset spec and a deterministic generator.

This is not a one-shot AI mesh generator. The AI writes a clear intermediate spec with explicit cuboid sizes, positions, hierarchy, and texture details; the bundled Python script validates that spec and builds the `.bbmodel`.

## What This Skill Does

- Turns text concepts into Blockbench-ready Minecraft-style cuboid models.
- Supports reference images as structural hints, not pixel-traced geometry.
- Generates editable `.bbmodel` files with validated part hierarchy, UVs, and embedded texture atlases.
- Provides proportion checks for archetypes such as humanoids, quadrupeds, furniture blocks, and simple items.
- Can reverse an existing `.bbmodel` back into an editable spec for iteration.

## Install

Clone this repository:

```bash
git clone https://github.com/Logeaddd/minecraft-concept-to-bbmodel.git
```

Copy the skill folder into your Codex skills directory.

macOS / Linux:

```bash
mkdir -p ~/.codex/skills
cp -r minecraft-concept-to-bbmodel/minecraft-concept-to-bbmodel ~/.codex/skills/
```

Windows PowerShell:

```powershell
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills"
Copy-Item -Recurse ".\minecraft-concept-to-bbmodel\minecraft-concept-to-bbmodel" "$env:USERPROFILE\.codex\skills\"
```

Restart Codex so it can discover the skill.

## Usage

Ask Codex to use the skill, for example:

```text
Use minecraft-concept-to-bbmodel to make a cute plush bunny chair for Blockbench.
```

or:

```text
Use minecraft-concept-to-bbmodel to create an original transforming robot .bbmodel.
```

The workflow is:

```text
text or image
  -> Codex writes an asset spec JSON
  -> scripts/generate_bbmodel.py validates the spec
  -> the script outputs a .bbmodel and optional texture atlas PNG
  -> open the .bbmodel in Blockbench for polish
```

## Manual Generator Usage

After Codex creates a spec file, run:

```bash
python scripts/generate_bbmodel.py path/to/model.spec.json -o model.bbmodel --png model_atlas.png
```

Useful flags:

- `--png PATH` writes the generated texture atlas as a PNG.
- `--strict` turns proportion warnings into hard failures.
- `--no-texture` builds geometry only.

## Requirements

- Python 3
- Pillow is recommended for texture atlas generation:

```bash
pip install pillow
```

If Pillow is not installed, geometry generation can still work, but texture output may be limited.

## Repository Layout

```text
minecraft-concept-to-bbmodel/
  README.md
  SKILL.md
  schema/
    asset_spec.schema.json
  presets/
    archetypes.json
  scripts/
    generate_bbmodel.py
    bbmodel_to_spec.py
```

## Notes

The generated model is meant to be a clean editable first draft, not a final production asset. Open it in Blockbench and polish proportions, pivots, animation setup, and texture details as needed.
