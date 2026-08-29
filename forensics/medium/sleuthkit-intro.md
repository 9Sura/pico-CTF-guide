# SLEUTHKIT INTRO PROBLEM GUIDE: 

## HINTS: 
Hint 1: Download the disk image and use `mmls` on it to find the size of the Linux partition.

Hint 2: The answer you submit to the remote service is the partition *length*, not its start offset.

## TOOLS: 
`$ gunzip <filename>.gz`

`$ mmls <image>`

`$ nc <host> <port>`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/245/disk.img.gz`
2. `$ gunzip disk.img.gz`

3. `$ mmls disk.img`
    - Output is a partition table, one row per slot:
        - `Slot   Start        End          Length       Description`
        - The row described as `Linux (0x83)` is the one you want
    - Read the `Length` column for that row — this is the size in sectors, e.g. `202752`
        - Not the `Start` column, and not the `End` column (Hint 2)

4. `$ nc saturn.picoctf.net <port>`
    - Port is listed on the challenge page
    - The service asks for the size of the Linux partition
    - Enter `202752`
        - Answer: `picoCTF{mm15_f7w!}`

## NOTES:
- `mmls` reads only the partition table, so it works on a disk image you cannot or should not mount.
- Sector counts are 512 bytes each, so `202752` sectors is about 99 MB. Multiply if the checker asks for bytes instead.
- This flag is static — there is no per-instance hash suffix on this one.
