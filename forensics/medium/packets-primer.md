# PACKETS PRIMER PROBLEM GUIDE: 

## HINTS: 
Hint 1: Download the packet capture and use packet analysis software to find the flag.

Hint 2: The flag is sitting in the payload in plaintext — it is just split across packets.

## TOOLS: 
`$ strings <filename>.pcap`

`$ tcpdump -r <filename>.pcap -A`

Wireshark: `Follow > TCP Stream`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c/243/network-dump.flag.pcap`

2. `$ file network-dump.flag.pcap`
    - Confirms it is a libpcap capture

### Route A — command line
3. `$ strings network-dump.flag.pcap | grep -i pico`
    - The flag is transmitted one character per packet, so `strings` shows it spaced out: `p i c o C T F { ...`

4. `$ strings network-dump.flag.pcap | grep "p i c o" | tr -d ' '`
    - `tr -d ' '` strips the spacing introduced by the per-packet split
        - Answer: `picoCTF{p4ck37_5h4rk_01b0a0d6}`

### Route B — Wireshark
3. Open `network-dump.flag.pcap` in Wireshark.
4. Right-click any TCP packet -> `Follow` -> `TCP Stream`.
    - Wireshark reassembles the conversation and prints the flag in reading order.

## NOTES:
- This is the intro packet challenge. Get comfortable with `Follow TCP Stream` here — `wireshark-doo` and `wireshark-two` both build on it.
- Useful display filters: `frame contains "picoCTF"`, `tcp contains "pico"`.
- The 8 hex characters at the end vary by download instance.
