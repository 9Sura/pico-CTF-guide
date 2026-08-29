# DISKO 2 PROBLEM GUIDE: 

## HINTS: 
Hint 1: The disk has more than one partition. The right one is Linux!

Hint 2: There are a lot of fake flags in here. Only one of them appears once.

## TOOLS: 
`$ gunzip <filename>.gz`

`$ fdisk -l <filename>.dd`

`$ mmls <filename>.dd`

`$ dd if=<image> of=<output> bs=512 skip=<start-sector> count=<sector-count>`

`$ strings <filename> | grep picoCTF`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/.../disko-2.dd.gz`
2. `$ gunzip disko-2.dd.gz`

3. `$ fdisk -l disko-2.dd`
    - `mmls disko-2.dd` gives the same partition table if you prefer Sleuthkit
    - Two partitions. The one with type `83 Linux` is the target (Hint 1)
    - Note its `Start` sector and `Sectors` count — commonly `2048` and `51200`

4. `$ dd if=disko-2.dd of=linux-part.dd bs=512 skip=2048 count=51200`
    - `bs=512` because sector offsets from fdisk are in 512-byte units
    - `skip` is the start sector, `count` is the partition length
    - Carving the partition out drops most of the decoy noise sitting elsewhere on the disk

5. `$ strings linux-part.dd | grep picoCTF`
    - Still dozens of candidates — the challenge seeds fake flags

6. `$ strings linux-part.dd | grep picoCTF | sort | uniq -c | sort -n`
    - The decoys are written many times over; the real flag is written exactly once
    - Take the entry with count `1` at the top of the list
        - Answer: `picoCTF{4_P4Rt_1t_i5_a93c3ba0}`

## NOTES:
- The 8 hex characters at the end are generated per download. Run step 6 against your own image rather than submitting the suffix above.
- If you would rather not carve manually, Autopsy can ingest `disko-2.dd` and keyword-search `picoCTF` across all partitions.
