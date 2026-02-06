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
