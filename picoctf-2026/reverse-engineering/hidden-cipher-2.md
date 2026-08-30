# HIDDEN CIPHER 2 PROBLEM GUIDE:

*(Reverse Engineering — Easy, 100pt)*

## HINTS:
Hint 1: What does the program do with your correct answer — is it reused?
Hint 2: Decompile to see the exact transformation.

## TOOLS:
Ghidra / IDA ; pwntools

## WALKTHROUGH:
1. Decompile `hiddencipher2`. It asks a random math question; if you answer right it prints the flag — but `encode_flag` multiplies each flag byte by the answer (`a2`).

2. So: answer the math question, then divide each printed number by that same answer to recover the ASCII:
```python
from pwn import *; import re
r = remote('crystal-peak.picoctf.net', <port>)
q = r.recvuntil(b'? ').decode()
a,op,b = re.search(r'What is (\d+) ([\+\-\*]) (\d+)', q).groups()
a,b = int(a),int(b)
ans = a+b if op=='+' else a-b if op=='-' else a*b
r.sendline(str(ans).encode()); r.recvline()
vals = [int(x) for x in r.recvline().decode().split(', ')]
print("".join(chr(v // ans) for v in vals))
```
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- The "correct answer" doubles as the encoding multiplier — reuse of a secret value as a key is the bug.
- Integer division exactly reverses the multiply because every flag byte was multiplied by the same `ans`.
