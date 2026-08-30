# SECURE PASSWORD DATABASE PROBLEM GUIDE:

*(Reverse Engineering — Medium, 200pt)*

## HINTS:
Hint 1: How does the hashing algorithm work?

## TOOLS:
Ghidra / IDA ; Python (djb2)

## WALKTHROUGH:
1. Decompile `system.out`. The flag prints when your entered hash equals `make_secret()`'s return — which does **not** depend on your input.

2. `make_secret` XORs an embedded array (`obf_bytes`) with `0xAA` to build a fixed secret, then `hash()` runs **djb2** over it. Reproduce both:
```python
obf = [-61,-1,-56,-62,-110,-101,-117,-64,128,-62,-60,-117]
secret = [(b & 0xFF) ^ 0xAA for b in obf]        # -> iUbh81!j*hn!
h = 5381
for b in secret: h = (h*33 + b) & 0xFFFFFFFFFFFFFFFF
print(h)                                          # 15237662580160011234
```

3. Connect, enter any password/length, then submit the computed hash `15237662580160011234`:
    - Answer: `picoCTF{...}` (instance-specific)

## NOTES:
- djb2: `h = 5381; h = h*33 + c`. Mask to 64 bits to mirror C's unsigned overflow.
- The check is input-independent — the whole task is recomputing one fixed hash offline.
