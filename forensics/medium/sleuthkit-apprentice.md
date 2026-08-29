# SLEUTHKIT APPRENTICE PROBLEM GUIDE: 

## HINTS: 
Hint 1: Download the disk image and find the flag.

Hint 2: There is more than one partition — check each one before giving up.

## TOOLS: 
`$ gzip -d <filename>.gz`

`$ mmls <image>`

`$ fsstat -o <offset> <image>`

`$ fls -o <offset> -r <image>`

`$ icat -o <offset> <image> <inode>`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/216/disk.flag.img.gz`
2. `$ gzip -d disk.flag.img.gz`

3. `$ mmls disk.flag.img`
    - Three partitions. The largest Linux one starts at sector `360448` — that is the `-o` offset for everything below
    - `$ fsstat -o 360448 disk.flag.img` confirms it is a valid ext filesystem before you dig in

4. `$ fls -o 360448 -r disk.flag.img | grep -i flag`
    - `-r` recurses; without it you only see the root directory
    - Returns two `.txt` candidates with their inode numbers, e.g. `r/r 2371: flag.txt`

5. `$ icat -o 360448 disk.flag.img 2371`
    - Dumps the file contents straight to the terminal
        - Answer: `picoCTF{by73_5urf3r_3497ae6b}`

## NOTES:
- If `icat` prints nothing, you used the wrong offset. Every Sleuthkit command must share the same `-o` value from `mmls`.
- One of the two `.txt` hits is a decoy in a different partition — check both.
- The 8 hex characters at the end vary by download instance.
