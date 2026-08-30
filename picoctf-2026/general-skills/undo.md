# UNDO PROBLEM GUIDE:

*(General Skills — Easy, 100pt)*

## HINTS:
Hint 1: Each step applied one transformation. Enter the Linux command that reverses it.
Hint 2: Reversing a transform sometimes means swapping the two arguments.

## TOOLS:
`base64 -d`, `rev`, `tr`

`$ nc <host> <port>`

## WALKTHROUGH:
Connect: `$ nc foggy-cliff.picoctf.net <port>`. Each step shows the transformed flag plus a hint; type the reversing command.

1. Step 1 "Base64 encoded" -> `base64 -d` (plain `base64` re-encodes; you need decode)
2. Step 2 "Reversed the text" -> `rev`
3. Step 3 "Replaced underscores with dashes" -> `tr '-' '_'`
    - Order matters: to undo `tr '_' '-'` you swap the sets
4. Step 4 "Replaced curly braces with parentheses" -> `tr '()' '{}'`
5. Step 5 "Applied ROT13" -> `tr 'n-za-mN-ZA-M' 'a-zA-Z'`
    - Swap the ROT13 map to reverse it
    - Final:
        - Answer: `picoCTF{Revers1ng_t3xt_Tr4nsf0rm@t10ns_8deed4b6}`

## NOTES:
- `tr` reverses by swapping its two SET arguments. ROT13 is self-inverse but `tr`'s explicit maps still must be swapped to match the challenge's checker.
- If unsure, the challenge helpfully prints `Try reversing: <cmd>` after a wrong answer — swap that command's args.
