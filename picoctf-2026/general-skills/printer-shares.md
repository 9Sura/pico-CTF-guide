# PRINTER SHARES PROBLEM GUIDE:

*(General Skills — Easy, 50pt)*

## HINTS:
Hint 1: Connect over SMB.

## TOOLS:
`$ smbclient -L //<host>/ -p <port> -N`

`$ smbclient //<host>/<share> -p <port> -N`

## WALKTHROUGH:
1. `$ smbclient -L //mysterious-sea.picoctf.net/ -p <port> -N`
    - `-L` lists shares, `-N` = no password
    - `shares    Disk    Public Share With Guests` looks promising

2. `$ smbclient //mysterious-sea.picoctf.net/shares -p <port> -N`
    - `smb: \> ls` shows `flag.txt`

3. `smb: \> get flag.txt`
4. `smb: \> exit` then `$ cat flag.txt`
    - Answer: `picoCTF{5mb_pr1nter_5h4re5_2f61915b}`

## NOTES:
- SMB = Server Message Block, Windows/Samba file sharing on port 445. `smbclient` is the interactive client; commands inside it (`ls`, `get`, `put`) behave like FTP.
- First of the printer-shares series ([[printer-shares-2]], [[printer-shares-3]]) — later parts add auth and cron abuse.
