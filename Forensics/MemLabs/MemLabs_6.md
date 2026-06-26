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

