# Forensic Imaging - TryHackMe


Learn the basic concepts of forensic imaging.


---

## Task 1 - Introduction

In Digital Forensics, imaging is the process of creating an exact, bit-by-bit copy of digital storage media. This process ensures that all the data, including deleted files, hidden files, and unallocated space, is captured. By generating a forensic image, we preserve the original data, allowing for a thorough examination.

The primary goal of this process is to create a verifiable and reliable copy that can be used in investigations and legal proceedings. This process is crucial for maintaining the integrity of digital evidence, ensuring that the original data remains untouched and admissible in legal matters. 

---

## Task 2 - Preparation

The process of imaging a disk starts by identifying the target drive, preparing it for imaging, and then creating the image file which is later verified for integrity

## Write-Blockers

It is important to mention that write-blockers(opens in new tab) (opens in new tab)are usually required when manipulating physical disks. A write-blocker is a device used to prevent any modifications to data on a storage device during analysis. It ensures reading data without risking changes to the original evidence.

## Audit Trail 

An audit trail is a chronological record that tracks actions and events within a system, providing a detailed history for accountability and security. It ensures traceability by documenting each step. This step can be performed with different parameters depending on the legal or compliance framework required(opens in new tab) by the task.

If we are using bash, the history command can help us log our activity. It can be saved using a timestamp and preserved in a file. Below, we can observe a chart with some commands recommended to use during our session.
```bash
set -o history                   -  Enables command history in the shell

shopt -s histappend              -  Ensures that the command history is appended to the history file

export HISTCONTROL=              -  Clears any settings that control which commands are saved in the history

export HISTIGNORE=               -  Clears any settings that ignore specific patterns of commands

export HISTFILE=~/.bash_history  -  Sets the file where the command history is saved.

export HISTFILESIZE=-1           -  Sets no limit on the number of lines stored in the history file.

export HISTSIZE=-1               - Sets no limit on the number of commands retained in the shell history.

export HISTTIMEFORMAT="%F-%R "   -  Formats timestamps in the history as "YYYY-MM-DD HH" for each command.
 
```

## Accessing the File System

While the disk and devices used may differ depending on the requirements and the environment, the process is similar for all disks physically or virtually attached to the Linux OS .


The df command is used to see the attached devices .

```bash
df
```

But the blocked devices will not be listed in df . We can still list block devices using the lsblk command with the -a option to list all devices .

```bash
lsblk -a
```

We can get more info about the device using the losetup command with the -l option to list the device and the assigned interface path .

```bash
sudo losetup -l /dev/loopx
```

It is possible to acquire further information like the UUID of the image using the blkid with the interface as an argument.
```bash
sudo blkid /dev/loopx
```

---

## Task 3 - Creating a Forensic Image

Now that we have identified the device, we want to create an image. Let's go ahead and copy this disk to a raw image file for further inspection. This could also be done to another disk if needed, which is a common practice, but in this case, we will do it to a local file in our system with the .img extension to identify the file as an image.

Below, you can find a list of some common tools used to create images.

```
dd              -  A standard Unix utility for copying and converting files, often used for creating raw disk images

dc3dd           -  An enhanced version of dd with additional features for forensic imaging, including hashing and logging

ddrescue        -  A data recovery tool that efficiently copies data from damaged drives, attempting to rescue as much data as possible

FTK Imager      -  A GUI-based tool for creating forensic images, widely used for its ease of use and comprehensive features

Guymager        -  A GUI-based forensic imaging tool that supports various image formats and provides detailed logs

EWF tools       -  Tools for creating and handling Expert Witness Format (EWF) images, often used in digital forensics

```

## Imaging

To start, let's execute the dc3dd  command with the following parameters:
```bash
sudo dc3dd if=/dev/loop1 of=example1.img log=imaging_loop1.txt
```

-  The if option (input file), indicates the device we want to copy.

-  The of option (output file) indicates where we want to write the disk bytes.

-  The log options will save all the output into a file.

---

## Task 4 - Integrity Checking

Integrity checking in digital forensics imaging is crucial for ensuring that the forensic copy of data is identical to the original. This process uses cryptographic hash functions, such as MD5, SHA-1, or SHA-256, to generate unique hash values for both the original and the copied data. By comparing these hash values, investigators can confirm that the evidence was not altered during the imaging process, maintaining its reliability and admissibility in legal proceedings.

<img width="704" height="213" alt="image" src="https://github.com/user-attachments/assets/b60b6444-49d1-464d-925d-98c01bb66266" />

<br>
<br>

Now lets check the integrity of the image file .

<img width="565" height="58" alt="image" src="https://github.com/user-attachments/assets/0d0cd0b2-ee0e-4168-9881-c9de614f7309" />
<br>
<br>

After hashing both re showing the same hash value which means they both are have having identical content .

Question :
```
What is the MD5 hash of the image "exercise.img" located in /home/analyst/?
```

Answer:
```
1f1da616156f73083521478c334841bb 
```

---

## Task 5 - Other Types of Imaging

It is worth noting that other types of imaging procedures can be performed, not only on disk devices. These can include:

| Type | Description |
|------|-------------|
| Remote Imaging | Involves creating an image over the network, allowing it to acquire data without physical access to the device. |
| USB Images	   | Creates an image of a USB drive's contents. |
|  Docker Images  | While not strictly an image, it creates a snapshot of a Docker container's filesystem and configuration|


## Mounting an Image
To verify an image is valid and inspect its contents:

```bash
#Create a mount point
sudo mkdir -p /filepath

#Mount the image
sudo mount -o loop image.img /filepath

#Browse the contents
ls /filepath
```
Question : 
```
Mount the image "exercise.img" located in the analyst home directory folder.
What is the content of the file "flag.txt" located within exercise.img?
```
Answer:
```
THM{mounttt-mounttt-me}
```

---

## Task 6 - Practical Exercise

First of all open the virutal meachine from TryHackMe. Then run the lsblk command to find all the blocked devices and find the 1gb llop device.
<br>

<img width="523" height="201" alt="image" src="https://github.com/user-attachments/assets/3f824f99-3f64-4bbd-a88f-03c102d94c8a" />
<br>

The lets image the the loop using dc3dd command with thw output file name as practical.img
<br>

<img width="606" height="248" alt="image" src="https://github.com/user-attachments/assets/44dcea3e-15ea-4197-87f2-43ae5fc1bbad" />
<br>

Then let's check the integrity of the image file using md35 hash

```
practical@ip-10-49-165-212:/$ sudo md5sum practical.img 
1fab86e499934dda789c9c4aaf27101d  practical.img
```
Now let's create a new directory to mount the image file to attain the flag.txt file .

```
practical@ip-10-49-165-212:/$ sudo mkdir -p /mnt/practical

practical@ip-10-49-165-212:/$ sudo mount -o loop practical.img /mnt/practical/

practical@ip-10-49-165-212:/$ ls /mnt/practical/
flag.txt  lost+found  testpractical01  testpractical02  testpractical03  testpractical04  testpractical05

practical@ip-10-49-165-212:/$ cat /mnt/practical/flag.txt 
THM{well-done-imaginggggggg}
```

Question:
```
Create an image of the attached 1gb loop device. What is the MD5 hash of the image?
```
Answer :
```
1fab86e499934dda789c9c4aaf27101d
```

Question:
```
Mount the image from the 1 GB loop device. What is the content of the file "flag.txt"?
```

Answer :
```
THM{well-done-imaginggggggg}
```
---
