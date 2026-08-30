# MY GIT PROBLEM GUIDE:

*(General Skills — Easy, 50pt)*

## HINTS:
Hint 1: Only a commit by `root` with email `root@picoctf` gets the flag.

## TOOLS:
`$ git config user.name` / `user.email`

`$ git add / commit / push`

## WALKTHROUGH:
1. `$ cat README.md`
    - "Only flag.txt pushed by `root:root@picoctf` will be updated with the flag."
    - The server checks the commit author, not who authenticates — so impersonate that identity

2. Set the author identity:
    - `$ git config user.name "root"`
    - `$ git config user.email "root@picoctf"`

3. Create, stage, commit, push:
    - `$ echo 'dummy' > flag.txt`
    - `$ git add flag.txt`
    - `$ git commit -m "add flag.txt"`
    - `$ git push`
    - Server matches the author and returns the flag in the push output:
        - Answer: `picoCTF{1mp3rs0n4t4_g17_345y_05f9a904}`

## NOTES:
- Git commit authorship is self-declared metadata with no verification (unless commits are GPG-signed). Anyone can set any `user.name`/`user.email`.
- Lesson: never trust commit author fields for access control.
