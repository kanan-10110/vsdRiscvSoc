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

<img width="1366" height="768" alt="1" src="https://github.com/user-attachments/assets/f1275199-8442-433d-a9d5-f22378a87713" />

After this give the following Command:
```bash
sudo dnf install -y epel-release
```

This command will install and upgrade the **epel-release**

<img width="1366" height="768" alt="3" src="https://github.com/user-attachments/assets/268764e9-01a0-4bb5-94e3-3ce1d88cdd81" />

Now to install packages like texinfo, gperf, dtc & gcc and many more I gave the following Command:

```bash
sudo dnf install -y git vim autoconf automake make gcc gcc-c++ \
mpfr-devel gmp-devel libmpc-devel gawk bison flex texinfo gperf \
libtool patchutils bc zlib-devel expat-devel wget curl device-tree-compiler
```

But I got the following error 

<img width="1366" height="768" alt="4" src="https://github.com/user-attachments/assets/2ec7db46-dcf7-4188-b2e0-44bc59145da3" />

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

<img width="1366" height="768" alt="5" src="https://github.com/user-attachments/assets/47a33fbd-ac0f-4758-8df3-6703b3689f08" />
<img width="1366" height="768" alt="6" src="https://github.com/user-attachments/assets/69c53d65-719e-4aa7-9249-34146d66f4ec" />

After this I installed Device Tree Compiler (dtc) using this command:

```bash
sudo dnf install -y dtc
```

<img width="1366" height="768" alt="7" src="https://github.com/user-attachments/assets/a4a02f8c-49b2-465b-b721-7454a8142fe5" />

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

<img width="436" height="132" alt="8" src="https://github.com/user-attachments/assets/26ddc7e1-1e86-42b1-b0eb-3f38c1561f7f" />

This shows that our Directory is created and we are in that directory. After this we have to add the prebuild gcc tollchain to our path.

## Get a prebuilt RISC‑V GCC toolchain & Add toolchain to your PATH

Give the following command and the gcc toolchain will be installed 

```bash
wget "https://static.dev.sifive.com/dev-tools/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz"
tar -xvzf riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14.tar.gz
```

<img width="1366" height="768" alt="9" src="https://github.com/user-attachments/assets/8ea69c06-9ca0-4c1c-8ecd-80ade3379715" />

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

<img width="1280" height="1024" alt="10" src="https://github.com/user-attachments/assets/715e99b7-6c9f-4faf-961a-9befaee30ea5" />

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

<img width="1366" height="768" alt="11" src="https://github.com/user-attachments/assets/2bd480e4-3b3d-4e1c-ab53-0c778bf21820" />

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

```bash
cat > ~/set_native_env.sh <<'EOF'
#!/bin/bash
# Restore native Linux environment paths

export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/bin
echo "Native Linux environment restored."
hash -r  # Clear shell command cache
EOF
chmod +x ~/set_native_env.sh
source ~/set_native_env.sh
```
**This command activated the Native Linux environment.**

After this I gave the following command -

```bash

cd $pwd/riscv_toolchain
rm -rf riscv-pk  # Remove problematic version
git clone https://github.com/riscv/riscv-pk.git
cd riscv-pk
git checkout v1.0.0
mkdir -p build && cd build
../configure --prefix=$pwd/riscv_toolchain/riscv64-unknown-elf-gcc-8.3.0-2019.08.0-x86_64-linux-ubuntu14 --host=riscv64-unknown-elf
make -j$(nproc)
sudo make install
```

<img width="1280" height="1024" alt="12" src="https://github.com/user-attachments/assets/40df8b20-5bf4-481b-8ff3-0ee62b0d0692" />

This shows that the Proxy Kernel is installed and the location is given 

###







