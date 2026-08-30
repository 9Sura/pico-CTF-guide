# FORENSICS GIT 0 PROBLEM GUIDE:

*(Forensics — Medium, 200pt)*

## HINTS:
Hint 1: Extract the directory from the disk image, then look at git history.

## TOOLS:
`$ mmls <image>`

`$ tsk_recover -e -o <offset> <image> <outdir>`

`$ git log`

## WALKTHROUGH:
1. `$ mmls disk.img`
    - Two Linux partitions (0x83). The data one starts at sector `1140736`

2. `$ mkdir extracted_files && tsk_recover -e -o 1140736 disk.img extracted_files/`
    - `tsk_recover -e` pulls **all** files (allocated + deleted) from the partition at that offset

3. `$ cd extracted_files/home/ctf-player/Code/secrets/`
    - `note.txt`: "wrap the leetspeak phrase in the flag format"

4. `$ git log`
    - The commit message holds the phrase:
        - `Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8`

5. Wrap it:
    - Answer: `picoCTF{g17_1n_7h3_d15k_041217d8}`

## NOTES:
- `tsk_recover` is the bulk-export counterpart to `fls`+`icat` — one command dumps a whole partition to disk.
- First of the git-forensics trio: [[forensics-git-1]] uses `git checkout`, [[forensics-git-2]] repairs a broken repo.
