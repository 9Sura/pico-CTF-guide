# SILENT STREAM PROBLEM GUIDE:

*(Reverse Engineering — Medium, 200pt)*

## HINTS:
Hint 1: Focus on what the encoding script does to each byte.
Hint 2: Reconstruct the file and open it — don't trust the flag format blindly.

## TOOLS:
Wireshark (`Follow Stream` -> save Raw) ; Python

## WALKTHROUGH:
1. `encrypt.py` transfers `flag.txt` after transforming each byte: `(b + 42) % 256`.

2. In Wireshark, right-click the TCP stream -> `Follow TCP Stream` -> save as Raw (`encoded.bin`).

3. Reverse the transform (`(b - 42) % 256`) and write bytes:
```python
enc = open("encoded.bin","rb").read()
open("decoded.bin","wb").write(bytes((b - 42) % 256 for b in enc))
```

4. `$ file decoded.bin` -> `JPEG image data`. Rename to `.jpg` and open:
    - The flag is in the image (Hint 3: it isn't plain text)
        - Answer: `picoCTF{...}` (read off your reconstructed image)

## NOTES:
- The transferred payload is a file, not text — reconstruct then `file` it before assuming anything.
- `(b + 42) % 256` is a Caesar shift on bytes; subtract to invert.
