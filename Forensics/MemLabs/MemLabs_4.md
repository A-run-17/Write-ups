# MemLabs Lab 4 - Obsession

## Challenge Description

My system was recently compromised. The Hacker stole a lot of information but he also deleted a very important file of mine. I have no idea on how to recover it. The only evidence we have, at this point of time is this memory dump. Please help me.

`Note:` This challenge is composed of only 1 flag.

The flag format for this lab is: inctf{s0me_l33t_Str1ng}

## Flag - 1

The first thing we want to do is figure out what type of dump we have received. To do that, we run the imageinfo plugin against the memory dump. The highlighted text shows the possible image profiles.

```
 vol.py -f MemoryDump_Lab4.raw imageinfo
```

Next, we want to know which processes were running when the memory dump was taken. To check that, we run the following command:

```
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 pslist
```

Lets look at what commands were run using this StickeyNote. We can use this command:

```
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 -p 2432 cmdline
```

Now lets look at filescan and see if there is anything intresting is related to this process. For that we can use this command:

```
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 filescan | grep "StikyNot"

```

That this seems to be some kind of malware. Since the question doesn’t require us to perform any malware analysis, we will keep this in the back burner for now and come back to it after we do some more digging.


Using the hashdump plugin , One more intersting fact we noticed here is the users are eminem and SlimShady, now we will modify the script and grep for eminem and SlimShady and see all the files assosiated with them. This is the command we can use:

```
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 filescan | grep "eminem" 
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 filescan | grep "SlimShady"
```

we found :
- `Screenshort1.png`
- `glaf.jpeg`
- `important.txt`

Now lets use dumpfiles plugin to dump all these information into our system. We can use this command to dump these files.

After taking a look at both the images we came to a conclusion that it is not useful for finding the flags.

Lets dump important.txt next.

Interestingly, I am not able to use dumpfiles plugin to dump important.txt. Then it made me realize that in the challenge description, it is mentioned that the file was deleted.

We can use mftparser plugin. Basically we can use this plugin to extract information about files that may have been deleted, track file activity, and reconstruct the directory structure during memory analysis of Windows systems. So now we will use this command:

```
vol.py -f MemoryDump_Lab4.raw --profile=Win7SP1x64 mftparser >> mfttable.txt
```

After going through the dump and searching for important.txt, we will get the flag.

```
Flag : inctf{1_is_n0t_EQu4l_7o_2_bUt_th1s_d0s3nt_m4ke_s3ns3}
```

