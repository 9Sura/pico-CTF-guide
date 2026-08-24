# CANYOUSEE PROBLEM GUIDE: 

## HINTS: 
Hint 1: How can you view the information about the picture?

Hint 2: If something isn't in the expected form, maybe it deserves attention?

## TOOLS: 
`$ unzip <filename>` 

`$ exiftool <filename>`

`$ echo "<text>" | base64 -d`

## WALKTHROUGH: 
1. `$ wget https://artifacts.picoctf.net/c_titan/128/unknown.zip`
2. `$ unzip unknown.zip`
3. `$ exiftool ukn_reality.jpg`
4. `$ echo "cGljb0NURntNRTc0RDQ3QV9ISUREM05fM2I5MjA5YTJ9Cg==" | base64 -d`
    - Answer: `picoCTF{ME74D47A_HIDD3N_3b9209a2}`