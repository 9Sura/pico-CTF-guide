# PASSWORD PROFILER PROBLEM GUIDE:

*(General Skills — Easy, 100pt)*

## HINTS:
Hint 1: Build a targeted wordlist from the victim's personal info.

## TOOLS:
`$ cat userinfo.txt`

`$ cupp -i` (Common User Passwords Profiler)

`$ python3 check_password.py`

## WALKTHROUGH:
1. `$ cat userinfo.txt`
    - Personal details: name Alice Johnson, nickname AJ, birthdate 15-07-1990, partner Bob, child Charlie

2. `$ cupp -i`
    - Interactive mode: feed in the names/dates above; cupp generates password permutations (name+year, nickname+dob, etc.)

3. `$ mv alice.txt passwords.txt`
    - Rename output to what `check_password.py` expects

4. `$ python3 check_password.py`
    - `Password found: picoCTF{Aj_15901990}`
        - Answer: `picoCTF{Aj_15901990}`

## NOTES:
- `cupp` turns OSINT (name, pet, birthday) into a small, high-hit-rate wordlist — the mechanism behind most real password guessing.
- The flag `Aj_15901990` = nickname `AJ` lowercased + a date mangling, exactly the kind of pattern cupp emits.
