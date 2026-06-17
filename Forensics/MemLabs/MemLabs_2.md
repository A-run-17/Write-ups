# MemLabs Lab 2 - A New World

## Challenge description

One of the clients of our company, lost the access to his system due to an unknown error. He is supposedly a very popular "environmental" activist. As a part of the investigation, he told us that his go to applications are browsers, his password managers etc. We hope that you can dig into this memory dump and find his important stuff and give it back to us.

Note: This challenge is composed of 3 flags.

---

Initial Thoughts :

- "environmental" activist            - Have to analyze the environment varaiable
- His go to applications are browsers - Have to take a look at his browser

## Flag - 1

First let's take the look at the environmental variavle using the envras plugin in volatility 3.
```
vol -f MemoryDump_Lab2.raw windows.envars
```

<img width="827" height="487" alt="image" src="https://github.com/user-attachments/assets/c2ff289f-7c08-4908-a417-e23781508974" />

Here this string `ZmxhZ3t3M2xjMG0zX1QwXyRUNGczXyFfT2ZfTDRCXzJ9` appears more time in the path. We can identify this as a base64 encoded string. So we can decode this string to get the flag.

```bash
~$ echo "ZmxhZ3t3M2xjMG0zX1QwXyRUNGczXyFfT2ZfTDRCXzJ9" | base64 -d
flag{w3lc0m3_T0_$T4g3_!_Of_L4B_2}
```

## Flag - 2

While running the pslist plugin we can find the keepass.exe application has been listed out.

`KeePass.exe` is the main executable of KeePass Password Safe, a popular open-source password manager used to securely store usernames, passwords, notes, and other sensitive data in an encrypted database.

Since it stores file in `.kdbx` format. we are going to search it against the file avaliable in the dump.

```bash
~$ vol -f MemoryDump_Lab2.raw windows.filescan | grep ".kdbx"
0x3fb112a0 100.0\Users\SmartNet\Secrets\Hidden.kdbx
```

Here we found Hidden.kbdx. Dump it to owr system. Then to unlock this .kdbs x file we need a password. We also search it from the dump

```
vol -f MemoryDump_Lab2.raw windows.filescan | grep -i "password"
```

Then we found password.png. Dump it to owr system and analze it. On analyzing the image the password is found to be `P4SSw0rd_123`

Using keepassxc tool in linux we can analyze the .kdbx file .

```
keepassxc file.0x3fb112a0.0xfa8001593ba0.DataSectionObject.Hidden.kdbx
```
On using the password we found out from the image for login , we got into the file.

The the flag is found in the recyclebin under the username flag.

```
Flag : flag{w0w_th1s_1s_Th3_SeC0nD_ST4g3_!!}
```

## Flag - 3

Since browsers were specifically mentioned in the initial scenario, I checked chrome browser history files using Volatility’s filescan plugin

<img width="435" height="29" alt="image" src="https://github.com/user-attachments/assets/5fdfa355-b5f0-4b09-a255-34b1d908641d" />


Found an entry related to chrome browser history, I extracted the file and checked it.

```
vol -f MemoryDump_2.raw -o . windows.dumpfiles --physaddr 0x3fcfb1d0
```

I opened the extracted file using sql lite browser. Once the database file was opened, I navigated to the Browse Data tab and selected the urls table to view the recorded browsing history.

Among the various URLs, I found an entry pointing to a MEGA link:


<img width="1551" height="982" alt="image" src="https://github.com/user-attachments/assets/fe7395fa-efb1-4df0-9387-65cb09f90724" />

while trying to unzip the fileImportant.zip.This message indicated that the zip file was password-protected, and the password was the SHA1 hash of the stage-3 flag from Lab-1.

```
echo "flag{w3ll_3rd_stage_was_easy}" | sha1sum
6045dd90029719a039fd2d2ebcca718439dd100a
```

Using the above password for extracting the zip, we will obtain the flag.

<img width="398" height="394" alt="image" src="https://github.com/user-attachments/assets/16dc494c-8091-4b51-bbc8-ca23e0d3a8ef" />


```
Flag : flag{oK_So_Now_St4g3_3_is_DoNE!!}
```


