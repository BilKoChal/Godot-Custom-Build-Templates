# 🎮 Godot Custom Build Templates

**Automated CI/CD pipeline for building custom Godot Engine export templates with GitHub Actions.**

Build optimized, customized Godot templates tailored to your project's needs - disable unused modules, customize rendering backends, and reduce binary sizes by up to 50%.

[![Windows Builds](https://img.shields.io/badge/Windows-Templates-blue?logo=windows)](../../actions/workflows/windows_builds.yml)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Godot](https://img.shields.io/badge/Godot-4.6--stable-478cbf?logo=godot-engine)](https://godotengine.org/)

---

## ✨ Features

- 🎯 **Custom Module Selection** - Disable unused modules and classes via `build_profile.gdbuild`
- 🏗️ **Multiple Architectures** - Build for x86_32, x86_64, and ARM64
- ⚡ **Optimization Options** - Choose between speed, size, or custom optimization
- 🎨 **Rendering Backends** - Enable/disable DirectX 12, Vulkan, OpenGL3
- 🔧 **Compiler Options** - Support for MSVC, LLVM/Clang, and MinGW
- 💾 **Smart Caching** - Fast rebuilds with SCons caching
- 🧪 **Automated Testing** - Built-in executable validation
- 📦 **Ready to Use** - Download compiled templates directly from GitHub Artifacts

---

## 🚀 Quick Start

### 1. Fork this Repository

Click the **Fork** button at the top right of this page.

### 2. Configure Your Build Profile

Edit the `build_profile.gdbuild` file to customize your build:

```json
{
  "disabled_classes": [
    "GridMap",
    "NavigationRegion3D",
    "XRCamera3D",
    "XRController3D",
    "AudioStreamPlayer3D"
  ],
  "disabled_build_options": {
    "module_gridmap_enabled": false,
    "module_navigation_enabled": false,
    "module_openxr_enabled": false,
    "module_webrtc_enabled": false,
    "disable_3d": false
  }
}
```

**Note:** The following critical classes are automatically protected and cannot be disabled:
- `Mutex` - Core synchronization primitive
- `Time` - Time management system
- `Window` - Display/window handling

### 3. Run the Workflow

1. Go to **Actions** tab in your forked repository
2. Select **🏁 Build Custom Godot Templates** workflow
3. Click **Run workflow**
4. Select your desired architectures:
   - ☑️ x86_32 (32-bit)
   - ☑️ x86_64 (64-bit)
   - ☑️ ARM64
5. Configure optimization and rendering options
6. Click **Run workflow**

### 4. Download Templates

Once the build completes (15-30 minutes):
1. Go to the workflow run page
2. Scroll to **Artifacts** section
3. Download `godot-4.6-custom-templates-complete`
4. Extract the templates

### 5. Install Templates in Godot

**Option A: Manual Installation**
```bash
# Windows
%APPDATA%\Godot\export_templates\4.6.stable.custom\

# Linux
~/.local/share/godot/export_templates/4.6.stable.custom/

# macOS
~/Library/Application Support/Godot/export_templates/4.6.stable.custom/
```

**Option B: Via Godot Editor**
1. Open your Godot project
2. Go to **Editor → Manage Export Templates**
3. Click **Install from File**
4. Select the downloaded `.tpz` file

---

## 📋 Build Profile Guide

### Disabling Classes

The `disabled_classes` array allows you to remove unused Godot classes to reduce binary size:

```json
"disabled_classes": [
  "GridMap",              // 3D tilemap system
  "NavigationRegion3D",   // 3D navigation
  "NavigationAgent3D",    // AI navigation
  "XRCamera3D",           // VR/AR camera
  "XRController3D",       // VR/AR controllers
  "AudioStreamPlayer3D",  // 3D spatial audio
  "Skeleton3D",           // 3D skeletal animation
  "MeshInstance3D"        // 3D mesh rendering (use carefully!)
]
```

**💡 Tips:**
- For 2D-only games: Consider disabling most 3D-related classes
- Check [Godot Class Reference](https://docs.godotengine.org/en/stable/classes/) for class descriptions
- Test your game after disabling classes to ensure nothing breaks

### Disabling Modules

The `disabled_build_options` object controls entire modules:

```json
"disabled_build_options": {
  "module_gridmap_enabled": false,        // Disable GridMap module
  "module_navigation_enabled": false,     // Disable navigation system
  "module_openxr_enabled": false,         // Disable OpenXR (VR/AR)
  "module_webrtc_enabled": false,         // Disable WebRTC networking
  "module_multiplayer_enabled": false,    // Disable multiplayer
  "module_mono_enabled": false,           // Disable C# support
  "disable_3d": false,                    // Disable ALL 3D rendering
  "disable_advanced_gui": false           // Disable complex UI controls
}
```

**⚠️ Warning:**
- `disable_3d: true` removes the entire 3D rendering pipeline (major size reduction)
- `disable_advanced_gui: true` removes TabContainer, Tree, RichTextLabel, etc.

---

## ⚙️ Workflow Options

### Architecture Selection
- **x86_32** - 32-bit Windows (legacy support)
- **x86_64** - 64-bit Windows (standard)
- **ARM64** - ARM-based Windows devices

Each architecture builds both:
- `template_debug` and `template_release`
- Standard GUI and Console executables

### Optimization

**Optimize Mode:**
- `speed` - Prioritize runtime performance
- `size` - Minimize binary size (recommended for web/mobile)
- `none` - No optimization (debug builds)

**Link-Time Optimization (LTO):**
- `none` - No LTO (faster compilation)
- `auto` - Compiler decides
- `full` - Maximum optimization (slower build, smaller binary)
- `thin` - Balanced LTO (LLVM only)

### Rendering Backends

- **DirectX 12** - Modern Windows graphics API (requires Windows 10+)
- **Vulkan** - Cross-platform, high-performance (recommended)
- **OpenGL 3** - Legacy compatibility

**💡 Recommendation:** Enable both Vulkan and OpenGL3 for maximum compatibility.

### Compiler Options

- **Use LLVM/Clang** - Alternative compiler (sometimes produces smaller binaries)
- **Use MinGW** - GCC-based compiler (free alternative to MSVC)
- **Static C++ Runtime** - Embed C++ runtime for portable executables

---

## 🏗️ Advanced Examples

### Example 1: Minimal 2D Game Template

For a simple 2D platformer with no multiplayer:

```json
{
  "disabled_classes": [
    "GridMap", "NavigationRegion3D", "NavigationAgent3D",
    "XRCamera3D", "XRController3D", "XRServer",
    "AudioStreamPlayer3D", "Skeleton3D", "MeshInstance3D",
    "DirectionalLight3D", "OmniLight3D", "SpotLight3D",
    "Camera3D", "Node3D", "CollisionShape3D"
  ],
  "disabled_build_options": {
    "module_navigation_enabled": false,
    "module_openxr_enabled": false,
    "module_webrtc_enabled": false,
    "module_multiplayer_enabled": false,
    "disable_3d": true
  }
}
```

**Result:** ~40-50% smaller binaries

### Example 2: 3D Game without VR/Networking

For a single-player 3D adventure game:

```json
{
  "disabled_classes": [
    "XRCamera3D", "XRController3D", "XRServer",
    "GridMap", "NavigationRegion3D"
  ],
  "disabled_build_options": {
    "module_openxr_enabled": false,
    "module_webrtc_enabled": false,
    "module_multiplayer_enabled": false,
    "module_navigation_enabled": false
  }
}
```

**Result:** ~20-30% smaller binaries

### Example 3: Maximum Compatibility

Keep everything enabled for a feature-rich game:

```json
{
  "disabled_classes": [],
  "disabled_build_options": {}
}
```

**Workflow settings:**
- ✅ Enable DirectX 12
- ✅ Enable Vulkan
- ✅ Enable OpenGL3
- Optimize: `speed`

---

## 📊 Build Times & Sizes

| Configuration | Build Time | Template Size (Release) |
|---------------|------------|-------------------------|
| Full (no optimizations) | ~25 min | ~95 MB |
| Size optimized | ~30 min | ~70 MB |
| 2D only (3D disabled) | ~20 min | ~45 MB |
| Minimal (aggressive) | ~18 min | ~35 MB |

*Times are approximate and include caching benefits.*

---

## 🛠️ Troubleshooting

### Build Fails with "uses undefined class"

**Cause:** You disabled a class that's required by another enabled class.

**Solution:**
1. Check the build log for the class name
2. Remove it from `disabled_classes`
3. Re-run the workflow

### "No architectures selected" Error

**Cause:** All architecture checkboxes were unchecked.

**Solution:** Select at least one architecture (x86_32, x86_64, or ARM64).

### Templates Don't Load in Godot

**Cause:** Template version mismatch.

**Solution:**
1. Ensure you're using Godot 4.6.x
2. Check template folder naming: `4.6.stable.custom`
3. Verify files are in the correct location (see Installation section)

### DirectX 12 Fails to Build

**Cause:** D3D12 SDK installation failed.

**Solution:**
- The workflow automatically falls back to `d3d12=no`
- Check workflow logs for D3D12 SDK installation step
- If persistent, disable "Enable DirectX 12" option

---

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

- 🐛 Report bugs via [Issues](../../issues)
- 💡 Suggest features or improvements
- 📝 Improve documentation
- 🔧 Submit pull requests for new platforms (Android, iOS, Web, etc.)

### Planned Features
- [ ] Android build workflow
- [ ] Web (HTML5) build workflow
- [ ] Linux build workflow
- [ ] macOS build workflow
- [ ] iOS build workflow
- [ ] Automated template packaging (.tpz)

---

## 📚 Resources

- [Godot Engine Documentation](https://docs.godotengine.org/)
- [Compiling for Windows](https://docs.godotengine.org/en/stable/contributing/development/compiling/compiling_for_windows.html)
- [Optimizing Build Size](https://docs.godotengine.org/en/stable/contributing/development/compiling/optimizing_for_size.html)
- [Build Profile Documentation](https://docs.godotengine.org/en/stable/contributing/development/compiling/introduction_to_the_buildsystem.html)
- [Critical Classes Issue #113595](https://github.com/godotengine/godot/issues/113595)

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Note:** Godot Engine is licensed under the [MIT License](https://github.com/godotengine/godot/blob/master/LICENSE.txt).

---

## ⭐ Show Your Support

If this project helped you, please give it a ⭐️ and share it with others!

---

**Made with ❤️ for the Godot Community**