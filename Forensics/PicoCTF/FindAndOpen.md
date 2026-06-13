# FindAndOpen - CyLab Security Academy

Category   : Forensics

Difficulty : Medium

## Method used for Resolving

Download the zip file and the trace file from the task and analyze it. When you try to unzip the file it requires a password which is not known to us. So our task is to find
the password for the zip file using the tracefile given to find the flag insdie the zip file.

When you try to view the trace file to shows a bunch of non readable characters. So using strings command in linux terminal to list only the readable ascii characters .


```
strings dump.pcap
```
While running this command you can see many ascii printable characters. but we are going to narrow down to the one that can 
potentially show us the password for unlocking the zip file.

```
Flying on Ethernet secret: Is this the flag

iBwaWNvQ1RGe1Could the flag have been splitted?

AABBHHPJGTFRLKVGhpcyBpcyB0aGUgc2VjcmV0OiBwaWNvQ1RGe1IzNERJTkdfTE9LZF8=

PBwaWUvQ1RGesabababkjaASKBKSBACVVAVSDDSSSSDSKJBJS

PBwaWUvQ1RGe1Maybe try checking the other file
```
The leading characters AABBHHPJGTFRLKV are junk padding. After removing the junk and base64 decode the rest will give us :
```
This is the secret: picoCTF{R34DING_LOKd_
```

Trying this a the password for unziping the provided zip file using the command:
```bash
unzip -P "picoCTF{R34DING_LOKd_" flag.zip
```

This will help us to unzip the provided zip file and a file named flag will be extracted. Reading the flag file will provide us with the flag for completing the level
```
Flag : picoCTF{R34DING_LOKd_fil56_succ3ss_cbf2ebf6}
```
