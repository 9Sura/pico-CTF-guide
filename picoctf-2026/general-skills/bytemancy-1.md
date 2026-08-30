# BYTEMANCY 1 PROBLEM GUIDE:

*(General Skills — Easy, 100pt)*

## HINTS:
Hint 1: "Send ASCII DECIMAL 101 1751 times, side-by-side, no space."

## TOOLS:
`$ python3 -c "print('e' * 1751)" | nc <host> <port>`

Shell pipe `|`

## WALKTHROUGH:
1. Read `app.py`:
    - `if user_input == "\x65"*1751:` prints the flag
    - `\x65` = `e`, so you must send the letter `e` 1751 times in a row

2. Generate and pipe it (typing 1751 by hand is impractical):
    - `$ python3 -c "print('e' * 1751)" | nc foggy-cliff.picoctf.net <port>`
    - The pipe feeds Python's output straight into nc's stdin
        - Answer: `picoCTF{h0w_m4ny_e's???_e0d51f4b}`

## NOTES:
- `'e' * 1751` in Python repeats the string; `print` adds the newline the server's `input()` needs.
- Sequel to [[bytemancy-0]]. Next: [[bytemancy-2]] moves to raw non-printable bytes.
