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

After this I installed Device Tree Compiler (dtc) using this command:

```bash
sudo dnf install -y dtc
```

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/7.png" />

And thus dtc was installed 

Our installing is finished and hence we move to next step i.e. **Work Space & building** 

## 🏢 Work Space & Building

### Create a clean workspace and capture your home path

For creating a workspace we firstly need a directory (riscv_toolchain) and thus we provide the following command:

```bash
cd
pwd=$PWD
mkdir -p riscv_toolchain
cd riscv_toolchain
```

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/8.png" />

This shows that our Directory is created and we are in that directory. After this we have to add the prebuild gcc tollchain to our path.

## Get a prebuilt RISC‑V GCC toolchain & Add toolchain to your PATH

Give the following command and the gcc toolchain will be installed 

```bash
wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"
tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
```

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/9.png" />

Our gcc toolchain is installed successfully and now we need to install spike ad proxy kernel

## Build and install Spike (RISC‑V ISA simulator)

To intall spike run the following code:

```bash
sudo dnf groupinstall "Development Tools"
sudo dnf install device-tree-compiler boost-devel gmp-devel mpfr-devel libmpc-devel zlib-devel
git clone https://github.com/riscv-software-src/riscv-isa-sim.git
cd riscv-isa-sim
rm -rf build
mkdir build
cd build
../configure --prefix=$HOME/riscv
make -j$(nproc)
make install
export PATH=$HOME/riscv/bin:$PATH
which spike
```

<img width="840" height="739" alt="image" src="https://github.com/kanan-10110/vsdRiscvSoc/blob/main/10.png" />

This has installed spike 

## Build and install the RISC‑V Proxy Kernel (riscv-pk)

```bash
cd $pwd/riscv_toolchain
git clone https://github.com/riscv/riscv-pk.git
cd riscv-pk
mkdir build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64
make -j$(nproc)
sudo make install
```

<img width="840" height="739" alt="image" src="" />

After running this code I got an error mcmodel and to resolve that I followed the given procedure 

```bash
cat > ~/set_riscv_env.sh <<'EOF'
#!/bin/bash
# Activate RISC-V toolchain paths for the current shell session
export PATH=/root/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/bin:/root/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14/riscv64-unknown-elf/bin:$PATH
echo "RISC-V toolchain environment activated."
EOF
chmod +x ~/set_riscv_env.sh
source ~/set_riscv_env.sh
```

**This command activated the RISC-V toolchain environment.**







