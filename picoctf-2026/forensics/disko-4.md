# DISKO 4 PROBLEM GUIDE:

*(Forensics — Medium, 200pt)*

## HINTS:
Hint 1: The file was deleted. How do you recover deleted files?
Hint 2: It's compressed, so `strings` won't find it.

## TOOLS:
`$ gunzip <image>.gz`

`$ fls -r -d <image>`

`$ icat <image> <inode> > out.gz`

`$ gzip -dc out.gz`

## WALKTHROUGH:
1. `$ gunzip disko-4.dd.gz`

2. `$ fls -r -d disko-4.dd`
    - `-d` lists **deleted** entries only:
        - `r/r * 532021:  log/dont-delete.gz`
    - The `*` marks it deleted; the `.gz` name is the target

3. `$ icat disko-4.dd 532021 > dont-delete.gz`
    - Recover the deleted file's contents by inode

4. `$ gzip -dc dont-delete.gz`
    - `Here is your flag` followed by:
        - Answer: `picoCTF{d3l...}` (instance-specific suffix — decompress your own recovery)

## NOTES:
- `icat` reads a file by inode even after deletion, as long as the data blocks haven't been reused.
- Latest in the disko series (see forensics/easy/[[disko-1]], forensics/medium/[[disko-2]], [[disko-3]] in the main guide): strings -> partition carve -> filesystem -> deleted-file recovery.
