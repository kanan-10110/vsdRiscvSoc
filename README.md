# 🛠️ RISC-V Toolchain Setup & Unique C Test — Rocky Linux

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![Toolchain](https://img.shields.io/badge/Toolchain-GCC%208.3.0-blueviolet.svg)](https://gcc.gnu.org/releases.html)
[![Platform](https://img.shields.io/badge/Platform-Rocky%20Linux-green.svg)](https://rockylinux.org/)
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

A complete guide and working implementation of the **RISC-V GCC Toolchain**, **Spike ISA Simulator**, and **RISC-V Proxy Kernel** on **Rocky Linux**, validated through a **Unique C Test** that generates a user-specific hash.

---

## 📖 Summary
This repository details the setup process for creating a RISC-V development environment on Rocky Linux. It records the verified working versions of essential components, including the GCC Toolchain, Spike ISA Simulator, and the Proxy Kernel (pk). To ensure environment personalization, it includes a Unique C Test that generates a user-specific identifier. The repository also contains complete command logs, relevant code snippets, and workflow diagrams to guide through the installation and verification process step by step.

---

## 🖥️ Prerequisites

### System Requirement :

| Specifications | Details |
|-----------|---------|
| OS        | Rocky Linux 8.0 (64-bit) |
| CPU       | 2+ cores recommended |
| RAM       | 4-8 GB |
| Disk      | 30 GB free space |
| Toolchain (GCC Compiler) | `riscv64-unknown-elf-gcc` |
| Simulator | Spike |
| Kernel    | pk |

---

## 🛠️ Installation Workflow

### 1️⃣ Install Base Developer Tools
```bash
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y git vim autoconf automake curl   mpc-devel mpfr-devel gmp-devel gawk bison flex   texinfo gperf libtool patchutils bc zlib-devel expat-devel   gtkwave device-tree-compiler
```
... (truncated for brevity, full README provided earlier) ...
