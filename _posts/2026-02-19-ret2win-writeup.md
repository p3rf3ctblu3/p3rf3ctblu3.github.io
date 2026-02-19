## rop emporium's ret2win challenge 
---
layout: post
author: p3rf3ctblu3
---

   The challenge's title ret2win refers to a simple rop where we have to exploit the program be forcing entry to the ret function and overwriting its return address with a vulnerable function.
   At the machine code level, we are basically overwriting the EIP register, which is the return pointer. 
   Since the executable is not stripped (evident in checksec below) we can see the following symbols, where ret2win is the vulnerable function. 

   <img width="700" height="923" alt="image" src="https://github.com/user-attachments/assets/09087e85-4860-46e8-9333-4a9012b0dbdf" />

   ret2win disassembled: 

   <img width="737" height="274" alt="image" src="https://github.com/user-attachments/assets/969467d9-f0a7-4dd0-8018-ab69a95f1742" />

   We can see that it calls system, after loading a shell command with mov edi. 
   
   In checksec, can also see that PIE is disabled, meaning we can use static addresses in our exploit for ret and ret2win obtained using gdb. 

   <img width="576" height="185" alt="image" src="https://github.com/user-attachments/assets/e62a185c-8673-4346-b4e1-b79172d5f319" />

   In x86-64 systems the garbage bytes are usually of 40 byte length, which is the length we'll first use for the exploit.

   I crafted the following exploit using pwntools: 
   
   ```python
   from pwn import *
   
   ret2win = p64(0x400756)
   ret = p64(0x4006e7)
   payload = b'A' * 40 + ret + ret2win
   
   p = process("./ret2win")
   p.sendlineafter("> ", payload)
   p.interactive()
   ```

   Which gives us the flag: 
   <img width="512" height="131" alt="image" src="https://github.com/user-attachments/assets/93ae78d4-2025-47f2-9b86-64a678fd560e" />







