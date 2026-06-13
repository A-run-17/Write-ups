# Packets Primer - CyLab Security Academy

Category   : Forensics

Difficulty : Medium

## Method used for resolving

Download the packet capture file form the task. open the file using **WireShark** a open source for analyzing packet capturing files and realtime packet capturing.

We can see that there are only 9 packets avaliable in the network-dump.flag.pcap file. While viewing them one by one we can find the flag.


<img width="732" height="512" alt="image" src="https://github.com/user-attachments/assets/8927f163-eb09-4f8f-a321-eebb6c7bbe4b" />

Or using basic strings command in linux terminal, We can find the flag.
```
strings network-dump.flag.pcap
```

The flag :
```
picoCTF{p4ck37_5h4rk_ceccaa7f}
```
