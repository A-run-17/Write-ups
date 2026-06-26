# MemLabs Lab 5 - Black Tuesday

## Challenge Description

We received this memory dump from our client recently. Someone accessed his system when he was not there and he found some rather strange files being accessed. Find those files and they might be useful. I quote his exact statement,

> The names were not readable. They were composed of alphabets and numbers but I wasn't able to make out what exactly it was.

Also, he noticed his most loved application that he always used crashed every time he ran it. Was it a virus?

***Note-1***: This challenge is composed of 3 flags. If you think 2nd flag is the end, it isn't!! :P

***Note-2***: There was a small mistake when making this challenge. If you find any string which has the string "L4B_3_D0n3!!" in it, please change it to "L4B_5_D0n3!!" and then proceed.

***Note-3***: You'll get the stage 2 flag only when you have the stage 1 flag.

---

## Flag 

First let's see what are the process that was running using the pslist plugin.
```
vol -f MemoryDump_Lab5.raw windows.pslist
```

The Suspicious activities are :

- `WINRAR.exe`
- `NOTEPAD.EXE`

Now Let's take a look at the commands executed using cmdline plugin .
```
 vol -f MemoryDump_Lab5.raw windows.cmdline
```

We can see a suspicious file name is found : `SW1wb3J0YW50.rar`

Now let's dump that into our system and see what's inside . There is a .png file which requires a password for opening.

In the archive we found a file named `ZmxhZ3shIV93M0xMX2QwbjNfU3Q0ZzMtMV8wZl9MNEJfNV9EMG4zXyEhfQ.bmp`. This look like a base64 encoded string.

```
$ echo "ZmxhZ3shIV93M0xMX2QwbjNfU3Q0ZzMtMV8wZl9MNEJfNV9EMG4zXyEhfQ" | base64 -d
flag{!!_w3LL_d0n3_St4g3-1_0f_L4B_5_D0n3_!!}
```

This gives us the first flag. Using this as the password for the `.rar` file to open that PNG file , we got the second flag.
```
Flag 2 : flag{W1th_th1s_$taGe_2_1s_cOmPL3T3_!!}
```

There is one flag remaining. we found `NOTEPAD.EXE` an suspicious activity. Dump that file into our system and analyze it.

I will load the file into IDA Pro for inspection. Once loaded, we noticed some strings. After putting them together, we will able to retrieve the flag

```
Flag 3 : bi0s{M3m_l4b5_OVeR_!}
```
