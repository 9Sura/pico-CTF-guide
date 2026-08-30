# FORENSICS GIT 2 PROBLEM GUIDE:

*(Forensics — Hard, 400pt)*

## HINTS:
Hint 1: The deletion was interrupted before any git objects were touched — the objects survive.

## TOOLS:
`$ tsk_recover -e -o <offset> <image> <outdir>`

`$ git cat-file -t/-p`, `git ls-tree`, `git show`

## WALKTHROUGH:
1. Recover the partition (as [[forensics-git-0]]), then `cd` into `.../Code/killer-chat-app/`.

2. `$ git status` -> `fatal: not a git repository`. The `.git/` exists but `refs/` was partly deleted. Rebuild the missing dirs:
    - `$ mkdir -p .git/refs/heads .git/refs/tags`
    - `$ git status` now works

3. Enumerate loose commit objects and print them:
```bash
find .git/objects/ -type f | grep -v pack | awk -F/ '{print $(NF-1)$NF}' \
 | xargs -I {} sh -c 'if [ "$(git cat-file -t {})" = "commit" ]; then echo "--- {} ---"; git cat-file -p {}; fi'
```
    - A commit `e80b38b3...` is titled **Add secret hideout chat log**

4. `$ git ls-tree -r e80b38b3...`
    - Lists `logs/3.txt` (not on disk anymore)

5. `$ git show e80b38b3...`
    - The chat log contains the password:
        - Answer: `picoCTF{...}` (instance-specific — read your own `git show`)

## NOTES:
- Git's object store (`.git/objects`) is content-addressed and independent of refs. As long as the blobs/commits exist, deleting refs doesn't destroy history — recreate `refs/` and the repo is readable again.
- `git cat-file -t` gives an object's type, `-p` pretty-prints it; that's how you walk objects with no branches.
