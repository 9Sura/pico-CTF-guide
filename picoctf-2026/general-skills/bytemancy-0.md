# BYTEMANCY 0 PROBLEM GUIDE:

*(General Skills — Easy, 50pt)*

## HINTS:
Hint 1: "Send me ASCII DECIMAL 101, 101, 101, side-by-side, no space."
Hint 2: ASCII 101 = the letter `e`.

## TOOLS:
`$ nc <host> <port>`

ASCII table

## WALKTHROUGH:
1. Read `app.py` (provided):
    - `if user_input == "\x65\x65\x65":` prints the flag
    - `\x65` is hex for decimal 101, which is the character `e`

2. `$ nc candy-mountain.picoctf.net <port>`
    - At the `==>` prompt type: `eee`
        - Answer: `picoCTF{pr1n74813_ch4r5_2f7a75e5}`

## NOTES:
- ASCII decimal 101 = hex 0x65 = `e`. Three of them side by side = `eee`.
- First of the `bytemancy` series ([[bytemancy-1]], [[bytemancy-2]], [[bytemancy-3]]) — each escalates how you send bytes.
