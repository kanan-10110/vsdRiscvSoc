# 🛠️ RISC-V Toolchain Setup & Unique C Test — Rocky Linux

[![RISC-V](https://img.shields.io/badge/Architecture-RISC--V-blue.svg)](https://riscv.org/)
[![Toolchain](https://img.shields.io/badge/Toolchain-GCC%208.3.0-blueviolet.svg)]()
[![Platform](https://img.shields.io/badge/Platform-Rocky%20Linux-green.svg)](https://rockylinux.org/)
[![Status](https://img.shields.io/badge/Status-✅%20Complete-success.svg)]()

A complete guide and working implementation of the **RISC-V GCC Toolchain**, **Spike ISA Simulator**, and **RISC-V Proxy Kernel** on **Rocky Linux**, validated through a **Unique C Test** that generates a user-specific hash.

---

## 📖 Overview
This repository contains:
- Setup process for a **RISC-V development environment** on Rocky Linux.
- Verified working versions of GCC Toolchain, Spike, and pk.
- A **Unique C Test** to ensure personalized environment verification.
- Command logs, code snippets, and workflow diagrams.

---

## 🖥️ System Environment
| Spec      | Details |
|-----------|---------|
| OS        | Rocky Linux 8.9 (64-bit) |
| CPU       | 2 vCPUs |
| RAM       | 8 GB |
| Disk      | 30 GB |
| Toolchain | `riscv64-unknown-elf-gcc` 8.3.0 |
| Simulator | Spike 1.1.1-dev |
| Kernel    | pk v1.0.0 |

---

## 🛠️ Installation Workflow

### 1️⃣ Install Base Developer Tools
```bash
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y git vim autoconf automake curl   mpc-devel mpfr-devel gmp-devel gawk bison flex   texinfo gperf libtool patchutils bc zlib-devel expat-devel   gtkwave device-tree-compiler
```
... (truncated for brevity, full README provided earlier) ...
