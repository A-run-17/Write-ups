# MemLabs Lab 6 - The Reckoning

## Challenge Description
We received this memory dump from the Intelligence Bureau Department. They say this evidence might hold some secrets of the underworld gangster David Benjamin. This memory dump was taken from one of his workers whom the FBI busted earlier this week. Your job is to go through the memory dump and see if you can figure something out. FBI also says that David communicated with his workers via the internet so that might be a good place to start.

**Note:*** This challenge is composed of 1 flag split into 2 parts.

The flag format for this lab is: inctf{s0me_l33t_Str1ng}

## Flag

For this we are going to use volatility 2 instead of volatility 3 since it has versatile plugin options than vol3.

First of all, we need to find the profile of the memory image using imageinfo plugin as in the following command.
```
python2 vol -f MemoryDump_Lab6.raw imageinfo
```
We can use `Win7SP1x64` profileas suggested.

Firstly, we can check the running processes to find a good starting point using the `pslist` plugin.
```
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 pslist
```

As appears in the above screenshot, there are four important processes:

- `cmd.exe` process that indicates that some commands get executed.
- `WinRAR.exe` process that may be used to extract a compressed file.
- `chrome.exe` processes that are instances of chrome browser.
- `firefox.exe` processes that are instances of firefox browser.


### Inspecting WinRAR process

At this step, we will look at command line arguments for the WinRAR process using the `cmdline` plugin. The command line argument of the WinRAR process contains a flag.rar file. So, let’s dump that file .

```
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 filescan | grep "flag.rar"
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000005fcfc4b0 -D .
```

When we tried to extract the `flag.rar`, I found that it’s protected by a password.

### Inspecting the executed commands

As previously mentioned, one of the running processes is cmd.exe. I thought to find the executed commands using the `consoles` plugin.


A unrecognized command env gets executed, which seems like a hint to check the environment variables. Thus, let’s examine the environment variables using the `envars` plugin to find if something is interesting.

```
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 envars
```

There we found that the password for the flag.rar file is  `easypeasyvirus` . Then the PNG file is analyzed.

<img width="500" height="500" alt="flag2" src="https://github.com/user-attachments/assets/7af26a33-787f-4fed-a5ab-947017472454" />

### Examining the browser activities

Now, let’s investigate the browsers’ activities and start with the user browsing history of the Chrome browser by dumping a file with the following command.

```
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 filescan | grep -i "chrome" | grep -E "*History$
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 dumpfiles -Q 0x000000005da5a610 -D .
```

Then we used DB Browser for SQLite to open the dumped History file of the Chrome browser. There we found a Pastebin URL.

The Pastebin URL contains a link to a google document and text about a key that was sent through the mail.

After that, we visited the Google document link. While going through it, we found a Mega.nz link whose content is encrypted.

As nothing more interesting within Chrome browser history, Now we are going to analyze the Firefox browser history, which is located in a file named places.sqlite. So, let’s find and dump that file using the following commands.

```
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 filescan | grep -E "*places.sqlite$"
python2 vol.py -f MemoryDump_Lab6.raw --profile=Win7SP1x64 dumpfiles -Q 0x00000000053fd070 -D .
```

While examining the Firefox history, we found two Google Mail URLs with similar titles. However, one of them contains the word “Mega Drive Key” and the other one contains random text `zyWxCjCYYSEMAhZe552qWVXiPwa5TecODbjnsscMIU` which is the key for that Mega.mz file we found in the last stage.

So we find a PNG image in the Mega drive. However, PNG is corrupted. Fixing the IHDR of the image gives us the 1st part of the flag.

<img width="490" height="462" alt="1_TAO5cA7VQIFIt9JxtHV4yw" src="https://github.com/user-attachments/assets/e6ef13ba-1117-474f-b2d4-ad69a9b06a1b" />


--- 

The Final Flag :
```
inctf{thi5_cH4LL3Ng3_!s_g0nn4_b3_?aN_Am4zINg!igU3Ss???_}
```


