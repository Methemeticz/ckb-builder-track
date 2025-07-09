## Builder Track Weekly Report — Week 3 & 4

**Name:** Ali Jouahri**Weeks Covered:** 06-24-2025 to 07-07-2025

---

### Courses & RFCs Studied

#### 1. **RFC 0008 — Serialization**

- Learned the **Molecule format** for encoding data structures.
- Understood fixed vs dynamic types, how headers & offsets work, and how it applies to `transaction`, `script`, and `witness` data.
- Practical impact: this clarified **how CKB calculates hashes and validates tx structure**.

#### 2. **RFC 0009 — VM Syscalls**

- Learned how RISC-V scripts interact with blockchain state using syscalls like:
  - `ckb_load_cell`, `ckb_load_transaction`, `ckb_load_witness`, etc.
- Practiced **writing RISC-V assembly** to load witness/tx/cell data from VM.
- Understood **partial loading**, `CKB_SOURCE_INPUT` vs `CKB_SOURCE_GROUP_INPUT`.

#### 3. **RFC 0003 — CKB-VM**

- Understood CKB-VM's architecture:
  - RISC-V based, single-threaded, no floating-point ops, max 4MB memory split into memory pages, either writable or executable.
- Learned about **dynamic linking**, **code memory execution rules (W^X)**, and ELF structure for script binaries.
- Key concept: code reuse via external cells (e.g. dynamic linking of token logic).

#### 4. **Revisited RFC 0022 — Transaction Structure**

- Applied earlier knowledge to debug and manually edit a transaction.
- Understood:
  - Input/output/dep structure
  - Role of `cell_deps`, `witnesses`, `lock`, `type`
  - Fee calculation via capacity delta

---

### Practical Work

#### 1. Wrote and ran RISC-V lock script

- Created a minimal script that exits with code `0`:
  ```asm
  li a0, 0
  li a7, 93
  ecall
  ```
- Compiled to ELF, calculated Blake2b hash, deployed in a code cell, successfully used as the lock script for a devnet cell.

#### 2. Manual Transaction Editing and Debugging

- Manually built a transaction JSON using:
  - `ckb-cli tx init`
  - `tx add-input`, `add-output`, `add-cell-dep`, `add-witness`
- Calculated correct capacities (hex, Shannon) using CLI
- Used:
  - `ckb-debugger --tx-file tx.json --script input.0.lock --bin script.elf`
  - Fixed malformed transaction errors (overflow, invalid witness, etc.)

#### 3. Script Execution & Grouping

- Debugged group input logic:
  - Witness provided once for all inputs with same lock script
  - Used `ckb_load_witness` with `CKB_SOURCE_GROUP_INPUT`

#### 4. Binary Inspection and Hashing

- Disassembled binaries
- Calculated Blake2b hash of the binary
- Understood effect of `.global _start` on ELF binary layout

#### 5. Array Search in Ripes Simulator

- Practiced RISC-V assembly using Ripes visual simulator
- Implemented a search function that scans through an array for a given value
- Understood loop structures, conditional branches (`beq`, `bne`), and indexed memory access (`lw`, `slli`, `add`) in practice
- Helped reinforce register use and memory addressing in assembly

---

### Developer Environment

- Continued use of:
  - Local dev node and miner
  - `ckb-cli` for tx manipulation
- Practiced RISC-V ELF toolchain
  - Compilation, binary inspection, hash computation

---

### Key Learnings

- Deepened mastery of CKB’s **execution model** and the separation between:
  - Transaction structure (RFC 0022)
  - VM interaction (RFC 0009)
  - Script lifecycle (compile, hash, deploy, use)
- Became more comfortable with **manual transaction building** and CLI usage.
- Internalized **how Molecule serialization** impacts hash calculation & script behavior.
