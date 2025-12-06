# Reverse Engineering Report — Notepad Launcher (BlueDragon7327) 

**Author:** D Eric Hattaway  
**Date:** 12/5/2025  
**Tools Used:** x64dbg, Strings (Sysinternals)  

---

## Overview
This crackme presents a simple validation challenge: the user must enter a username and key, and if the input is accepted, the program launches Notepad. The goal is to identify and bypass the validation logic.

---

## Initial Analysis

### Static Recon
I began by performing **strings analysis** to extract visible text and hints. Then I loaded the binary into **x64dbg**, analyzed the module, and used the **Symbols → Find References** feature to locate calls to `ShellExecuteA`, `WinExec`, or similar process-launching APIs.

These API calls served as an anchor point for identifying the path that corresponds to successful validation.

### Dynamic Recon
Stepping through the program revealed that the Notepad-launching API was surrounded by logic controlling whether the user’s input should reach the success path. I initially struggled to locate the exact comparison responsible for validation but was able to trace execution flow near the Notepad-launch instructions.

---

## Patching Strategy

Rather than reverse the full username/key logic, I chose a simpler and equally valid RE approach:

### **Control Flow Redirection**

I identified the instruction responsible for displaying the crackme’s introductory text and replaced it with a direct `JMP` to the code responsible for launching Notepad. This allowed me to:

- Skip username collection  
- Skip key input handling  
- Skip the entire validation routine  
- Force execution directly into the success branch  

This resulted in a clean logical bypass.

---

## What I Learned

Through this challenge, I gained experience with:

- Navigating **x32dbg/x64dbg**  
- Finding API references inside a binary  
- Extracting useful info with **strings**  
- Understanding x86/x64 assembly relevant to validation  
- Patching instructions with the debugger  
- Recognizing when a control-flow shortcut can replace deep logic analysis  

---

## Outcome
The patched binary launches Notepad immediately, regardless of username or key input, demonstrating a successful bypass of the intended validation logic.

---

## Future Steps
- Practice full logic reversal (not just patching)  
- Learn buffer tracing and input transformation techniques  
- Build progressively more complex crackme write-ups  
- Consolidate all reports into a public RE portfolio  

---

*End of Report*
