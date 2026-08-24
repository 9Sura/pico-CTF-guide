# picoCTF Primer Guide

Notes and challenge write-ups for [picoCTF](https://play.picoctf.org/practice), organized
by category and difficulty. Each category has a `primer-guide.md` covering the background
concepts, plus one file per challenge.

## Categories

### [Cryptology](cryptology)
- [Primer guide](cryptology/primer-guide.md) — encoding vs. encryption, classical ciphers,
  frequency analysis, and the tools to break them.

### [The Shell](the-shell)
- [Primer guide](the-shell/primer-guide.md) — command line basics for CTF work.

### [Forensics](forensics)
- [Primer guide](forensics/primer-guide.md) — file signatures, metadata, steganography,
  disk images, and packet captures.

| Easy | Medium | Hard |
| --- | --- | --- |
| [canyousee](forensics/easy/canyousee.md) | [disko-2](forensics/medium/disko-2.md) ▪ | [unforgotten-bits](forensics/hard/unforgotten-bits.md) ▪ |
| [corrupted-file](forensics/easy/corrupted-file.md) | [disko-3](forensics/medium/disko-3.md) ▪ | |
| [disko-1](forensics/easy/disko-1.md) | [operation-orchid](forensics/medium/operation-orchid.md) ▪ | |
| [flags-in-flame](forensics/easy/flags-in-flame.md) | [packets-primer](forensics/medium/packets-primer.md) ▪ | |
| [hidden-in-plainsight](forensics/easy/hidden-in-plainsight.md) | [pcap-poisoning](forensics/medium/pcap-poisoning.md) ▪ | |
| [information](forensics/easy/information.md) ▪ | [sleuthkit-intro](forensics/medium/sleuthkit-intro.md) ▪ | |
| [red](forensics/easy/red.md) ▪ | [sleuthkit-apprentice](forensics/medium/sleuthkit-apprentice.md) ▪ | |
| [riddle-registry](forensics/easy/riddle-registry.md) ▪ | [sleuthkit-ii](forensics/medium/sleuthkit-ii.md) ▪ | |
| | [wireshark-doo](forensics/medium/wireshark-doo.md) ▪ | |
| | [wireshark-two](forensics/medium/wireshark-two.md) ▪ | |

### [Web Exploitation](web-exploitation)
- [Primer guide](web-exploitation/primer-guide.md) — HTML, JavaScript, HTTP, servers, and
  cross-site scripting.
- [Tools and commands](web-exploitation/easy/tools-and-commands.md)
- [Burp Suite](web-exploitation/easy/burp-suite.md)

| Easy | Medium |
| --- | --- |
| [bookmarklet](web-exploitation/easy/bookmarklet.md) | [soap](web-exploitation/medium/soap.md) |
| [cookies](web-exploitation/easy/cookies.md) | |
| [get-a-head](web-exploitation/easy/get-a-head.md) | |
| [head-dump](web-exploitation/easy/head-dump.md) | |
| [how-to-steal-cookie-monsters-stuff](web-exploitation/easy/how-to-steal-cookie-monsters-stuff.md) | |
| [includes](web-exploitation/easy/includes.md) | |
| [intro-to-burp](web-exploitation/easy/intro-to-burp.md) | |
| [local-authority](web-exploitation/easy/local-authority.md) | |
| [scavenger-hunt](web-exploitation/easy/scavenger-hunt.md) | |
| [ssti-1](web-exploitation/easy/ssti-1.md) | |
| [unminify](web-exploitation/easy/unminify.md) | |
| [web-decode](web-exploitation/easy/web-decode.md) | |
| [where-are-the-robots](web-exploitation/easy/where-are-the-robots.md) | |

▪ = placeholder, not written yet.

## Naming conventions

- Directories and files are lowercase `kebab-case`. This `README.md` is the only exception.
- Each category is a top-level directory named after the picoCTF category.
- Difficulty tiers are `easy/`, `medium/`, and `hard/` directories inside a category.
- Category background notes live in `primer-guide.md`; every other markdown file is a
  single challenge, named after that challenge.
- Images live in an `images/` directory beside the documents that use them, and are named
  after the challenge they illustrate (`bookmarklet-1.png`), or `figure-N` for the
  illustrations in a primer guide.
- Image links are relative to the document (`./images/...`).
