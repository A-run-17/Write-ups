## Disk Imaging
A forensic image is an electronic copy of a drive (e.g. a hard drive, USB, etc.). It’s a bit-by-­bit or bitstream file that’s an exact, unaltered copy of the media being duplicated.

Wikipedia said that the most straight­forward disk imaging method is to read a disk from start to finish and write the data to a forensics image format. “This can be a time-consuming process, especially for disks with a large capacity."

## Checksum

Validating files is one of the most important aspects of disk forensics. This hash will change if any part of file is changed, making it great for catching alterations to the original source.

Checking the hash of a download is also good practice when using third-party vendors for common software. It catches tampered files that could potentially contain malware.

<img width="2399" height="709" alt="sha" src="https://github.com/user-attachments/assets/62041605-ee73-4c2a-adaa-8c5483815031" />

We can use tools like md5sum or sha256sum from the command line to generate the hashes of a file. Observe the different hash when the file is altered.


https://github.com/user-attachments/assets/db34096a-25ed-431a-8301-371f854b96cd

## Write Blocker
It's common practice to use a write blocker before imagining a disk. This prevents unintended writes to the disk to maintain integrity.

Kali Linux has a "forensics" mode that features a write blocker and is designed for all sorts of forensics in mind.

## Why image a disk?
Prevents tampering with the original data­ evidence
Allows you to play around with the copy, without worrying about messing up the original


