# DISK, DISK, SLEUTH! II PROBLEM GUIDE: 

*(picoCTF 2021 — the follow-up to Sleuthkit Intro / Apprentice)*

## HINTS: 
Hint 1: All we know is the file with the flag is named `down-at-the-bottom.txt`.

Hint 2: You do not need to mount anything — Sleuthkit reads the filesystem directly.

## TOOLS: 
`$ gzip -d <filename>.gz`

`$ mmls <image>`

`$ fls -o <offset> <image> [inode]`

`$ fls -o <offset> -r <image> | grep <name>`

`$ icat -o <offset> <image> <inode>`

## WALKTHROUGH: 
1. `$ wget https://mercury.picoctf.net/static/.../dds2-alpine.flag.img.gz`
2. `$ gzip -d dds2-alpine.flag.img.gz`

3. `$ mmls dds2-alpine.flag.img`
    - A single Linux partition starting at sector `2048` — that is the `-o` offset for every command below

4. `$ fls -o 2048 -r dds2-alpine.flag.img | grep down-at-the-bottom`
    - Straight to the inode, e.g. `r/r 18291: down-at-the-bottom.txt`
    - Slower manual route, if you want to see the tree: `$ fls -o 2048 dds2-alpine.flag.img` lists the root, then `$ fls -o 2048 dds2-alpine.flag.img 18290` descends into the directory holding the file

5. `$ icat -o 2048 dds2-alpine.flag.img 18291`
    - Answer: `picoCTF{f0r3ns1c4t0r_n0v1c3_69ab1dc8}`

## NOTES:
- "Down at the bottom" is a hint about the inode number: the file was written last, so it sits at a high inode. `fls -r ... | tail` also surfaces it.
- Same three-command pattern as every disk challenge in this folder: `mmls` for the offset, `fls` for the inode, `icat` for the contents.
- The 8 hex characters at the end vary by download instance.
