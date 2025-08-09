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

- The First Command is:

```bash
sudo dnf groupinstall -y "Development Tools"
```

This will give you the following message:

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/1.png" />

After this give the following Command:
```bash
sudo dnf install -y epel-release
```

This command will install and upgrade the **epel-release**

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/3.png" />

Now to install packages like texinfo, gperf, dtc & gcc and many more I gave the following Command:

```bash
sudo dnf install -y git vim autoconf automake make gcc gcc-c++ \
mpfr-devel gmp-devel libmpc-devel gawk bison flex texinfo gperf \
libtool patchutils bc zlib-devel expat-devel wget curl device-tree-compiler
```

But I got the following error 

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/4.png" />

### Problem:

There was no match for Device Tree Compiler (dtc) and texinfo & gperf
So thus I gave the following command to fix the error

```bash
sudo dnf install -y epel-release
sudo dnf update -y

sudo dnf install -y texinfo gperf

dnf search texinfo
dnf search gperf

sudo dnf config-manager --set-enabled powertools
sudo dnf config-manager --set-enabled codeready-builder-for-rhel-8-x86_64-rpms
sudo dnf update -y

sudo dnf install -y texinfo gperf
```

And I got the problem fixed and all the packages were installed:

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/5.png" />

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/6.png" />



