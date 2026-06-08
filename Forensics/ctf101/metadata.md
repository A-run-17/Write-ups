## Metadata
Metadata is data about data. Different types of files have different metadata. The metadata on a photo could include dates, camera information, GPS location, comments, etc. For music, it could include the title, author, track number and album. CTF challenges often have you looking for specific clues in the metadata of a file (especially media files).

## How do I find it?
One of our favorite tools is exiftool, which displays metadata for an input file: - File size - Dimensions (width and height) - File type - Programs used to create (e.g. Photoshop) - OS used to create (e.g. Apple)

<img width="1177" height="696" alt="image" src="https://github.com/user-attachments/assets/7d3688d5-acc9-4f5b-a2dc-cce331c07265" />

## Timestamps
Timestamps are data that indicate the time of certain events (MAC): - Modification – when a file was modified - Access – when a file or entries were read or accessed - Creation – when files or entries were created

## Types of Timestamps

```
Modified                     → The time when the file’s content was last changed.
Accessed                     → The time when the file was last opened or read.
Created                      → The time when the file was originally created on the filesystem.
Date Changed (MFT)           → The time when the file’s metadata (permissions, rename, move, etc.) was changed in the NTFS Master File Table.
Filename Date Created (MFT)  → The creation timestamp stored in the filename attribute inside the MFT.
Filename Date Modified (MFT) → The last content modification time stored in the filename attribute inside the MFT.
Filename Date Accessed (MFT) → The last access time stored in the filename attribute inside the MFT.
INDX Entry Date Created      → The file creation time recorded in the NTFS directory index entry ($I30).
INDX Entry Date Modified     → The file modification time recorded in the directory index entry.
INDX Entry Date Accessed     → The file access time recorded in the directory index entry.
INDX Entry Date Changed      → The metadata change time recorded in the directory index entry.
```

Why do we care?
Certain events such as creating, moving, copying, opening, editing, etc. might affect the MAC times. If the MAC timestamps can be attained, a timeline of events could be created.


## Example 
We know that the BMP files fileA and fileD are the same, but that the JPEG files fileB and fileC are different somehow. So how can we find out what went on with these files?


<img width="805" height="159" alt="timestamp-6" src="https://github.com/user-attachments/assets/6a5abbd6-3641-4fe7-8303-f1bd5c768ea7" />

<img width="865" height="328" alt="timestamp-7" src="https://github.com/user-attachments/assets/39d6e6ff-b6e1-4e8d-95e7-142af30b23eb" />

<img width="805" height="302" alt="timestamp-8" src="https://github.com/user-attachments/assets/46e5d2cf-44f1-4b32-9341-5b0dfb6a8251" />


<img width="836" height="300" alt="timestamp-9" src="https://github.com/user-attachments/assets/1e31735b-e0cc-43db-b585-f6048ef30316" />

<img width="847" height="321" alt="timestamp-10" src="https://github.com/user-attachments/assets/73b979d9-0e5d-4d0f-97f3-0d5fe2f96723" />

<img width="819" height="288" alt="timestamp-11" src="https://github.com/user-attachments/assets/6a948b7d-108c-46e5-93ab-42e54e2cd952" />

<img width="839" height="294" alt="timestamp-12" src="https://github.com/user-attachments/assets/2e31a1d0-f820-43b4-ab7e-f68f68191462" />

<img width="829" height="303" alt="timestamp-13" src="https://github.com/user-attachments/assets/87298dc2-7c36-49cc-bca1-d5af42a6d49a" />

<img width="821" height="305" alt="timestamp-14" src="https://github.com/user-attachments/assets/71bc0166-76ca-477f-9828-ff25c0c31002" />

<img width="833" height="313" alt="timestamp-15" src="https://github.com/user-attachments/assets/3b9041ce-d1f7-4dc5-962f-ea47fa4608ef" />


So we can conclude that 

<img width="863" height="370" alt="timestamp-16" src="https://github.com/user-attachments/assets/019e1f03-6f84-441d-b9b6-a148e176241c" />

This how the investigation need to be done!! .







