# PCAPPOISONING PROBLEM GUIDE: 

## HINTS: 
Hint 1: How about some hide and seek heh?

Hint 2: The capture is padded with noise — filter, do not scroll.

## TOOLS: 
`$ strings <filename>.pcap | grep pico`

Wireshark display filter: `frame contains "picoCTF"`

Wireshark display filter: `tcp contains "pico"`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/482/trace.pcap`

2. `$ file trace.pcap`
    - libpcap capture. Thousands of packets, almost all filler

### Route A — command line
3. `$ strings trace.pcap | grep -i pico`
    - The flag is in cleartext in one TCP payload, so one grep is enough
        - Answer: `picoCTF{P64P_4N4L7S1S_SU55355FUL_31010c46}`

### Route B — Wireshark
3. Open `trace.pcap` in Wireshark.
4. Apply the display filter `frame contains "picoCTF"`.
    - Exactly one packet survives the filter (around packet #507).
5. Select it and read the payload in the bytes pane, or right-click -> `Follow` -> `TCP Stream`.

## NOTES:
- `frame contains` searches every byte of the packet; `tcp contains` searches only the TCP segment. Start broad with `frame contains`.
- The 8 hex characters at the end vary by download instance.
