# WIRESHARK TWOO TWOOO TWO TWOO... PROBLEM GUIDE: 

## HINTS: 
Hint 1: Try to find the traffic that looks suspicious.

Hint 2: The `/flag` HTTP requests are a decoy — every one returns a different fake flag.

Hint 3: DNS is a data channel too.

## TOOLS: 
Wireshark display filter: `dns`

Wireshark display filter: `dns and ip.dst==<ip>`

Wireshark: `Statistics > Conversations`

`$ echo "<text>" | base64 -d`

## WALKTHROUGH: 
1. `$ wget https://mercury.picoctf.net/static/.../shark2.pcapng`
2. Open `shark2.pcapng` in Wireshark.

3. Filter `http.request.uri contains "flag"`.
    - Dozens of requests to `/flag`, each returning a different flag. All fakes — this is the decoy (Hint 2)

4. Filter `dns`.
    - A long run of lookups for subdomains of `reddshrimpandherring.com`. Requesting hundreds of random subdomains of one domain is classic DNS exfiltration (Hint 3)

5. Check where the queries are going: most go to `8.8.8.8`, but a subset go to `18.217.1.57`.
    - `18.217.1.57` is not a public resolver — that is the attacker's authoritative server

6. Filter `dns and ip.dst==18.217.1.57`.
    - Only the exfil queries remain

7. Read the subdomain label off each query in capture order and concatenate them:
    - `cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==`
        - Trailing `==` means base64

8. `$ echo "cGljb0NURntkbnNfM3hmMWxfZnR3X2RlYWRiZWVmfQ==" | base64 -d`
    - Answer: `picoCTF{dns_3xf1l_ftw_deadbeef}`

## NOTES:
- Keep the packets in capture order when concatenating. Sorting the column will scramble the base64 and it will not decode.
- General tell for DNS exfil: high query volume, one parent domain, long random-looking labels, queries sent to a non-standard resolver.
- Flag is static for this challenge.
