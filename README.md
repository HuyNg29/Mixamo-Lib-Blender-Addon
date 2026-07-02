# Mixamo Animation Library (Blender 4.x – 5.x)

An animation library add-on for Blender: scan a folder of **FBX files downloaded from Mixamo**, list every animation in the sidebar, and apply them directly to the selected rig — no more importing/deleting files by hand.

## Features

- **Folder scanning** (recursive) for Mixamo `.fbx` files, with a searchable list in the panel.
- **Apply to Selected Armature** — imports the FBX in the background, copies the action onto the active armature (Mixamo rigs sharing the `mixamorig:` skeleton), then cleans up the temporary objects. Warns when bone names don't match.
- **Click to Preview** — click any imported animation (✓) in the list and the armature switches to it instantly; start playback (Space) and browse through your library.
- **In Place** — automatically detects and removes root-motion channels on the Hips (forward-travelling walk/run → stays in place).
- **Import as New Character** — imports the full character (mesh + rig + animation).
- **Import All as Actions** — batch-imports the whole library as Actions (with fake user) for the Action Editor / NLA.
- **Stash All to NLA (Unity)** — creates one NLA strip per imported action, so each one exports as a separate animation clip in FBX / Unity.
- **Export FBX (Unity)** — opens the FBX exporter with Unity-friendly presets (NLA strips as clips, no leaf bones, applied scale).
- Optional **Push to NLA** and **Set Frame Range**.
- A ✓ mark in the list means the animation has already been imported as an Action.
- Compatible with Blender 4.4+ **slotted actions**.

## Installation

### Blender 4.2 and newer (Extensions)
1. Zip the `mixamo_anim_lib` folder (or use the prebuilt zip).
2. `Edit > Preferences > Get Extensions > ▾ (top-right) > Install from Disk…` → pick the zip file.

### Blender 4.0 / 4.1 (Legacy add-on)
1. `Edit > Preferences > Add-ons > Install…` → pick the zip file.
2. Enable the **Mixamo Animation Library** checkbox.

## Usage

1. Download animations from [mixamo.com](https://www.mixamo.com) as **FBX Binary, With Skin** (or Without Skin if you already have a character) and collect them in one folder.
2. Open the panel: **3D Viewport → N key → "Mixamo Lib" tab**.
3. Pick the library folder — the add-on scans it automatically (or press **Scan Library**).
4. Select your character's armature, pick an animation in the list, and press **Apply to Selected Armature**.

### Exporting to Unity / game engines

1. Press **Stash All to NLA (Unity)** — every imported action becomes an NLA strip. Only stashed animations are exported; loose actions (fake user only) are skipped by the FBX exporter.
2. Press **Export FBX (Unity)** and drag the resulting `.fbx` into your project — each strip shows up as a separate clip (`Armature|Standing Melee Punch`, …).
3. Alternatively, to drag `.blend` files straight into Unity (fast iteration), patch Unity's converter with the included **UnityFix** package (`patch-unity.bat`) — Unity's stock `Unity-BlenderToFBX.py` exports with `bake_anim_use_nla_strips=False`, which collapses everything into a single "Scene" clip. Re-apply the patch after every Unity update.

> Note: copying actions directly is only correct when the target rig uses the same Mixamo skeleton (`mixamorig:...` bone names, same proportions). For other rigs (Rigify, custom, …) use a dedicated retargeting tool.

## Structure

```
mixamo_anim_lib/
├── __init__.py            # the entire add-on logic
└── blender_manifest.toml  # manifest for the Extensions system (4.2+)
```
