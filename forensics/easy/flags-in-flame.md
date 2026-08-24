# FLAGS IN FLAME PROBLEM GUIDE: 

## HINTS:
Hint 1: Use base64 to decode the data and generate the image file.

## TOOLS: 
`$ cat <filename> | base64 -d > <filename>`

## WALKTHROUGH: 
1. `$ cat logs.txt | base64 -d > output.bin`
    - Hint 1: Use base64 to decode the data and generate the image file.
    - `output.bin` is an placeholder file used to indicate img type

2. `$ file output.bin`
    - Should indicate `output.bin` is png type 

3. `$ cat logs.txt  | base64 -d > output.png`
4. `$ open output.png`
    - A set of numbers should be visible on the bottom

5. `$ echo "7069636F4354467B666F72656E736963735F616E616C797369735F69735F616D617A696E675F61633165333538347D" | xxd -r -p`
    - At this point you could also use GPT to decrypt 
        - Answer: picoCTF{forensics_analysis_is_amazing_ac1e3584