# FORENSICS GIT 1 PROBLEM GUIDE:

*(Forensics — Medium, 300pt)*

## HINTS:
Hint 1: How do you check out the files of a previous commit?

## TOOLS:
`$ tsk_recover -e -o <offset> <image> <outdir>`

`$ git log`

`$ git checkout <commit>`

## WALKTHROUGH:
1. Recover the partition as in [[forensics-git-0]]:
    - `$ tsk_recover -e -o <offset> disk.img extracted_files/`
    - `$ cd extracted_files/home/ctf-player/Code/secrets/`

2. `$ git log`
    - Two commits:
        - `5fb81945... Remove flag` (HEAD)
        - `177789af... Add flag`
    - The flag was committed then removed — go back to the "Add flag" state

3. `$ git checkout 177789af0b300e043ea8f54ea57d6cee352291ae`
    - `$ ls` -> `flag.txt`

4. `$ cat flag.txt`
    - Answer: `picoCTF{...}` (instance-specific — read your own checkout)

## NOTES:
- Deleting a file in a later commit doesn't erase it from history. `git checkout <old-commit>` restores the working tree to that point.
- Also viewable without switching: `git show 177789af:flag.txt`.
