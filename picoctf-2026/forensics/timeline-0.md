# TIMELINE 0 PROBLEM GUIDE:

*(Forensics — Easy, 100pt)*

## HINTS:
Hint 1: Create a Sleuthkit MAC timeline.
Hint 2: Sloppy timestomping leaves strange (very old) timestamps.

## TOOLS:
`$ file <image>`

`$ fls -r -m / <image> > timeline.body`

`$ mactime -b timeline.body`

`$ icat <image> <inode>`

## WALKTHROUGH:
1. `$ file partition4.img`
    - `Linux rev 1.0 ext4 filesystem data` — a raw filesystem, no partition table, so no `-o` offset needed

2. `$ fls -r -m / partition4.img > timeline.body`
    - `-m /` writes body-file format; `-r` recurses

3. `$ mactime -b timeline.body`
    - Sorted MAC timeline. One entry has an absurd date:
        - `Wed Jan 02 1985 ... 4945 /bin/bcab` — the timestomped file

4. `$ icat partition4.img 4945`
    - `NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK` (base64)

5. `$ echo 'NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK' | base64 -d`, wrap in flag format:
    - Answer: `picoCTF{71m311n3_0u7113r_h3r_43a2e7af}`

## NOTES:
- Timestomping = faking file timestamps to hide activity. Sorting the timeline makes the outlier obvious.
- The description says "wrap what you find" — the recovered content is the inside of the braces; add `picoCTF{...}` yourself. See [[timeline-1]] for the harder version.
