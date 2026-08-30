# JITFP PROBLEM GUIDE:

*(Reverse Engineering — Hard, 500pt)*

## HINTS:
Hint 1: The comparator functions are filled in at runtime (JIT) — static reads are unreliable.
Hint 2: Break at the indirect call and read the target each time.

## TOOLS:
Binary Ninja / IDA ; GDB (breakpoint scripting)

## WALKTHROUGH:
1. The binary verifies a 33-byte input. A permutation array plus a **runtime-filled** function-pointer table drives the check: each table entry points to a tiny comparator that tests one character with a `cmp imm8`.

2. Because the table is populated at runtime (Just-In-Time), you can't read the expected chars statically or by timing. Instead break at the indirect call (`call rdx`) in a debugger: at each hit, read `rdx` (the comparator address), then disassemble that function and pull the `cmp imm8` immediate — that byte is the expected character for that position.

3. Collect all 33 (position, expected char) pairs, undo the permutation to order them, and assemble the input string:
    - Scanning the ~65 tiny functions for their `cmp imm8` bytes gives the full character set; the permutation array tells you the order.
    - Answer: `picoCTF{...}` (recover by running your own breakpoint script; not published as text)

## NOTES:
- JIT/runtime-generated checks defeat pure static analysis — dynamic breakpoints at the dispatch (`call rdx`) are the reliable read.
- One `cmp imm8` per comparator = one flag byte; the permutation array is the map from comparator order to string index.
