# picoCTF 2026 — Writeups

Writeups for every picoCTF 2026 challenge **except Web Exploitation**, in the same format as the rest of this guide (`## HINTS / ## TOOLS / ## WALKTHROUGH / ## NOTES`).

Organized by category. 60 challenges across 6 categories.

## General Skills (17)
- [SUDO MAKE ME A SANDWICH](general-skills/sudo-make-me-a-sandwich.md) — 50 — sudo emacs
- [Piece by Piece](general-skills/piece-by-piece.md) — 50 — cat split zip
- [bytemancy 0](general-skills/bytemancy-0.md) — 50 — ASCII `eee`
- [Printer Shares](general-skills/printer-shares.md) — 50 — smbclient guest
- [MY GIT](general-skills/my-git.md) — 50 — git author spoof
- [Password Profiler](general-skills/password-profiler.md) — 100 — cupp wordlist
- [KSECRETS](general-skills/ksecrets.md) — 100 — kubectl secrets -A
- [ping-cmd](general-skills/ping-cmd.md) — 100 — command injection
- [bytemancy 1](general-skills/bytemancy-1.md) — 100 — 1751×`e`
- [Undo](general-skills/undo.md) — 100 — reverse transforms
- [MultiCode](general-skills/multicode.md) — 200 — nested encodings
- [bytemancy 2](general-skills/bytemancy-2.md) — 200 — raw 0xFF bytes
- [Printer Shares 2](general-skills/printer-shares-2.md) — 200 — SMB brute force
- [ABSOLUTE NANO](general-skills/absolute-nano.md) — 200 — sudo nano sudoers
- [Failure Failure](general-skills/failure-failure.md) — 200 — HAProxy failover
- [Printer Shares 3](general-skills/printer-shares-3.md) — 300 — cron script abuse
- [bytemancy 3](general-skills/bytemancy-3.md) — 400 — nm + p32 addresses

## Cryptography (12)
- [Shared Secrets](cryptography/shared-secrets.md) — 100 — DH leaked exponent
- [StegoRSA](cryptography/stegorsa.md) — 100 — RSA key in JPEG
- [cryptomaze](cryptography/cryptomaze.md) — 100 — LFSR -> AES key
- [Black Cobra Pepper](cryptography/black-cobra-pepper.md) — 200 — AES sans S-box (linear)
- [Related Messages](cryptography/related-messages.md) — 200 — Franklin-Reiter
- [shift registers](cryptography/shift-registers.md) — 200 — 8-bit LFSR brute
- [Small Trouble](cryptography/small-trouble.md) — 200 — Wiener's attack
- [Timestamped Secrets](cryptography/timestamped-secrets.md) — 200 — time-derived AES key
- [Secure Dot Product](cryptography/secure-dot-product.md) — 300 — SHA-512 length extension
- [ClusterRSA](cryptography/clusterrsa.md) — 400 — multi-prime RSA
- [MSS ADVANCE Revenge](cryptography/mss-advance-revenge.md) — 400 — LLL lattice
- [Not TRUe](cryptography/not-true.md) — 400 — NTRU lattice attack

## Forensics (8)
- [Binary Digits](forensics/binary-digits.md) — 100 — binary -> JPEG
- [Timeline 0](forensics/timeline-0.md) — 100 — Sleuthkit MAC timeline
- [DISKO 4](forensics/disko-4.md) — 200 — recover deleted file
- [Forensics Git 0](forensics/forensics-git-0.md) — 200 — tsk_recover + git log
- [Forensics Git 1](forensics/forensics-git-1.md) — 300 — git checkout old commit
- [Rogue Tower](forensics/rogue-tower.md) — 300 — PCAP + IMSI XOR
- [Timeline 1](forensics/timeline-1.md) — 300 — timeline macb filter
- [Forensics Git 2](forensics/forensics-git-2.md) — 400 — rebuild broken git repo

## Reverse Engineering (11)
- [Gatekeeper](reverse-engineering/gatekeeper.md) — 100 — hex input gate
- [Hidden Cipher 1](reverse-engineering/hidden-cipher-1.md) — 100 — XOR key S3Cr3t
- [Hidden Cipher 2](reverse-engineering/hidden-cipher-2.md) — 100 — multiply-by-answer
- [Bypass Me](reverse-engineering/bypass-me.md) — 100 — decode runtime password
- [Silent Stream](reverse-engineering/silent-stream.md) — 200 — +42 byte transform
- [Autorev 1](reverse-engineering/autorev-1.md) — 200 — auto-parse 20 ELFs
- [Secure Password Database](reverse-engineering/secure-password-database.md) — 200 — djb2 hash
- [The Add-On Trap](reverse-engineering/the-add-on-trap.md) — 200 — Fernet in .xpi
- [Binary Instrumentation 3](reverse-engineering/binary-instrumentation-3.md) — 300 — Frida unpack dump
- [Binary Instrumentation 4](reverse-engineering/binary-instrumentation-4.md) — 400 — Frida hook connect/lstrcmp
- [JITFP](reverse-engineering/jitfp.md) — 500 — JIT comparator breakpoints

## Binary Exploitation (8)
- [Quizploit](binary-exploitation/quizploit.md) — 50 — pwn quiz
- [Echo Escape 1](binary-exploitation/echo-escape-1.md) — 100 — 64-bit ret2win
- [Echo Escape 2](binary-exploitation/echo-escape-2.md) — 100 — 32-bit ret2win
- [tea-cash](binary-exploitation/tea-cash.md) — 100 — tcache traversal
- [Heap Havoc](binary-exploitation/heap-havoc.md) — 200 — heap overflow + GOT overwrite
- [offset-cycle](binary-exploitation/offset-cycle.md) — 300 — timed ret2win, cyclic
- [offset-cycleV2](binary-exploitation/offset-cyclev2.md) — 400 — known-canary bypass
- [Pizza Router](binary-exploitation/pizza-router.md) — 400 — leaks + heap fptr overwrite

## Blockchain (4)
- [Access Control](blockchain/access-control.md) — 200 — unprotected changeOwner
- [Front Running](blockchain/front-running.md) — 300 — mempool front-run
- [Smart Overflow](blockchain/smart-overflow.md) — 300 — uint256 overflow
- [Reentrance](blockchain/reentrance.md) — 400 — reentrancy drain

---

**Note on flags:** picoCTF regenerates the trailing hash of most flags per instance, so where a walkthrough shows `picoCTF{...}` the exact suffix is instance-specific — run the steps against your own download. Static/confirmed flags are shown in full (all General Skills, plus StegoRSA, Timeline 0/1, Forensics Git 0, Binary Instrumentation 3/4, shift registers).
