# MemLabs Lab 3 - The Evil's Den

## Challenge Description
A malicious script encrypted a very secret piece of information I had on my system. Can you recover the information for me please?

- Note-1: This challenge is composed of only 1 flag. The flag split into 2 parts.

- Note-2: You'll need the first half of the flag to get the second.

You will need this additional tool to solve the challenge,
```bash
$ sudo apt install steghide
```
The flag format for this lab is: inctf{s0me_l33t_Str1ng}

## Flag

First of all when we try to run a basic plugin like windows.info it shows
```bash
~$ vol -f MemoryDump_Lab3.raw windows.info
Volatility 3 Framework 2.28.0
Progress:  100.00               PDB scanning finished
Unsatisfied requirement plugins.Info.kernel.layer_name:
Unsatisfied requirement plugins.Info.kernel.symbol_table_name:

A translation layer requirement was not fulfilled.  Please verify that:
        A file was provided to create this layer (by -f, --single-location or by config)
        The file exists and is readable
        The file is a valid memory image and was acquired cleanly

A symbol table requirement was not fulfilled.  Please verify that:
        The associated translation layer requirement was fulfilled
        You have the correct symbol file for the requirement
        The symbol file is under the correct directory or zip file
        The symbol file is named appropriately or contains the correct banner

Unable to validate the plugin requirements: ['plugins.Info.kernel.layer_name', 'plugins.Info.kernel.symbol_table_name']
```

Volatility 3 couldn't automatically identify the OS kernel from the memory dump.

### Why it happens 

MemLabs dumps were created a while ago, often from specific Windows builds. Vol3 needs an ISF (Intermediate Symbol File) that exactly matches that build. If it's not in your symbols cache, Vol3 can't proceed.

#### So we are going to use volatility 2 For this lab.

First we need to identify the operating system of the memory image.

```bash
$ python2 vol.py -f MemoryDump_Lab3.raw imageinfo

```

<img width="959" height="157" alt="image" src="https://github.com/user-attachments/assets/958548bd-00ba-4647-bf1b-aef06dd5ac6b" />

the profile is found to be ***Win7SP1x86_23418***

Next, let’s check the command line of the running processes.

```bash
$ python2 vol.py -f MemoryDump_Lab3.raw --profile Win7SP1x86_23418 cmdline
Volatility Foundation Volatility Framework 2.6.1
........
notepad.exe pid:   3736
Command line : "C:\Windows\system32\NOTEPAD.EXE" C:\Users\hello\Desktop\evilscript.py
************************************************************************
notepad.exe pid:   3432
Command line : "C:\Windows\system32\NOTEPAD.EXE" C:\Users\hello\Desktop\vip.txt
```

We got two files. evilscript.py which as the name implies is evil and vip.txt which look like an important file.


Let’s search for these two files in memory.

```bash
$ python2 vol.py -f MemoryDump_Lab3.raw --profile Win7SP1x86_23418 filescan | egrep "evilscript.py|vip.txt"
Volatility Foundation Volatility Framework 2.6.1
0x000000003de1b5f0      8      0 R--rw- \Device\HarddiskVolume2\Users\hello\Desktop\evilscript.py.py
.........
0x000000003e727e50      8      0 -W-rw- \Device\HarddiskVolume2\Users\hello\Desktop\vip.txt
```


Now that we have the offsets of the two files, let’s dump them.

```bash
$ python2 vol.py -f MemoryDump_Lab3.raw --profile Win7SP1x86_23418 dumpfiles -Q 0x000000003de1b5f0 -D lab3_output/
```

Here is the dumped python file:

```python
import sys
import string

def xor(s):
	a = ''.join(chr(ord(i)^3) for i in s)
	return a

def encoder(x):
	return x.encode("base64")

if __name__ == "__main__":
	f = open("C:\\Users\\hello\\Desktop\\vip.txt", "w")
	arr = sys.argv[1]
	arr = encoder(xor(arr))
	f.write(arr)
	f.close()
```

This evil script is XORing the file `vip.txt` with a single character then Base64 encoding it.

And here is the content of the dumped text file:

```text
am1gd2V4M20wXGs3b2U=
```

So we first need to Base64 decode it then XOR it again with same character to retrieve the original text.

```python
$ python
>>> s = 'am1gd2V4M20wXGs3b2U='
>>> s = s.decode('base64')
>>> ''.join(chr(ord(i)^3) for i in s)
inctf{0n3_h4lf
```

`First half: inctf{0n3_h4lf`

This one took me sometime, then I looked at the hint and it says something about steghide.

Steghide is a steganography program that is able to hide data in images and audio files and it supports JPEG and BMP images, so I decided to search memory for JPEG images.

```bash
$ python2 vol.py -f MemoryDump_Lab3.raw --profile Win7SP1x86_23418 filescan | grep ".jpeg"
Volatility Foundation Volatility Framework 2.6.1
0x0000000004f34148      2      0 RW---- \Device\HarddiskVolume2\Users\hello\Desktop\suspision1.jpeg
```

Here is the dumped Jpeg image

<img width="256" height="197" alt="2" src="https://github.com/user-attachments/assets/9ef9b125-d572-4e27-8a20-39cfc76089f6" />

```bash
$ steghide extract -sf lab3_output/suspision1.jpeg 
Enter passphrase:
```

It’s asking for a passphrase, the hint clearly says that: You'll need the first half of the flag to get the second.

Let’s try the first half of the flag as the passphrase.

```bash
$ steghide extract -sf lab3_output/suspision1.jpeg 
Enter passphrase:
wrote extracted data to "secret text".
```

Now let's see the content of the file 


```bash
$ cat 'secret.text' 
_1s_n0t_3n0ugh}
```

`Flag: inctf{0n3_h4lf_1s_n0t_3n0ugh}`
