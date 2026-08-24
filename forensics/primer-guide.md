# Forensics:
Digital Forensics: focuses on gathering evidence in computers

*note: many of these new tools can be installed through homebrew(on mac)
## Searching for Strings and Filenames
`cat file` returns the content of a file

`grep ‘keyword’ file` returns wherever keyword is found
	add —-color for some color in your terminal

`find . -name file` returns the directory of where the file with the specified name is located

### Regexing in Grep!!!


## Hidden Data in Images:
steghide suite
exiftool for metadata analysis for hints

xxd for analysing headers, for determining the right file type, obtaining the correct information to process later

Use 
```
xxd -g 1 file | head
```
 to open the header file
hexedit can determine the correct file type if the header isn’t corrupted

use pngcheck to determine if any extra bytes are corrupted or not

Without very advanced knowledge of command line string manipulation, its best to just change the file bit by bit in hexedit

## Disk Analysis:
Sleuthkit: tools that are used to do analysis on disks

```
strings -t d dds1-alpine.flag.img | grep “CTF”  
``` 
is quite good

Tips:
Look in home then root first as they have the highest possibility of containing the stuff you want to see


`mmls disk.img` Lists the **partitions** of a disk
	Most commonly the partition with the largest size is mostly likely the one that contains the information

```
echo “text” | openssl [encryption format] -d 
```
**deciphers** text in its format outlined in encryption format

```
openssl [encryption format] -d -in filename 
```
deciphers text in its format in the file you specified

```
fls -o [offset] disk.img
```
 **ls** but for disks, offset given by mmls

```
fls -o [offset] disk.img [inode number]
```
 **ls** the directory in the inode number, for going deeper into the file structure

```
icat -o [offset] disk.img [inode number] 
```
**returns** the stuff in the file specified by the inode number

```
icat -o [offset] disk.img [inode number] > filename
```
 spits out the **information** in the file you specifed in filename into your working directory, for further decryption or analysis

`gunzip file.gz` **unzips** a gunzip file, or you could also decrypt by double clicking in GUI


## Packet Analysis
hoho this is hot

Use Wireshark for analysis

### Searching:
use regex to search
very versatile to search
be familiar with the different types of encodings so you know what format to search for
“picoCTF” is 7 chars, followed by a “{"
`[a-zA-Z]{7}\{`
keep in mind that the flag is most likely encoded in some sort 

`tcp.flags.push==1 and !arp`
tcp.flags.push==1 isolates the TCP
!arp gets ride of the extra hardwire->ip info
`and` allows for multiple filters