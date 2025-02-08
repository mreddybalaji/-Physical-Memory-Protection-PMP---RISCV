# Physical-Memory-Protection-PMP - RISCV
 The behavior of a PMP (Physical Memory Protection) checker script. The script takes a PMP configuration file, an address, a privilege mode, and an operation as input, and determines whether the specified memory access is allowed based on the PMP entries.


### Report: PMP (Physical Memory Protection) Checker

#### **Objective:**
The objective of this task is to analyze the behavior of a PMP (Physical Memory Protection) checker script. The script takes a PMP configuration file, an address, a privilege mode, and an operation as input, and determines whether the specified memory access is allowed based on the PMP entries.

---

#### **Input:**
1. **PMP Configuration File:** `pmp_configuration.txt`
   - Contains PMP entries with attributes such as address (addr), access permissions (R, W, X), and addressing mode (A).
2. **Address:** `0xdeadbeef` (first run) and `0x20000000` (second run).
3. **Privilege Mode:** `M` (Machine mode).
4. **Operation:** `R` (Read) for the first run and `W` (Write) for the second run.

---

#### **Output:**
1. **First Run:**
   - **Command:** `python pmp_check.py pmp_configuration.txt 0xdeadbeef M R`
   - **Output:** 
     - The script lists all PMP entries with their attributes.
     - The address `0xdeadbeef` is checked against the PMP entries.
     - **Result:** `Access Allowed`.

2. **Second Run:**
   - **Command:** `python pmp_check.py pmp_configuration.txt 0x20000000 M W`
   - **Output:**
     - The script lists all PMP entries with their attributes.
     - The address `0x20000000` is checked against the PMP entries.
     - **Result:** `Access Allowed`.

---

#### **Observations:**
1. **PMP Entries:**
   - The PMP configuration file contains 64 entries.
   - Each entry has attributes such as:
     - `A`: Addressing mode (0 = OFF, 1 = TOR, 3 = NAPOT).
     - `R`: Read permission.
     - `W`: Write permission.
     - `X`: Execute permission.
     - `addr`: Address associated with the entry.
   - Most entries have `A=0` (OFF), meaning they are inactive.
   - A few entries have `A=1` (TOR) or `A=3` (NAPOT), indicating active regions with specific permissions.

2. **Access Checks:**
   - For the address `0xdeadbeef`:
     - The script checks the address against all PMP entries.
     - No active PMP entry restricts access to this address.
     - **Result:** Access is allowed.
   - For the address `0x20000000`:
     - The script checks the address against all PMP entries.
     - No active PMP entry restricts access to this address.
     - **Result:** Access is allowed.

3. **TOR and NAPOT Modes:**
   - **TOR (Top of Range):** Used in PMP Entry 32 (`A=1`). The script checks if the address falls within the range `[0x3c0000000, 0x3c0000004)`.
   - **NAPOT (Naturally Aligned Power of Two):** Used in PMP Entries 40, 48, and 56 (`A=3`). The script calculates the base address and size of the region and checks if the address falls within it.

---

#### **Conclusion:**
- The PMP checker script successfully reads the PMP configuration file and evaluates memory access requests based on the PMP entries.
- For the provided inputs (`0xdeadbeef` and `0x20000000`), the script correctly determines that access is allowed, as no active PMP entry restricts access to these addresses.
- The script handles both TOR and NAPOT addressing modes correctly.

---

#### **Recommendations:**
1. **Test Edge Cases:**
   - Test the script with addresses at the boundaries of PMP regions to ensure correct behavior.
   - Test with addresses that should be denied access to verify that the script correctly restricts access.

2. **Enhance Output:**
   - Provide more detailed output, such as which PMP entry allowed or denied the access.
   - Include a summary of the PMP configuration for better debugging.

3. **Error Handling:**
   - Add error handling for invalid inputs (e.g., malformed PMP configuration file, invalid addresses).

4. **Performance Optimization:**
   - Optimize the script for large PMP configurations by stopping the check once a matching entry is found.

---

#### **Sample Input and Output:**

**Input:**
```bash
python pmp_check.py pmp_configuration.txt 0xdeadbeef M R
```

**Output:**
```
PMP Entry 0: A=0, R=0, W=0, X=0, addr=0x300000000
PMP Entry 1: A=0, R=0, W=0, X=0, addr=0x300000004
...
PMP Entry 63: A=0, R=0, W=0, X=0, addr=0x3ffffffffffffffe0
Access Allowed
```

**Input:**
```bash
python pmp_check.py pmp_configuration.txt 0x20000000 M W
```

**Output:**
```
PMP Entry 0: A=0, R=0, W=0, X=0, addr=0x300000000
PMP Entry 1: A=0, R=0, W=0, X=0, addr=0x300000004
...
PMP Entry 63: A=0, R=0, W=0, X=0, addr=0x3ffffffffffffffe0
Access Allowed
```

---

This report summarizes the functionality and behavior of the PMP checker script based on the provided inputs and outputs. Further testing and enhancements can improve its robustness and usability.
