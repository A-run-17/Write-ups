# Critical - TryHackMe

Acquire the basic skills to analyze a memory dump in a practical scenario.

---

## Task 1 - Introduction

Learning Objectives

In this room, we'll cover the following learning objectives.

- Memory Forensics’ basic concepts.
- How to access and set up the environment.
- Gathering information from the compromised target.
- Search for suspicious activity using the information obtained.
- Extracting and analysing data from memory.
- Conclusion & further steps after completing the room.

## Task 2 - Memory Forensics

In cyber security, memory forensics is a subset of computer forensics that analyzes volatile memory, usually on a compromised machine; in Windows OS, it corresponds to the Random Access Memory (RAM), and its content is flushed with every reboot or shutdown, making it one of the usual initial task to perform during an incident. The process differs from disk forensics analysis since it not only provides information about what resides on the target computer but also provides us with information about the processes or applications that were running at a particular time and detailed information on the execution flow on a system that may not be present in regular storage units or application logs. 

We can divide the tasks in a Memory forensic task into two main phases: Memory Acquisition and Memory Analysis.

- During the memory acquisition phase, we'll copy the live memory to a file, commonly referred to as a dump, to perform the analysis without risking losing the data from an inadvertent reboot on the compromised system and have proof of the analysis inc as needed.

- Next, during the memory analysis phase, we'll analyze the obtained memory dump of the forensic data.

Question :
```
What type of memory is analyzed during a forensic memory task?
```
Answer :
```
RAM (Random Access Memory)
```

Question :
```
In which phase will you create a memory dump of the target system?
```
Answer:
```
Memory Acquisition
```
---
## Task 3 - Environment & Setup

## Imaging Tools
 
There are several ways to acquire the memory from the lab machine if needed; several tools can help us, but which one to use will depend on personal preference and the OS involved in the imaging task. Some of these tools are:
| Field | Value |
|-------|-------|
|Windows|	FTK imager , WinPmem|
|Linux	|LIME|
|macOS	|osxpmem|

Question:
```
Which plugin can help us to get information about the OS running on the lab machine?
```
Answer:
```
Windows.info
```
Question:
```
Which tool referenced above can help us take a memory dump on a Linux OS?
```
Answer:
```
LIME
```
Question:
```
Which command will display the help menu using Volatility on the lab machine?
```
Answer:
```
vol -h
```

---

## Task 4 - Gathering Target Information

## Obtaining Information

Getting information about the target is crucial to our investigation since it ensures we're analyzing the correct context and environment of the evidence. This step helps us understand specific architecture and operating systems, ensuring our findings' accuracy, relevance, and legitimacy.

We can get information about the target using the -f switch to indicate the file to analyze, in this case, memdump.mem followed by the plugin windows.info  used to get the general information.

```bash
analyst@ip-10-48-183-173:~$ vol -f memdump.mem windows.info
Volatility 3 Framework 2.5.2
Progress:  100.00               PDB scanning finished                        
Variable        Value

Kernel Base     0xf8066161b000
DTB     0x1ad000
Symbols file:///home/analyst/volatility3-2.5.2/volatility3/symbols/windows/ntkrnlmp.pdb/4DBE144182FF4156845CD3BD8B654E56-1.json.xz
Is64Bit True
IsPAE   False
layer_name      0 WindowsIntel32e
memory_layer    1 FileLayer
KdVersionBlock  0xf8066222a400
Major/Minor     15.19041
MachineType     34404
KeNumberProcessors      2
SystemTime      2024-02-24 22:52:52
NtSystemRoot    C:\Windows
NtProductType   NtProductWinNt
NtMajorVersion  10
NtMinorVersion  0
PE MajorOperatingSystemVersion  10
PE MinorOperatingSystemVersion  0
PE Machine      34404
PE TimeDateStamp        Sat Jan 13 03:45:32 2085
```
These informarions where gathered from the Target machine inside TryHackMe

Question:
```
Is the architecture of the machine x64 (64bit) Y/N?
```
Answer:
```
Y
```
Question:
```
What is the Verison of the Windows OS
```
Answer:
```
10
```
Question:
```
What is the base address of the kernel?
```
Answer:
```
0xf8066161b000
```

---

## Task 5 - Searching for Suspicious Activity

Now that we have the information from the target we are working on let's try to identify any suspicious activity in the memory dump. 
 
## Suspicious Activity
 
Suspicious activity refers to technical anomalies that may be present in a system, such as unexpected processes, unusual network connections, or registry modifications. These activities often signal potential security threats like malware attacks or data breaches.
 
## Starting the Search
 
We can start by observing any potential network activity. We can use the plugin windows.netstat to see if there's an interesting or unusual connection. At this stage, remote access connections or access to suspicious sites are something to look for.

Question:
```
Using the plugin "windows.netscan". Can you identify the destination IP address where a connection is established on port 80?
```
Answer:
```
192.168.182.128
```
Question:
```
Using the plugin "windows.netscan," can you identify the program (owner) used to access through port 80?
```
Answer:
```
msedge.exe
```
Question:
```
Analyzing the processes present on the dump, what is the PID of the child process of critical_updat?
```
Answer:
```
1612
```
Question:
```
What is the time stamp time for the process with the truncated name critical_updat?
```
Answer:
```
2024-02-24 22:51:50.000000
```

---

## Task 6 - Finding Interesting Data

With the information we have collected, we can investigate the process critical_updat  that we identified in our previous task, which has a child process called updater. Let's investigate the child process more in-depth. Let's start by looking at where on the disk it was saved; for that, we can use the plugin windows.filescan which will allow us to examine the files accessed that are stored in the memory dump. This output is quite big, so to access the data in a better way, we will use the > character in bash to redirect the output to a file, in this case, filescan_out.

```
vol -f memdump.mem windows.filescan > filescan_out
```


If we want to have more detailed information like when the file was accessed or modified, we can use the plugin windows.mftscan.MFTScan, whose output is also quite big, so we will redirect the output to the file mftscan_out as shown below.

 ```
vol -f memdump.mem windows.mftscan.MFTScan > mftscan_out
```

## Getting the Goods
 
Let's get information on the process. There are several ways to examine memory, but we will continue using volatility. This time, we will dump the memory region corresponding to updater.exe , and examine it.
 
 To accomplish the above, we'll use the plugin windows.memmap. This time, we'll specify the output dir with the -o switch. In this case, we will use the same directory denoted by the character " . "and the option --dump followed by the option --pid and the PID of the process, which in the case of updater.exe is.

 ```
vol -f memdump.mem -o . windows.memmap --dump --pid 1612
```

Question:
```
Analyzing the "windows.filescan" output, what is the full path and name for critical_updat?
```
Answer:
```
C:\Users\user01\Documents\critical_update.exe
```
Question:
```
Analyzing the "windows.mftscan.MFTScan" what is the Timestamp for the created date of important_document.pdf?
```
Answer:
```
2024-02-24 20:39:42.000000
```
Question:
```
Analyzing the updater.exe memory output,
can you observe the HTTP request and determine the server used by the attacker?
```
Answer:
```
SimpleHTTP/0.6 Python/3.10.4
```

