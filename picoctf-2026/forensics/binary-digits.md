# BINARY DIGITS PROBLEM GUIDE:

*(Forensics — Easy, 100pt)*

## HINTS:
Hint 1: "Just a bunch of 1s and 0s" — but maybe it's a file.

## TOOLS:
CyberChef: `From Binary`

`$ file <output>`

## WALKTHROUGH:
1. The download is a long string of `0`/`1`. Treat the bits as bytes.

2. In CyberChef apply `From Binary`.
    - The reconstructed bytes are a JPEG (magic `FF D8 FF`)

3. Save the output as `.jpg` and open it.
    - The flag is drawn in the image
        - Answer: read `picoCTF{...}` off the rendered image (instance-specific; not published as text)

## NOTES:
- `From Binary` maps every 8 bits to one byte. Confirm the result with `file` — you should see `JPEG image data`.
- Command-line alternative: `python3 -c "import sys;open('out.jpg','wb').write(int(open('f').read().strip(),2).to_bytes(...))"` — but CyberChef is faster here.
