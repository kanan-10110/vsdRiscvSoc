# RISC-V Reference SoC Tapeout Program VSD

<div align="center">

[![RISC-V](https://img.shields.io/badge/RISC--V-SoC%20Tapeout-blue?style=for-the-badge&logo=riscv)](https://riscv.org/)
[![VSD](https://img.shields.io/badge/VSD-Program-orange?style=for-the-badge)](https://vsdiat.vlsisystemdesign.com/)
![Participants](https://img.shields.io/badge/Participants-3500+-success?style=for-the-badge)
![India](https://img.shields.io/badge/Made%20in-India-saffron?style=for-the-badge)

</div>

# Program Overview
The RISC-V Reference SoC Tapeout Program by VSD (VLSI System Design) offers comprehensive training in open-source ASIC design methodology. This industry-focused program covers the complete digital design cycle from specification to silicon, emphasizing practical implementation using cutting-edge open-source EDA tools and methodologies.

# Core Learning Objectives:
1. Master RISC-V processor architecture and instruction set
2. Implement robust RTL design patterns and verification strategies
3. Execute synthesis optimization using advanced Yosys techniques
4. Navigate the complete open-source RTL-to-GDSII design flow
5. Perform comprehensive timing and functional analysis with modern visualization tools

# Industry Impact: 
This program connects over 3500+ engineers and students globally, fostering innovation in semiconductor design while strengthening India's position in the global chip design ecosystem through open-source collaboration and knowledge sharing.


## Tools Installation

### ⚙️ System Requirements
| Resource | Requirement |
|----------|-------------|
| RAM      | 6 GB |
| Storage  | 50 GB HDD |
| OS       | Ubuntu 24.04.3 |
| CPU      | 4 vCPU |
The following tools form the backbone of modern open-source digital design flows, enabling seamless progression from concept to manufacturable silicon.

#### <ins>**Yosys**</ins>
Yosys serves as the primary synthesis engine for transforming high-level RTL descriptions into optimized gate-level implementations. Its advanced optimization algorithms and technology mapping capabilities make it essential for achieving timing closure and area efficiency in modern chip designs.
```
$ sudo apt-get update
$ sudo apt install git                 
$ git clone https://github.com/YosysHQ/yosys.git
$ cd yosys
$ git submodule update --init --recursive
$ sudo apt install make               # If make is not installed
$ sudo apt-get install build-essential clang bison flex \
    libreadline-dev gawk tcl-dev libffi-dev git \
    graphviz xdot pkg-config python3 libboost-system-dev \
    libboost-python-dev libboost-filesystem-dev zlib1g-dev
$ make config-gcc
$ make
```
<img width="514" height="673" alt="make install successful" src="https://github.com/user-attachments/assets/755940f6-056f-4a69-a508-6b5cbe379ab1" />
```
$ sudo make install
```
<img width="902" height="229" alt="yosys install" src="https://github.com/user-attachments/assets/7f55a86c-506b-4c14-ae99-6d4b7d6dce04" />

```
$ yosys
```
<img width="830" height="185" alt="Yosys" src="https://github.com/user-attachments/assets/0b3c22a4-664e-438e-aa88-78a03789dd28" />

#### <ins>**Iverilog**</ins>
Icarus Verilog provides a robust simulation platform for comprehensive design verification and validation. Its event-driven simulation engine supports complex testbench scenarios, enabling thorough functional coverage analysis and corner case testing essential for first-silicon success.
```
$ sudo apt-get update
$ sudo apt-get install iverilog
$ iverilog -v
```
<img width="1022" height="421" alt="IVerilog Installation" src="https://github.com/user-attachments/assets/c5cf5cbb-dfb1-49a9-abe7-a146a080efbb" />
<img width="1022" height="671" alt="iVerilog Installed" src="https://github.com/user-attachments/assets/f625d767-04aa-4921-9aed-abf3749ee650" />

#### <ins>**GTKWave - Waveform Viewer**</ins>
GTKWave delivers advanced signal analysis capabilities for debugging complex digital systems. Its multi-domain visualization features and hierarchical signal browsing enable efficient identification of timing violations, protocol errors, and functional anomalies across large-scale designs.
```
$ sudo apt-get update
$ sudo apt install gtkwave
$ gtkwave
```
<img width="1005" height="767" alt="gtkwave" src="https://github.com/user-attachments/assets/0d809fcf-1f91-40af-af16-d8337017a17d" />
<img width="1087" height="845" alt="gtkwave_2" src="https://github.com/user-attachments/assets/533bea38-039d-4672-b164-5a043cc6b04d" />

# Conclusion
The successful deployment of Yosys, Icarus Verilog, and GTKWave establishes a professional-grade open-source EDA environment capable of handling industrial-scale digital design challenges. This integrated toolchain provides comprehensive coverage of the verification-synthesis-analysis workflow, enabling designers to achieve production-quality results while maintaining cost-effectiveness and flexibility. The foundation created through this setup empowers engineers to tackle advanced ASIC development projects and contributes to the growing ecosystem of open-source semiconductor design innovation.
