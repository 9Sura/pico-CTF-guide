# OPERATION ORCHID PROBLEM GUIDE: 

## HINTS: 
Hint 1: Download the disk image and use `mmls` on it to find the size of the Linux partition.

Hint 2: Shell history files remember far more than they should.

## TOOLS: 
`$ gunzip <filename>.gz`

`$ mmls <image>`

`$ fls -o <offset> -r <image>`

`$ icat -o <offset> <image> <inode> > <output>`

`$ sudo kpartx -av <image>` (Linux alternative to Sleuthkit)

`$ openssl aes256 -d -salt -in <file>.enc -out <file> -k <password>`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/206/disk.flag.img.gz`
2. `$ gunzip disk.flag.img.gz`

3. `$ mmls disk.flag.img`
    - Lists the partitions. Take the start sector of the largest Linux partition as the `-o` offset below

4. `$ fls -o <offset> -r disk.flag.img | grep -i flag`
    - Finds `/root/flag.txt.enc` with its inode
    - `.enc` means the flag is encrypted — extracting it alone is not enough

5. `$ fls -o <offset> -r disk.flag.img | grep -i history`
    - Also in `/root`: `.ash_history` — the BusyBox shell history

6. `$ icat -o <offset> disk.flag.img <history-inode>`
    - Shows the exact command the author ran:
        - `openssl aes256 -salt -in flag.txt -out flag.txt.enc -k unbreakablepassword1234567`
        - That gives you both the cipher and the key

7. `$ icat -o <offset> disk.flag.img <flag-inode> > flag.txt.enc`

8. `$ openssl aes256 -d -salt -in flag.txt.enc -out flag.txt -k unbreakablepassword1234567`
9. `$ cat flag.txt`
    - Answer: `picoCTF{h4un71ng_p457_5113beab}`

## NOTES:
- On Linux you can skip Sleuthkit: `sudo kpartx -av disk.flag.img`, then mount the mapped partition and read `/root` directly.
- Newer OpenSSL warns about the default digest. If the decrypt produces garbage, retry with `-md md5`, which matches what older OpenSSL used to encrypt.
- The 8 hex characters at the end vary by download instance.
