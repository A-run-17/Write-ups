# MemLabs Lab 1 - Beginner's Luck


## Challenge description
My sister's computer crashed. We were very fortunate to recover this memory dump. Your job is get all her important files from the system. From what we remember, we suddenly saw a black window pop up with some thing being executed. When the crash happened, she was trying to draw something. Thats all we remember from the time of crash.

## Initial Thoughts :

- Black window - indicates cmd
- Draw Something 


Now  the zip file downloaded and extracted it to get the dump file. Then the file is hash checked with the hash given in the challenge repositary.

Now let's run the process list plugin on volatility 3 
```
vol -f MemoryDump_Lab1.raw windows.pslist
```
<img width="586" height="500" alt="image" src="https://github.com/user-attachments/assets/8bdb28f9-409d-4e55-a248-5830bfd2dd91" />

Here,

- Cmd.exe - 1984(pid)
- mspaint - 2422(pid)

Are the potential suspects.

## Flag - 1

Let’s start with cmd.exe (PID 1984). Unlike volatility2, wherein you can type command console and it will output all command history that was done here. But we’re using volatility3 so we are doing a different approach.

We will run the handles plugin in volatility 3
```bash
vol -f MemoryDump_Lab1.raw windows.handles --pid 1984
```

<img width="825" height="290" alt="image" src="https://github.com/user-attachments/assets/5ffac4d9-a178-426c-826c-06f423845fcc" />

When we see a Type — File, means this is a physical file that is linked with the process. we can just try dumping it using command
```bash
vol -f MemoryDump_Lab1.raw -o .  windows.memmap.Memmap --pid 1984 --dump
```

A file named pid.1984.dmp will appeare in our current working directory. Let's just extract the readable text and and upload it to a new file and analyze it.

```bash
strings pid.1984.dmp > cmd.txt
```

<img width="502" height="345" alt="image" src="https://github.com/user-attachments/assets/b0cb659c-6285-4953-bbaf-692a1b7e236a" />

Here we can see an interesting .dat file st4G$1.bat

Now let's extract that file to owr system for further investiation.

```bash
vol -f MemoryDump_Lab1.raw windows.filescan.FileScan | grep St4G3
vol -f MemoryDump_Lab1.raw -o . windows.dumpfiles.DumpFiles --physaddr 0x3edcfc20
```

It will be automatically saved with a .dat file extension. Remove it then open in a text editor and see it’s contents.

<img width="500" height="343" alt="image" src="https://github.com/user-attachments/assets/a224bab2-544b-4cb1-9335-9845f0d07867" />

We see that there is a base64 encoded text. On decoding the text we will get the first flag

```bash
~$ echo "ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=" | base64 -d
flag{th1s_1s_th3_1st_st4g3!!}
```

## Flag - 2

Let’s move on with mspaint.exe (PID 2424). After doing some research, you can directly extract it’s memory and then change the file extension to .data which can the be opened in Gimp.

```
vol -f MemoryDump_Lab1.raw -o "dump" windows.memmap.Memmap --pid 2424 --dump
```

Rename the .dmp file to .data for further analysis. Open that file in Gimp and adjust the width and offset until the image appears.

<img width="550" height="319" alt="image" src="https://github.com/user-attachments/assets/b662ea38-0fa5-4696-8ef3-d1fd8a4a8d96" />

```text
Flag : flag{G00d_Boy_good_girL}
```

## Flag - 3

In the given challenge it is said that ***Our job is to find all the important document***. So we will find that in the file system of the dump

```
vol -f MemoryDump_Lab1.raw windows.filescan | grep Important
```
These are all the files that have been found.

<img width="446" height="40" alt="image" src="https://github.com/user-attachments/assets/6fff8290-2f15-4b97-95f8-6fc1c131cb2f" />

Now let's dump that files to owr system 

```
vol -f MemoryDump_Lab1.raw windows.dumpfiles --physaddr 0x3fa3ebc0
```

Important.rar is password protected but once we open it, we can see that there’s a comment beside saying that the Alissa’s NTLM hash (in uppercase) is the password.

<img width="793" height="381" alt="image" src="https://github.com/user-attachments/assets/c7b7cc0f-2d4a-42b5-a8ab-3130349682de" />

To dump NTLM hashes from a Windows memory image using Volatility 3, we can use:

```
vol -f MemoryDump_Lab1.raw windows.hashdump
```

or in some newer versions:

```
vol -f MemoryDump_Lab1.raw windows.registry.hashdump
```

We using on fo the above commands

```bash
~$ vol -f MemoryDump_Lab1.raw windows.registry.hashdump
Volatility 3 Framework 2.28.0
Progress:  100.00               PDB scanning finished
User    rid     lmhash  nthash

Administrator   500     aad3b435b51404eeaad3b435b51404ee        31d6cfe0d16ae931b73c59d7e0c089c0
Guest   501     aad3b435b51404eeaad3b435b51404ee        31d6cfe0d16ae931b73c59d7e0c089c0
SmartNet        1001    aad3b435b51404eeaad3b435b51404ee        4943abb39473a6f32c11301f4987e7e0
HomeGroupUser$  1002    aad3b435b51404eeaad3b435b51404ee        f0fc3d257814e08fea06e63c5762ebd5
Alissa Simpson  1003    aad3b435b51404eeaad3b435b51404ee        f4ff64c8baac57d22f22edc681055ba6

```

So the Password is found to be ***F4FF64C8BAAC57D22F22EDC681055BA6***


<img width="247" height="247" alt="image" src="https://github.com/user-attachments/assets/db7f960d-d0d2-4a08-87b7-beee30d91f7d" />

```
Flag : flag{w3ll_3rd_stage_was_easy}
```

So the all the three flags are found:

- flag{th1s_1s_th3_1st_st4g3!!}
- flag{G00d_Boy_good_girl_}
- flag{w3ll_3rd_stage_was_easy}

---

If you done all the above steps correctly and got the flag , submit it to the email given the Lab. After submiting the flags if it is correct you will recive an confirmation email

<img width="702" height="212" alt="image" src="https://github.com/user-attachments/assets/3b445734-c771-4e85-a5b0-e2c1d9d2b745" />








