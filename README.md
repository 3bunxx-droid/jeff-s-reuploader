# JEFF Asset Spoofer

**Short description**

`JEFF Asset Spoofer` is a developer utility for use with Roblox Studio. It provides a simple, local-only workflow to preview and test alternative **Animations**, **Sounds**, and **Meshes** by redirecting asset references in your development environment to local or authorized substitute assets.

> **Important:** This tool is intended strictly for development and testing with assets you own or have explicit permission to use. Do **not** use this tool to impersonate others, bypass platform protections, pirate assets, or violate Roblox's Terms of Service or copyright law.

---

## Key features

* Preview and swap **Animations** on characters and rigs without changing published asset IDs.
* Replace **Sounds** at runtime to test audio mixes and timing.
* Substitute **Meshes** to test LODs, collision shapes, or alternative geometry.
* Simple mapping UI (local ID → target asset) and hot-reload for quick iteration.
* Scoped to local testing: changes apply only inside Roblox Studio or during local playtests.

---

## Installation

1. Clone or copy the `JEFF Asset Spoofer` module into your game's `ReplicatedStorage` or a development-only folder.
2. Include the spoofer module in any script that needs to resolve assets.
3. Optionally add the provided UI module to `StarterGui` for in-Studio mapping.

Example structure:

```
/Workspace
/ReplicatedStorage
  /JEFFAssetSpoofer
    - SpooferModule.lua
    - SpooferConfig.lua
/StarterGui
  - JeﬀSpooferUI (ScreenGui)
/ServerScriptService
  - BootstrapSpoofer.lua (optional)
```

---

## Usage

### 1) Configure mappings

Edit `SpooferConfig.lua` (or use the UI) to create mappings of original asset references to local/test assets. A mapping typically looks like:

```lua
-- SpooferConfig.lua
return {
  Animations = {
    ["rbxassetid://123456789"] = "rbxassetid://987654321", -- original -> replacement
  },
  Sounds = {
    ["rbxassetid://111222333"] = "rbxassetid://333222111",
  },
  Meshes = {
    ["rbxassetid://444555666"] = "rbxassetid://666555444",
  }
}
```

### 2) Require the module

In a LocalScript or ModuleScript used during development, call the spoofer resolver before creating or loading assets.

```lua
local Spoofer = require(game.ReplicatedStorage.JEFFAssetSpoofer.SpooferModule)
Spoofer:Init(require(game.ReplicatedStorage.JEFFAssetSpoofer.SpooferConfig))

-- When creating a new sound or animation instance:
local assetId = "rbxassetid://123456789"
local resolved = Spoofer:Resolve("Sounds", assetId) -- returns replacement if mapped
sound.SoundId = resolved or assetId
```

### 3) Use the UI (optional)

The included `JeﬀSpooferUI` ScreenGui allows you to:

* Add/remove mappings at runtime.
* Toggle the spoofer on/off for playtests.
* Hot-reload mappings without restarting Studio.

---

## Examples

**Swapping an animation on a character**

1. Map the original animation asset ID to your test animation in `SpooferConfig.lua` or the UI.
2. When loading the animation (via `Animator:LoadAnimation`), resolve the ID using `Spoofer:Resolve("Animations", originalId)`.

**Testing a mesh replacement**

1. Map the mesh ID (the MeshId property) to a test mesh.
2. On model load, set `Part.Mesh.MeshId = Spoofer:Resolve("Meshes", originalMeshId) or originalMeshId`.

---

## Safety & legal

* Only test with assets you legally own or have explicit permission to use.
* Do not use this tool to publish someone else’s work as your own, or to circumvent Roblox’s policies.
* This README and the tool are **not** legal advice. If in doubt, consult Roblox’s Terms of Service or a legal professional.

---

## Troubleshooting

* **Mappings not applying:** Ensure `Spoofer:Init` is called before assets are created. If using the UI, verify it reports the loaded mapping file path.
* **Studio/Server differences:** The spoofer is intended for local/studio testing. Do not rely on it for published game behavior unless you explicitly implement server-validated mappings.
* **Animation or sound timing issues:** Some replacements may differ in length or keyframe timing; test thoroughly.

---

## Development notes

* Keep mappings in a single config file for easy source control.
* Consider namespacing mappings per feature (e.g., `JEFF/Map/Animations`) to avoid conflicts.
* Provide a toggle in your debug menu to disable the spoofer during QA.

---

## License

Include your preferred license here (MIT, Apache 2.0, etc.).

---

## Contact

Maintained by **JEFF**. For questions or contributions, join our discord server.

---

*README created for local development and educational testing. Respect creators and platform rules.*
