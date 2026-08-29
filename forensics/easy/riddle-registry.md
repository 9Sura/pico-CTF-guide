# RIDDLE REGISTRY PROBLEM GUIDE: 

## HINTS: 
Hint 1: Not everything that is written is meant to be read.

Hint 2: The visible text is a decoy — look at the file's metadata.

## TOOLS: 
`$ pdfinfo <filename>.pdf`

`$ exiftool <filename>.pdf`

`$ strings <filename>.pdf | less`

`$ echo "<text>" | base64 -d`

## WALKTHROUGH: 
1. Download `confidential.pdf` from the challenge page.

2. `$ open confidential.pdf`
    - Pages of redacted / garbled riddle text. This is the decoy the description warns about

3. `$ strings confidential.pdf | grep -i pico`
    - No hit. The flag is not sitting in the page content stream

4. `$ pdfinfo confidential.pdf`
    - Or `$ exiftool confidential.pdf` for the same information plus the XMP block
    - `Author:         cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8zNTc4NzM5YX0=`
        - Trailing `=` padding is the giveaway for base64

5. `$ echo "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV8zNTc4NzM5YX0=" | base64 -d`
    - Answer: `picoCTF{puzzl3d_m3tadata_f0und!_3578739a}`

## NOTES:
- PDF metadata fields worth checking: `Author`, `Title`, `Subject`, `Keywords`, `Creator`, `Producer`, and the raw XMP packet.
- The trailing 8 hex characters are generated per challenge instance — if your download differs, decode your own Author field rather than pasting the flag above.
