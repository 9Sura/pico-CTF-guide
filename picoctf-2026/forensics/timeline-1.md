# TIMELINE 1 PROBLEM GUIDE:

*(Forensics — Medium, 300pt)*

## HINTS:
Hint 1: Create a Sleuthkit MAC timeline.
Hint 2: Look at recent timestamps, near an anti-forensic action.
Hint 3: Filter for `macb`.

## TOOLS:
`$ fls -r -m / <image> > timeline.body` ; `$ mactime -b timeline.body > timeline.txt`

`$ grep macb timeline.txt | tail`

`$ icat <image> <inode>`

## WALKTHROUGH:
1. Build the timeline as in [[timeline-0]]:
    - `$ fls -r -m / disk.img > timeline.body`
    - `$ mactime -b timeline.body > timeline.txt`

2. `$ grep "macb" timeline.txt | tail -n 20`
    - `macb` = all four MAC times set at once (a freshly created/stomped file). Scan the most recent block:
        - `... 32716  /etc/chat` stands out

3. `$ icat disk.img 32716`
    - `NTczNDE3aDEzcl83aDRuXzdoM18xNDU3XzU4NTI3YmIyMjIK` (base64)

4. `$ echo '...' | base64 -d`, wrap in flag format:
    - Answer: `picoCTF{573417h13r_7h4n_7h3_1457_58527bb222}`

## NOTES:
- `macb` all-set-together is the fingerprint of an anti-forensic touch; grepping for it plus `tail` isolates the planted file fast.
- Harder sequel to [[timeline-0]] — here you filter by event type instead of eyeballing an old date.
