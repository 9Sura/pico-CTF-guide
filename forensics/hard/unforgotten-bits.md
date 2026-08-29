# UNFORGOTTEN BITS PROBLEM GUIDE: 

*(picoCTF 2023, 500 points — the hardest forensics challenge in the set. Expect a long chain: disk triage, steghide, password cracking, slack-space recovery, and AES decryption.)*

## HINTS: 
Hint 1: Download the disk image and find the flag. If you are on the webshell, extract into `/tmp`, not your home directory.

Hint 2: The user's own chat logs tell you which tools were used.

Hint 3: The challenge title is the hint for the last stage — deleted and leftover bytes are still on the disk.

## TOOLS: 
`$ gunzip <filename>.gz`

`$ sudo autopsy` (or `$ sudo mount -o loop <image> /mnt/t`)

`$ fls -o <offset> -r <image>` / `$ icat -o <offset> <image> <inode>`

`$ steghide extract -sf <file>.bmp -p <password>`

`$ stegcracker <file>.bmp <wordlist>`

`$ strings <image> | grep -A 20 -B 20 <keyword>`

`$ openssl enc -aes-256-cbc -d -S <salt> -K <key> -iv <iv> -in <in> -out <out>`

## WALKTHROUGH: 

### Stage 1 — get the disk open
1. `$ wget https://artifacts.picoctf.net/c/485/disk.flag.img.gz`
2. `$ gunzip disk.flag.img.gz`
    - Large image, the decompression takes a while

3. Load it into Autopsy (`$ sudo autopsy`, then browse to `http://localhost:9999/autopsy` and add the image as a new case). Pick the largest volume and choose `File Analysis`.
    - Sleuthkit on the command line works too, but Autopsy is worth it here for the keyword search and slack-space view used in Stage 5.
    - Everything of interest lives under `/home/yone`.

### Stage 2 — read the IRC logs for the method
4. Open `/home/yone/irclogs/01/04/#avidreader13.log`.
    - The user spells out their own workflow:
        - `[08:15] <yone786> First it's steghide.`
        - `[08:15] <yone786> Use password: akalibardzyratrundle`
        - `[08:18] <yone786> The next is the encryption. Use openssl, AES, cbc.`
        - `[08:19] <yone786> salt=0f3fa17eeacd53a9 key=58593a7522257f2a95cce9a68886ff78546784ad7db4473dbd91aecd9eefd508 iv=7a12fd4dc1898efcd997a1b9496e7591`
    - The other IRC logs are League of Legends chatter. Not useless — remember it for Stage 4.

### Stage 3 — steghide the gallery
5. Export the four BMPs in `/home/yone/gallery`: `1.bmp`, `2.bmp`, `3.bmp`, `7.bmp`.

6. Run steghide on each with the password from the log:
    - `$ steghide extract -sf 1.bmp -p akalibardzyratrundle`
    - `$ steghide extract -sf 2.bmp -p akalibardzyratrundle`
    - `$ steghide extract -sf 3.bmp -p akalibardzyratrundle`
    - `$ steghide extract -sf 7.bmp -p akalibardzyratrundle`
    - The first three yield `dracula.txt.enc`, `frankenstein.txt.enc`, and `les-mis.txt.enc`
    - `7.bmp` fails — it uses a different password. That is the real target

7. Decrypt the three with the parameters from the log:
    - `$ openssl enc -aes-256-cbc -d -S 0f3fa17eeacd53a9 -K 58593a7522257f2a95cce9a68886ff78546784ad7db4473dbd91aecd9eefd508 -iv 7a12fd4dc1898efcd997a1b9496e7591 -in dracula.txt.enc -out dracula.txt`
    - Repeat for the other two. They decrypt to the full public-domain novels and nothing else — deliberate dead ends that confirm your OpenSSL invocation is correct.

### Stage 4 — crack 7.bmp
8. Read `/home/yone/notes/`:
    - `1.txt`: `chizazerite`
    - `2.txt`: `guldulheen`
    - `3.txt`: `I keep forgetting this, but it starts like: yasuoaatrox...`

9. `$ strings disk.flag.img | grep -B 20 -A 20 "Enlightened passwords"`
    - Recovers a deleted email thread (it is not in the Maildir — it was deleted, but the bytes remain)
    - It points at `https://xkcd.com/936/`: build passwords from four memorable words, and the correspondent uses names from his favourite game

10. `yasuo` and `aatrox` are League of Legends champions (Stage 2's logs). So the password is four champion names concatenated, the first two already known.
    - Build the wordlist: take a list of champion names, lowercase it, and generate every `yasuoaatrox` + champ + champ combination:
```python
#!/usr/bin/env python3
arr = open('leagueOfLegendsChampions.txt').readlines()
with open('output.txt', 'w') as f:
    for i in arr:
        for j in arr:
            f.write('yasuoaatrox' + i.strip() + j.strip() + '\n')
```

11. `$ stegcracker 7.bmp output.txt`
    - Hits on `yasuoaatroxashecassiopeia` after ~1259 tries
    - Produces `7.bmp.out` — the encrypted ledger. The Stage 2 key does not decrypt it

### Stage 5 — slack space and golden ratio base
12. This is where the title pays off. Look at the *slack space* of the notes files — the unused tail of the last disk block after the file's real content ends.
    - In Autopsy, under `Tools > Options > View`, uncheck the "hide slack files" boxes, then open `1.txt-slack`.
    - It holds a long string of binary-looking digits that is not valid base 2

13. Check `/home/yone/.lynx/` browsing history for what the user was researching:
    - `https://www.google.com/search?q=number+encodings`
    - `https://en.wikipedia.org/wiki/Church_encoding`
    - `https://www.wikiwand.com/en/Golden_ratio_base`
    - Golden ratio base (base-phi) matches the shape of the slack data: digits are `0`/`1`, but with a radix point and a fractional part

14. Decode the slack data as base-phi. Each character is one ~15-digit group (11 integer digits, a point, 3 fractional digits); sum the corresponding powers of phi (about 1.618) to get an ASCII code point.
    - This yields a second set of parameters:
        - `salt=2350e88cbeaf16c9`
        - `key=a9f86b874bd927057a05408d274ee3a88a83ad972217b81fdc2bb8e8ca8736da`
        - `iv=908458e48fc8db1c5a46f18f0feb119f`

### Stage 6 — decrypt the ledger
15. `$ openssl enc -aes-256-cbc -d -S 2350e88cbeaf16c9 -K a9f86b874bd927057a05408d274ee3a88a83ad972217b81fdc2bb8e8ca8736da -iv 908458e48fc8db1c5a46f18f0feb119f -in 7.bmp.out -out finallyReleased.txt`

16. `$ cat finallyReleased.txt | grep "picoCTF{" | tr -d ' '`
    - Answer: `picoCTF{f473_53413d_de7d35ee}`

## NOTES:
- Deliberate dead ends to not sink time into: the three decrypted novels, the spam in `Maildir/cur`, the League of Legends logs beyond establishing the game, and the second IRC log about "light".
- The single most productive move when stuck was `strings` over the whole raw image rather than browsing the mounted filesystem — deleted content only exists in the raw bytes.
- Two separate key sets are in play. The one from the IRC log decrypts the decoys; only the base-phi one from slack space decrypts the ledger.
- Slack space is the lesson of this challenge: a file's last block keeps whatever was there before, and no file browser will show it to you.
