# ⚡ Project: OMEGA CrackMe (2026 Edition) ⚡

Welcome to the **OMEGA** security architecture. This project is not a simple beginner's CrackMe. It is a high-level reverse engineering challenge designed to neutralize 99% of automated analysis tools (VirusTotal, Filescan.io, CAPE) and make manual analysis (IDA/Ghidra) extremely painful.

> **Objective:** Discover the secret password and reach the `[+] ACCESS GRANTED` message without triggering a BSOD or forced application closure.

---

## 🛠️ Defense Architecture (Inside the Box)

The CrackMe relies on four interleaved pillars of protection:

### 1. 🛡️ Early Detection & Sandbox Evasion (OMEGA Mode)
From the very first millisecond, the program validates its environment. If a sandbox is detected, it doesn't just close: it **saturates** the analysis system.
*   **Spec Verification**: Rejection if RAM < 8GB, CPU < 2 or > 8 cores, or Disk < 100GB.
*   **Resource Exhaustion**: If a VM is suspected, the program launches a **Recursive Process Explosion** (5 children per level, up to a depth of 2200). Each child consumes GBs of RAM via a `MemoryBomb` to force an OOM (Out Of Memory) on the sandbox server.
*   **BSOD Trigger**: Using `NtRaiseHardError` and CPU register `CR0` sabotage to attempt to crash the hypervisor or the guest OS.

### 2. 👻 Ghost Dropper & Mutation
The original binary suicides immediately after cloning itself into a file with a random name (e.g., `aBcD1234eFgH.exe`).
*   **Polymorphism**: The clone is "mutated" by adding XML padding (Goodware) to lower its entropy and deceive AI scanners.
*   **CAPE/Cuckoo Bypass**: Use of `RegLoadAppKeyA` to break common sandbox system hooks.

### 3. 🧩 Runtime Section Encryption (S-Box)
The critical function that verifies the password (`Check_Real`) resides in a dedicated memory section named `.pdata_c`. 
*   **Stable XOR Encryption**: On disk, this section is encrypted. It is only decrypted in memory (via `CryptSection`) at the exact moment of password entry.
*   **Zero-Persistence**: Immediately after verification, the section is re-encrypted to leave nothing in the clear in RAM for a memory dump.

### 4. 🕵️ Anti-Debug Watchdog (The Bloodhound)
A separate thread runs in a loop to detect:
*   **ScyllaHide Hooks**: Analysis of the first bytes of `ntdll.dll` APIs to see if they have been modified.
*   **Hardware Breakpoints**: Reading debug registers `DR0-DR7`.
*   **Timing Checks**: Use of `__rdtsc` to detect slowdowns caused by "Step-over" in IDA/x64dbg.
*   **Blacklist**: Detection of over 40 tools (Wireshark, Process Hacker, etc.).

---

## 🔓 What Reverse Engineers will have to do

To successfully crack this binary, an analyst will have to:
1.  **Bypass the Watchdog**: Disable the monitoring thread without crashing the application.
2.  **Defeat the Ghosting**: Prevent the original process from suiciding to maintain a stable entry point.
3.  **Dump the .pdata_c section**: Find the exact moment when `CryptSection` decrypts the code to extract the `Check_Real` logic.
4.  **Reverse the obfuscated logic**: Even if the code is decrypted, all character strings are protected by `AY_OBFUSCATE`, making the search for the password invisible to a simple `grep` or static string search.

---

## 🚀 Compilation

*   **IDE**: Visual Studio 2022 (x64)
*   **Mode**: Release (Essential for anti-debug optimization)
*   **Subsystem**: Windows / Console (your choice for tests)
*   **Libraries**: `crypt32.lib`, `ws2_32.lib`, `wininet.lib`, `iphlpapi.lib`.

---
*Developed exclusively by ZenithSouu. Maximum Security Engaged.* ⚡🦾
