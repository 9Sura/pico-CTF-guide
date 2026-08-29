# WIRESHARK DOO DOOO DO DOO... PROBLEM GUIDE: 

## HINTS: 
Hint 1: Try dissecting the TCP streams one at a time.

Hint 2: The flag looks wrong because it is enciphered, not encoded. Count the letter shift.

## TOOLS: 
Wireshark: `Statistics > Conversations`, `Follow > TCP Stream`

Wireshark display filter: `tcp.stream eq <n>`

`$ tr 'A-Za-z' 'N-ZA-Mn-za-m'` (ROT13 in one command)

## WALKTHROUGH: 
1. `$ wget https://mercury.picoctf.net/static/.../shark1.pcapng`
2. Open `shark1.pcapng` in Wireshark.

3. `$ strings shark1.pcapng | grep -i pico`
    - No hit — a plain grep will not solve this one, since the flag is enciphered

4. Walk the TCP streams: apply `tcp.stream eq 0`, then `1`, `2`, ... and `Follow > TCP Stream` on each.
    - Stream `5` is the interesting one:
        - `Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}`

5. `cvpbPGS` is `picoCTF` shifted by 13 — ROT13 (Hint 2).
    - `$ echo "Gur synt vf cvpbPGS{c33xno00_1_f33_h_qrnqorrs}" | tr 'A-Za-z' 'N-ZA-Mn-za-m'`
    - Reads: `The flag is picoCTF{p33kab00_1_s33_u_deadbeef}`
        - Answer: `picoCTF{p33kab00_1_s33_u_deadbeef}`

## NOTES:
- ROT13 is its own inverse, so the same `tr` command encodes and decodes.
- Quick way to spot a Caesar cipher in a capture: search for a 7-letter word followed by `{` — `[a-zA-Z]{7}\{` in the primer guide catches `cvpbPGS` as well as `picoCTF`.
- Flag is static for this challenge.
