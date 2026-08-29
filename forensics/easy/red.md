# RED PROBLEM GUIDE: 

## HINTS: 
Hint 1: Read the metadata — the capital letters in the poem spell something out.

Hint 2: LSB steganography. `zsteg` sweeps every bit plane for you.

## TOOLS: 
`$ exiftool <filename>`

`$ zsteg <filename>.png`

`$ zsteg -E <payload-name> <filename>.png`

`$ base64 -d`

Install: `brew install zsteg` (or `gem install zsteg`)

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/558/red.png`

2. `$ open red.png`
    - A plain red square. No visible content, so the data is in the bits, not the picture

3. `$ exiftool red.png`
    - A poem sits in the metadata. Reading only its capital letters spells `CHECK LSB`
        - Confirms least-significant-bit steganography

4. `$ zsteg red.png`
    - zsteg walks each bit plane / channel / pixel-order combination and prints anything that decodes to text
    - The interesting row is the long one:
        - `b1,rgba,lsb,xy .. text: "<base64 blob>"`

5. `$ zsteg -E b1,rgba,lsb,xy red.png | base64 -d`
    - Or copy the base64 blob out and run `$ echo "<blob>" | base64 -d`
    - Answer: `picoCTF{r3d_1s_th3_ult1m4t3_cur3_f0r_54dn355_}`

## NOTES:
- `zsteg` is PNG/BMP only. For JPEG use `steghide` (as in `hidden-in-plainsight`) — JPEG's lossy compression destroys LSB payloads.
- `b1,rgba,lsb,xy` reads as: 1 bit deep, all four channels, least-significant bit first, scanned left-to-right then top-to-bottom.
