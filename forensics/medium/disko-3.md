# DISKO 3 PROBLEM GUIDE: 

## HINTS: 
Hint 1: The file is in the filesystem, not loose in the raw bytes.

Hint 2: `strings` will not find it — it is compressed.

## TOOLS: 
`$ gunzip <filename>.gz`

`$ fdisk -l <filename>.dd`

`$ fls -o <offset> -r <image>`

`$ icat -o <offset> <image> <inode> > <output>`

`$ mount -o loop <image> /mnt/drive`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/.../disko-3.dd.gz`
2. `$ gunzip disko-3.dd.gz`

3. `$ strings disko-3.dd | grep picoCTF`
    - Nothing useful. The flag is gzip-compressed, so its bytes are not printable (Hint 2)

4. `$ fdisk -l disko-3.dd`
    - Note the start sector of the Linux partition (the offset passed to every Sleuthkit command below)

### Route A — Sleuthkit (no root, works anywhere)
5. `$ fls -o 2048 -r disko-3.dd | grep -i flag`
    - `-r` recurses the whole tree
    - Output points at `/log/flag.gz` with an inode number, e.g. `r/r 522628:  flag.gz`

6. `$ icat -o 2048 disko-3.dd 522628 > flag.gz`
    - `icat` dumps a file by inode without ever mounting the image

7. `$ gunzip flag.gz`
8. `$ cat flag`
    - Answer: `picoCTF{n3v3r_z1p_2_h1d3_654235e0}`

### Route B — mount it (Linux, needs root)
5. `$ sudo mount -o loop disko-3.dd /mnt/drive`
6. `$ cd /mnt/drive/log`
7. `$ gunzip flag.gz && cat flag`

## NOTES:
- Route A is preferred on macOS: `mount -o loop` is Linux-only, and mounting a suspect image read-write can alter evidence.
- The 8 hex characters at the end vary by download instance.
