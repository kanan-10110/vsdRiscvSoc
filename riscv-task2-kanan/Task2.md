# 🛠️ RISC-V Task 2 - Local Setup Verification

## 📋 Overview

This repository contains the implementation and verification of 4 RISC-V C programs compiled with the local RISC-V toolchain and executed using Spike simulator with proxy kernel (pk). Each program includes a uniqueness mechanism that embeds username, hostname, machine ID, and timestamps to ensure outputs are unique to this PC.

## 🎯 Objectives

- Verify local RISC-V toolchain installation and functionality
- Demonstrate cross-compilation capabilities for RISC-V architecture
- Validate Spike simulator integration with proxy kernel
- Generate unique, machine-specific program outputs for verification

## 🔍 Step-by-Step Implementation
# A.Uniqueness Mechanism Setup
First, set identity variables in the Rocky Linux host shell:
```
export U=$(id -un)
export H=$(hostname -s)
export M=$(cat /etc/machine-id | head -c 16)
export T=$(date -u +%Y-%m-%dT%H:%M:%SZ)
export E=$(date +%s)
```
<img width="594" height="174" alt="Step 1" src="https://github.com/user-attachments/assets/e481e16d-a27b-414e-a347-50bfefc45edc" />

## B. Common Header (unique.h)
```
#ifndef UNIQUE_H
#define UNIQUE_H
#include <stdio.h>
#include <stdint.h>
#include <time.h>
#ifndef KANAN_BAJAJ
#define KANAN_BAJAJ "KANAN_BAJAJ"
#endif
#ifndef PDEU
#define PDEU "PDEU"
#endif
#ifndef MACHINE_ID
#define MACHINE_ID "unknown_machine"
#endif
#ifndef BUILD_UTC
#define BUILD_UTC "unknown_time"
#endif
#ifndef BUILD_EPOCH
#define BUILD_EPOCH 0
#endif
static uint64_t fnv1a64(const char *s) {
const uint64_t OFF = 1469598103934665603ULL, PRIME = 1099511628211ULL;
uint64_t h = OFF;
for (const unsigned char *p=(const unsigned char*)s; *p; ++p) {
h ^= *p; h *= PRIME;
}
return h;
}
static void uniq_print_header(const char *program_name) {
time_t now = time(NULL);
char buf[512];
int n = snprintf(buf, sizeof(buf), "%s|%s|%s|%s|%ld|%s|%s",
KANAN_BAJAJ, PDEU, MACHINE_ID, BUILD_UTC,
(long)BUILD_EPOCH, __VERSION__, program_name);
(void)n;
uint64_t proof = fnv1a64(buf);
char rbuf[600];
snprintf(rbuf, sizeof(rbuf), "%s|run_epoch=%ld", buf, (long)now);
uint64_t runid = fnv1a64(rbuf);
printf("=== RISC-V Proof Header ===\n");
printf("User : %s\n", KANAN_BAJAJ);
printf("Host : %s\n", PDEU);
printf("MachineID : %s\n", MACHINE_ID);
printf("BuildUTC : %s\n", BUILD_UTC);
printf("BuildEpoch : %ld\n", (long)BUILD_EPOCH);
printf("GCC : %s\n", __VERSION__);
printf("PointerBits: %d\n", (int)(8*(int)sizeof(void*)));
printf("Program : %s\n", program_name);
printf("ProofID : 0x%016llx\n", (unsigned long long)proof);
printf("RunID : 0x%016llx\n", (unsigned long long)runid);
printf("===========================\n");
}
#endif
```
<img width="1013" height="871" alt="Unique h image" src="https://github.com/user-attachments/assets/51a61512-a443-40d6-b7ec-24afc7b682ae" />

### C. C. Program Implementation
Now we create files in vi editor
<img width="823" height="176" alt="Files Created" src="https://github.com/user-attachments/assets/722f8035-de2a-4635-a3ee-0da93d490995" />

Each program must include unique.h and print the header first.
# 1. factorial.c
```
#include "unique.h"
static unsigned long long fact(unsigned n){ return (n<2)?1ULL:n*fact(n-1); }
int main(void){
uniq_print_header("factorial");
unsigned n = 12;
printf("n=%u, n!=%llu\n", n, fact(n));
return 0;
}
```
<img width="762" height="218" alt="factorial_h" src="https://github.com/user-attachments/assets/ba7b8029-3a09-41b9-bb8c-2cb8b7af0774" />

## 2. max_array.c
```
#include "unique.h"
int main(void){
uniq_print_header("max_array");
int a[] = {42,-7,19,88,3,88,5,-100,37};
int n = sizeof(a)/sizeof(a[0]), max=a[0];
for(int i=1;i<n;i++) if(a[i]>max) max=a[i];
printf("Array length=%d, Max=%d\n", n, max);
return 0;
}
```
<img width="775" height="226" alt="max_array" src="https://github.com/user-attachments/assets/4873624f-df45-468a-bbd2-be75f61b2754" />

## 3. bitops.c
```
#include "unique.h"
int main(void){
uniq_print_header("bitops");
unsigned x=0xA5A5A5A5u, y=0x0F0F1234u;
printf("x&y=0x%08X\n", x&y);
printf("x|y=0x%08X\n", x|y);
printf("x^y=0x%08X\n", x^y);
printf("x<<3=0x%08X\n", x<<3);
printf("y>>2=0x%08X\n", y>>2);
return 0;
}
```
<img width="778" height="261" alt="bitops" src="https://github.com/user-attachments/assets/709a070a-1dd2-44c7-b898-13b3ab383cb7" />

## 4. bubble_sort.c
```
#include "unique.h"
void bubble(int *a,int n){ for(int i=0;i<n-1;i++) for(int j=0;j<n-1-i;j++) if(a[j]>a[j
+1]){int t=a[j];a[j]=a[j+1];a[j+1]=t;} }
int main(void){
uniq_print_header("bubble_sort");
int a[]={9,4,1,7,3,8,2,6,5}, n=sizeof(a)/sizeof(a[0]);
bubble(a,n);
printf("Sorted:"); for(int i=0;i<n;i++) printf(" %d",a[i]); puts("");
return 0;
}
```
<img width="798" height="252" alt="bubble_sort" src="https://github.com/user-attachments/assets/62369251-4eb7-4e93-a2cd-9eb47aec8adb" />

### D. Build, Run, and Capture Evidence
# Program 1: Factorial
# Compile
```
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    factorial.c -o factorial
```
# Run
```
spike pk ./factorial
```
<img width="1366" height="336" alt="Spike Factorial" src="https://github.com/user-attachments/assets/8797decf-f7d6-4c06-8295-08a7afa86385" />

# Program 2: max_array
# Compile
```
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    max_array.c -o max_array
```
# Run
```
spike pk ./max_array
```
<img width="535" height="273" alt="Spike_Max Array" src="https://github.com/user-attachments/assets/2c494eb6-e242-4eff-9bc8-33b47169e1bd" />

# Program 3: bitops
# Compile
```
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    bitops.c -o bitops
```
# Run
```
spike pk ./bitops
```
<img width="1366" height="388" alt="Spike bitops" src="https://github.com/user-attachments/assets/cb1faebb-3c27-42dc-bf50-42c6d2c16bba" />

# Program 4: bubble_sort
# Compile
```
riscv64-unknown-elf-gcc -O0 -g -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    bubble_sort.c -o bubble_sort
```
# Run
```
spike pk ./bubble_sort
```
<img width="1366" height="307" alt="Spike Bubble Sort" src="https://github.com/user-attachments/assets/e272012f-bf4c-4636-9327-cd00cfc38468" />

### E. Produce Assembly and Disassembly

## factorial
# Generate Assembly:
```
riscv64-unknown-elf-gcc -O0 -S -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    factorial.c -o factorial.s
```
# Generate Disassembly:
```
riscv64-unknown-elf-objdump -d ./factorial | sed -n '/<main>:/,/^$/p' > factorial_main_objdump.txt
```
<img width="1366" height="562" alt="Disassemble Factorial" src="https://github.com/user-attachments/assets/d4568131-6783-4a35-bced-5b7674f8eea4" />

### max_array
# Generate Assembly:
```
riscv64-unknown-elf-gcc -O0 -S -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    max_array.c -o max_array.s
```

# Generate Disassembly:
```
riscv64-unknown-elf-objdump -d ./max_array | sed -n '/<main>:/,/^$/p' > max_array_main_objdump.txt
```
<img width="1239" height="329" alt="Disassemble Max_Array" src="https://github.com/user-attachments/assets/2cb19065-55aa-4a30-8dcc-ee1265afab34" />

## bitops
# Generate Assembly:
```
riscv64-unknown-elf-gcc -O0 -S -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    bitops.c -o bitops.s
```

# Generate Disassembly:
```
riscv64-unknown-elf-objdump -d ./bitops | sed -n '/<main>:/,/^$/p' > bitops_main_objdump.txt
```
<img width="1188" height="334" alt="Disassemble Bitops" src="https://github.com/user-attachments/assets/0052ac82-103d-41bb-98bb-d762d3bf0968" />

## bubble_sort
# Generate Assembly:
```
riscv64-unknown-elf-gcc -O0 -S -march=rv64imac -mabi=lp64 \
    -DUSERNAME="\"$U\"" -DHOSTNAME="\"$H\"" -DMACHINE_ID="\"$M\"" \
    -DBUILD_UTC="\"$T\"" -DBUILD_EPOCH=$E \
    bubble_sort.c -o bubble_sort.s
```

# Generate Disassembly:
```
riscv64-unknown-elf-objdump -d ./bubble_sort | sed -n '/<main>:/,/^$/p' > bubble_sort_main_objdump.txt
```
<img width="1242" height="404" alt="Disassemble Bubble_sort" src="https://github.com/user-attachments/assets/4dd6c5bb-7d54-4e04-99d5-8ddb85537027" />

# F. RISC-V Integer Instruction Decoding
# RISC-V Integer Instruction Decoding

This document decodes 5 RISC-V integer instructions extracted from the provided objdump files.

## Instruction Format Reference
- **R-Type**: `funct7[6:0] rs2[4:0] rs1[4:0] funct3[2:0] rd[4:0] opcode[6:0]`
- **I-Type**: `imm[11:0] rs1[4:0] funct3[2:0] rd[4:0] opcode[6:0]`
- **S-Type**: `imm[11:5] rs2[4:0] rs1[4:0] funct3[2:0] imm[4:0] opcode[6:0]`

## Decoded Instructions

| Instruction | Opcode | rd | rs1 | rs2 | funct3 | funct7 | Binary | Description |
|-------------|--------|----|----|-----|--------|--------|---------|-------------|
| `addi sp,sp,-32` | 0010011 | x2 | x2 | - | 000 | - | 111111100000 00010 000 00010 0010011 | sp = sp + (-32) |
| `and a5,a5,a4` | 0110011 | x15 | x15 | x14 | 111 | 0000000 | 0000000 01110 01111 111 01111 0110011 | a5 = a5 & a4 |
| `lw a5,-20(s0)` | 0000011 | x15 | x8 | - | 010 | - | 111111101100 01000 010 01111 0000011 | a5 = memory[s0-20] |
| `sw a5,-24(s0)` | 0100011 | - | x8 | x15 | 010 | - | 1111111 01111 01000 010 01000 0100011 | memory[s0-24] = a5 |
| `blt a4,a5,10458` | 1100011 | - | x14 | x15 | 100 | - | 1111110 01111 01110 100 11000 1100011 | if (a4 < a5) goto 10458 |

## Detailed Analysis

### 1. `addi sp,sp,-32` (Address: 10328)
- **Type**: I-Type
- **Machine Code**: `1101` (0x1101)
- **Purpose**: Stack pointer adjustment - allocating 32 bytes on the stack
- **Immediate**: -32 (sign-extended 12-bit immediate: 0xFE0)

### 2. `and a5,a5,a4` (Address: 1035a)  
- **Type**: R-Type
- **Machine Code**: `8ff9` (0x8ff9)
- **Purpose**: Bitwise AND operation between registers a5 and a4
- **Source**: From bitops_main_objdump.txt - part of bitwise operations demo

### 3. `lw a5,-20(s0)` (Address: 10352)
- **Type**: I-Type  
- **Machine Code**: `fec42703` (0xfec42703)
- **Purpose**: Load word from memory at address (s0-20) into register a5
- **Offset**: -20 (0xFEC in 12-bit two's complement)

### 4. `sw a5,-24(s0)` (Address: 1034e)
- **Type**: S-Type
- **Machine Code**: `fef42423` (0xfef42423) 
- **Purpose**: Store word from register a5 to memory at address (s0-24)
- **Offset**: -24 (split as imm[11:5]=0x7F, imm[4:0]=0x08)

### 5. `blt a4,a5,10458` (Address: 1048a)
- **Type**: B-Type
- **Machine Code**: `fcf747e3` (0xfcf747e3)
- **Purpose**: Branch if less than - jumps to address 10458 if a4 < a5
- **Source**: From bubble_sort loop condition checking

## Register Mapping
- `sp` (x2): Stack Pointer
- `s0` (x8): Saved Register / Frame Pointer  
- `a4` (x14): Function Argument / Temporary
- `a5` (x15): Function Argument / Temporary

## Notes
- All addresses are in hexadecimal format as shown in objdump
- The binary representations show the actual bit layout in RISC-V instruction format
- Instructions are extracted from functional C programs including bitwise operations, sorting, and array processing
# G. Repository Structure
```
riscv-task2-username
├── unique.h
├── factorial.c
├── factorial.s
├── factorial_main_objdump.txt
├── factorial_output.png
├── factorial_main_asm.png
├── max_array.c
├── max_array.s
├── max_array_main_objdump.txt
├── max_array_output.png
├── max_array_main_asm.png
├── bitops.c
├── bitops.s
├── bitops_main_objdump.txt
├── bitops_output.png
├── bitops_main_asm.png
├── bubble_sort.c
├── bubble_sort.s
├── bubble_sort_main_objdump.txt
├── bubble_sort_output.png
├── bubble_sort_main_asm.png
├── instruction_decoding.md
└── Task 2.md
```
