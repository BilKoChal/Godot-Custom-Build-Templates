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
- 🎨 Direct3D 12 support (D3D12) enabled by default, using Godot’s dependency install script and automatically falling back to `d3d12=no` if installation fails. [web:44][web:53]
- ⚙️ Optional size/perf build flags: `production`, `lto`, `optimize`, `debug_symbols` (passed directly on the SCons command line). [web:110]

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
This file is JSON (Godot uses `.gdbuild` extension). Example:

```json
{
  "disabled_build_options": {
    "disable_3d": true,
    "disable_xr": true,
    "module_openxr_enabled": false
  },
  "disabled_classes": [
    "XRServer",
    "XRCamera3D",
    "XRController3D"
  ],
  "type": "build_profile"
}

Critical classes protection

The workflow will automatically remove these 3 classes from disabled_classes (if present), to avoid breaking the build:

    Mutex

    Time

    Window

Everything else in your profile stays untouched.
3) Run the workflow

    Go to the Actions tab

    Select: 🏁 Build Custom Godot Templates (Windows)

    Click Run workflow

    Configure inputs:

        godot_ref (default: 4.6-stable) — you can use tags/branches/commits from Godot releases.
        https://github.com/godotengine/godot/releases

        Architectures: x86_32 / x86_64 / ARM64

        console_subsystem (default: off)

        Optional build flags: production, lto, optimize, debug_symbols

        enable_d3d12 (default: on)

4) Download the templates

After it finishes:

    Download the combined artifact: godot-windows-custom-templates

    It contains a zip with all built EXEs (renamed to the format below).
