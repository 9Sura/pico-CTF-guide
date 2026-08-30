# BYTEMANCY 2 PROBLEM GUIDE:

*(General Skills — Medium, 200pt)*

## HINTS:
Hint 1: "Send the HEX BYTE 0xFF 3 times, side-by-side, no space."
Hint 2: The server reads with `readline()` — it waits for a newline.

## TOOLS:
`$ python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')" | nc <host> <port>`

## WALKTHROUGH:
1. Read `app.py`:
    - `user_input = sys.stdin.buffer.readline().rstrip(b"\n")`
    - `if user_input == b"\xff\xff\xff":` prints the flag
    - `0xFF` is not typeable, so emit raw bytes with Python

2. First attempt without newline hangs — `readline()` blocks until it sees `\n`. Add the newline:
    - `$ python3 -c "import sys; sys.stdout.buffer.write(b'\xff\xff\xff\n')" | nc lonely-island.picoctf.net <port>`
        - Answer: `picoCTF{3ff5_4_d4yz_9a6da265}`

## NOTES:
- Use `sys.stdout.buffer.write` (raw bytes), not `print` — `print` would mangle `0xFF` through text encoding.
- `readline()` needs the trailing `\n` or it waits forever. Escalation of [[bytemancy-1]]; leads to [[bytemancy-3]].
