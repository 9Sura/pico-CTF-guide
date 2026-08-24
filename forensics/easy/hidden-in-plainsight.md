# HIDDEN IN PLAINSIGHT PROBLEM GUIDE:


## HINTS:
Hint 1: Download the jpg image and read its metadata.

## TOOLS: 
`$ exiftool <filename>`

`$ echo '<string>' | base64 -d`

`$ steghide extract -sf <filename> -p <password>`

## WALKTHROUGH:
1. `$ exiftool img.jpg`
    - `Comment                         : c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9`
        - Comment seems oddly suspicious... like base64 suspicious

2. `$ echo 'c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9' | base64 -d `
    - Output: `steghide:cEF6endvcmQ=`
        - `cEF6endvcmQ=` is still in base64 

3. `$ echo 'cEF6endvcmQ=' | base64 -d`
    - Output: `pAzzword`

4. `$ steghide extract -sf img.jpg -p pAzzword`
    - Uses steghide to extract flag data from img.jpg 
        - Output: `wrote extracted data to "flag.txt".`

5. `$ open -e flag.txt`
    - Answer: `picoCTF{h1dd3n_1n_1m4g3_656e4d79}`