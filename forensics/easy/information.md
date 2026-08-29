# INFORMATION PROBLEM GUIDE: 

## HINTS: 
Hint 1: Look at the details of the file.

Hint 2: Make sure to submit the flag as picoCTF{XXXXX}

## TOOLS: 
`$ exiftool <filename>`

`$ echo "<text>" | base64 -d`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/226/cat.jpg`

2. `$ file cat.jpg`
    - Output: `cat.jpg: JPEG image data, JFIF standard 1.01 ...`
        - Nothing corrupted, so the payload is not in the pixels — check metadata

3. `$ exiftool cat.jpg`
    - Scan every field, not just the obvious ones
    - `License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9`
        - A licence field is normally a URL or a short phrase. A long mixed-case blob ending in a letter/number run is base64

4. `$ echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d`
    - Answer: `picoCTF{the_m3tadata_1s_modified}`

## NOTES:
- Same pattern as `canyousee` and `hidden-in-plainsight`: exiftool first, then decode whatever field looks out of place.
- Common hiding fields to check: `Comment`, `Artist`, `Copyright`, `License`, `XMP Toolkit`, `Description`.
