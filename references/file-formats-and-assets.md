# File Formats and Asset Management

Use this reference for Roblox place files, model files, imports, exports, asset handling, and Rojo file mapping.

## Native Roblox Formats

| Format | Use |
|---|---|
| `.rbxl` | Binary place file for an entire place |
| `.rbxlx` | XML place file, useful for inspection and version control workflows |
| `.rbxm` | Binary model or asset hierarchy |
| `.rbxmx` | XML model or asset hierarchy |

Use binary formats for normal Studio saves and XML formats when diffability or tooling inspection matters.

## External Asset Formats

| Asset Type | Common Formats | Notes |
|---|---|---|
| Meshes/models | `.fbx`, `.gltf`, `.glb`, `.obj` | Use Universal Importer; FBX/glTF handle richer scenes than OBJ |
| Images | `.png`, `.jpg`, `.tga`, `.bmp` | PNG is best for UI transparency |
| Audio | `.mp3`, `.ogg`, `.wav`, `.flac` | Check current Roblox limits before upload |
| Video | `.mp4`, `.mov` | Check current VideoFrame docs before use |

## Import Rules

- Prefer Roblox Studio's Universal Importer for external 3D assets.
- Use Asset Manager to track uploaded images, meshes, audio, videos, and packages.
- Creator Store assets can accelerate prototyping, but inspect scripts for unknown or obfuscated code before inserting into a real project.
- For asset-heavy or recovered work, document third-party assets, generated assets, provenance, and known licensing constraints in the project log.

## Export Rules

- Export places before risky migrations.
- Save reusable systems as `.rbxm` or `.rbxmx` models.
- Use `.rbxlx` or Rojo for source-control-friendly review when appropriate.
- Keep local backups for major project recovery work.

## Rojo Mapping

| File | Roblox Type |
|---|---|
| `*.server.luau` | Server Script |
| `*.client.luau` | LocalScript |
| `*.luau` | ModuleScript |
| `default.project.json` | Rojo project map |

## Place File Troubleshooting

If a place grows unexpectedly:

1. Inspect imported models for hidden scripts or large embedded content.
2. Check duplicate parts, textures, SurfaceAppearance objects, and terrain artifacts.
3. Use simpler collision fidelity for meshes where acceptable.
4. Split oversized experiences into multiple places only when product design supports it.
5. Verify current upload and asset limits using `references/current-platform-lookup.md`.
