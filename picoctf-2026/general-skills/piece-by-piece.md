# PIECE BY PIECE PROBLEM GUIDE:

*(General Skills — Easy, 50pt)*

## HINTS:
Hint 1: The flag is split into parts of a zip. Combine them with Linux commands.
Hint 2: The zip is password protected — password is `supersecret`.

## TOOLS:
`$ cat <parts> > combined.zip`

`$ unzip -P <password> combined.zip`

## WALKTHROUGH:
1. `$ cat instructions.txt`
    - Confirms: parts of a zip, combine them, extract with password `supersecret`

2. `$ cat part_* > combined.zip`
    - `cat` = con**cat**enate; the shell expands `*` in alphabetical order, so the parts join in order
    - `part_a`, `part_b`, ... must be named so alphabetical order equals correct order

3. `$ unzip -P supersecret combined.zip`
    - `-P` supplies the password non-interactively

4. `$ cat flag.txt`
    - Answer: `picoCTF{z1p_and_spl1t_f1l3s_4r3_fun_27804340}`

## NOTES:
- If the parts are numbered past 9 (`part_10`), plain `*` sorts them wrong (`10` before `2`). Use `ls -v` ordering or `cat part_{1..N}` instead.
- Verify the join with `$ file combined.zip` before extracting — it should say `Zip archive data`.
