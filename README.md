# 🎮 Godot Custom Build Templates

Automated CI/CD for building **custom Godot Engine export templates** with GitHub Actions.

This repository lets you fork, edit a `build_profile.gdbuild` (JSON) to disable classes/modules, then run the workflow to download ready-to-use Windows export templates.

---

## ✨ Features

- 🎯 **Build profile support** via `build_profile.gdbuild` (JSON): disable classes and build options.
- 🏗️ **Multiple architectures**: x86_32, x86_64, ARM64.
- 🧪 **template_debug + template_release** builds for each selected architecture.
- 🪟 Windows subsystem modes:
  - Default (**console_subsystem = false**): produces 2 EXEs (normal + console wrapper variant).
  - Optional (**console_subsystem = true**): produces 1 EXE with full console subsystem.
- 🎨 Direct3D 12 support (D3D12) enabled by default, using Godot’s dependency install script and automatically falling back to `d3d12=no` if installation fails.
- ⚙️ Optional size/perf build flags: `production`, `lto`, `optimize`, `debug_symbols` (passed directly on the SCons command line).

---

## 📦 Repository layout

Keep these files at the repository root:

- `build_profile.gdbuild` (required)
- `.github/workflows/windows_builds.yml`

---

## 🚀 Quick start

### 1) Fork this repository
Click **Fork** on GitHub, then open your fork.

### 2) Edit `build_profile.gdbuild`
This file is JSON (Godot uses `.gdbuild` extension).

#### Critical classes protection
The workflow will automatically remove these 3 classes from `disabled_classes` (if present), to avoid breaking the build:

- `Mutex`
- `Time`
- `Window`

Everything else in your profile stays untouched.

### 3) Run the workflow
1. Go to the **Actions** tab
2. Select: **🏁 Build Custom Godot Templates (Windows)**
3. Click **Run workflow**
4. Configure inputs:
   - `godot_ref` (default: `4.6-stable`) — you can use tags/branches/commits from Godot releases.  
     https://github.com/godotengine/godot/releases
   - Architectures: x86_32 / x86_64 / ARM64
   - `console_subsystem` (default: off)
   - Optional build flags: `production`, `lto`, `optimize`, `debug_symbols`
   - `enable_d3d12` (default: on)

### 4) Download the templates
After it finishes:
- Download the combined artifact: **`godot-windows-custom-templates`**
- It contains a zip with all built EXEs (renamed to the format below).

---

## 🧾 Output naming

For each `target` (debug/release) and architecture:

- Normal EXE:
  - `windows_debug_x86_64.exe`
  - `windows_release_arm64.exe`

If `console_subsystem = false` (default), you also get:

- Console wrapper EXE:
  - `windows_debug_x86_64_console.exe`
  - `windows_release_arm64_console.exe`

If `console_subsystem = true`, you get **only one** EXE per build (console subsystem enabled), named without `_console`.

---

## ⚙️ Workflow options (what they mean)

### `enable_d3d12` (default: true)
Godot requires extra dependencies to compile with Direct3D 12 support. The workflow runs:

`python misc/scripts/install_d3d12_sdk_windows.py`

and then compiles with `d3d12=yes`. If installation fails, it automatically compiles with `d3d12=no` instead.

### `production`, `lto`, `optimize`, `debug_symbols`
These flags are passed directly to SCons to match Godot’s documented build system options.

- `production=yes` can produce smaller/faster binaries but may increase build cost.
- `lto=full` can reduce size/perf further but uses more RAM and takes longer.
- `optimize=size` and `optimize=size_extra` focus on reducing binary size (size_extra is available in newer Godot versions).
- On Windows/MSVC, use `debug_symbols=no` for smaller binaries (instead of `strip`).

### ThinLTO note
If you choose `lto=thin`, the workflow automatically adds `use_llvm=yes` because ThinLTO requires LLVM.

---

## 🛠️ Troubleshooting

### “ERROR: The Direct3D 12 rendering driver requires dependencies…”
This happens if D3D12 dependencies aren’t installed. Leave `enable_d3d12` enabled (default) so the workflow runs the installer, or disable it to force `d3d12=no`.

### “ThinLTO is only compatible with LLVM…”
Pick `lto=full` or keep `lto=none`, or use `lto=thin` (the workflow will enable LLVM automatically).

### “No architectures selected”
Select at least one architecture checkbox.

---

## 🤝 Contributing

PRs are welcome, especially for adding more platforms/workflows (Android, Web, Linux, macOS).

---

## 📚 Resources

- Godot docs: Optimizing for size
- Godot docs: Compiling for Windows + D3D12 dependencies
- Godot releases (pick refs/tags): https://github.com/godotengine/godot/releases

---

## 📜 License

This project is licensed under the MIT License (see `LICENSE`).

Godot Engine itself is MIT-licensed:  
https://github.com/godotengine/godot/blob/master/LICENSE.txt
