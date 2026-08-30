# MULTICODE PROBLEM GUIDE:

*(General Skills — Medium, 200pt)*

## HINTS:
Hint 1: Nested encodings. Peel them off one layer at a time.

## TOOLS:
CyberChef (https://gchq.github.io/CyberChef/) — recipe: From Base64 -> From Hex -> URL Decode -> ROT13

## WALKTHROUGH:
Identify each layer by its shape and strip it:

1. Ciphertext ends in `=` -> **Base64**. Apply `From Base64`
2. Result is only `0-9a-f` -> **Hex**. Apply `From Hex`
3. Result has `%7B`, `%5F` -> **URL encoding**. Apply `URL Decode`
4. Result is near flag format, only letters shifted -> **ROT13**. Apply `ROT13`
    - Answer: `picoCTF{nested_enc0ding_1d75be63}`

## NOTES:
- Recognition cheatsheet: trailing `=` -> base64; only `0-9a-f` -> hex; `%NN` -> URL; readable-but-shifted -> ROT13/Caesar.
- CyberChef stacks these as a pipeline so you watch the flag emerge layer by layer.
