# CORRUPTED FILE PROBLEM GUIDE: 

## HINTS:
Hint 1: Try checking the file’s header.

Hint 2: JPEG

Hint 3: Tools like xxd or hexdump can help you inspect and edit file bytes.

## TOOLS:
`xxd <filename> > <filename>.hex`

`xxd -r <filename>.hex > <filename>.jpg/png/pdf/etc.`

## WALKTHROUGH: 
1. `$ file file`
    - Output: `file: data`
        - Confirms file is a data (bytes) file

2. `$ xxd file > file.hex`
    - Converts file to hex (readable & editable bytes)

3. `open -e file.hex`
    - Output (1st line): `00000000: 5c78 ffe0 0010 4a46 4946 0001 0100 0001`
        - Hint 2: JPEG 
        - Standard JPEG file header: `FF D8 FF E0 00 10 4A 46 49 46 00`
            - Change `5c78` -> `ffd8`

4. `$ xxd -r file.hex > fixed.jpg`
    - Reverses hexdump file to jpg 

5. `$ open fixed.jpg`
    - Answer: `picoCTFlr3st0r1ng_th3_by73s.2326ca93)`