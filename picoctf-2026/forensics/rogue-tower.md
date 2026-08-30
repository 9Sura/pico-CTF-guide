# ROGUE TOWER PROBLEM GUIDE:

*(Forensics — Medium, 300pt)*

## HINTS:
Hint 1: Look for unauthorized test-network broadcasts on UDP port 55000.
Hint 2: Find the victim by its HTTP User-Agent.
Hint 3: The encryption key is derived from the victim's IMSI.
Hint 4: The exfil data is split across multiple HTTP POSTs.

## TOOLS:
Wireshark filters; `File > Export Objects > HTTP`

Python (base64 + XOR)

## WALKTHROUGH:
1. Filter `udp.port == 55000`.
    - A packet reads `UNAUTHORIZED-TEST-NETWORK PLMN=00101 CELLID=96261` — the rogue tower's CELLID is `96261`

2. Filter `http.user_agent contains "96261"`.
    - `MobileDevice/1.0 (IMSI:310410185261676; CELL:96261)` — victim IMSI `310410185261676`, source IP `10.100.77.215`

3. Filter `ip.src == 10.100.77.215`. Six POSTs to `/upload`. `File > Export Objects > HTTP`, save all, `cat` them together:
    - `SFxRWXJicU1KBVVDAmlUBVRZbUIBQQREZwJXD1VQBAcBSA==` (base64, but decodes to binary)

4. Hint 3: key = last 8 digits of IMSI (`85261676`). Base64-decode then XOR:
```python
import base64
data = base64.b64decode("SFxRWXJicU1KBVVDAmlUBVRZbUIBQQREZwJXD1VQBAcBSA==")
key = b"85261676"
print(bytes(data[i] ^ key[i % len(key)] for i in range(len(data))).decode())
```
    - Answer: `picoCTF{...}` (instance-specific — run against your own capture)

## NOTES:
- Multi-stage PCAP: pivot filter to filter (rogue broadcast -> User-Agent -> victim IP -> exfil objects), each hint naming the next filter.
- Reassemble the split POST bodies in capture order before decoding, or the base64 breaks.
