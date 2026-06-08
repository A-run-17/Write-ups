## Hex Editor
A hexadecimal (hex) editor (also called a binary file editor or byte editor) is a computer program you can use to manipulate the fundamental binary data that constitutes a computer file. The name “hex” comes from “hexadecimal,” a standard numerical format for representing binary data. A typical computer file occupies multiple areas on the platter(s) of a disk drive, whose contents are combined to form the file.

Hex editors that are designed to parse and edit sector data from the physical segments of floppy or hard disks are sometimes called sector editors or disk editors. A hex editor is used to see or edit the raw, exact contents of a file. Hex editors may used to correct data corrupted by a system or application. A list of editors can be found on the forensics Wiki.

Your hex editor should have two sections, the hexadecimal and character representations of that data. It's helpful to also have a "goto" feature in your hex editor to navigate large dumps of data.

## Example
A simple CTF challenge is modifying the header of a file. In this example, I changed the first byte of this file to AA instead of the conventional FF needed in the JFIF(JPEG File Interchangable Format). Observe how it changes the behavior of the file command.


<img width="1062" height="368" alt="image" src="https://github.com/user-attachments/assets/b290f5c3-7426-48a7-8d02-5c15ddc8dc0f" />

Using a hexeditor like hexcurse, we can change the header back to FF to be recognizable again by file.


<img width="1695" height="482" alt="image" src="https://github.com/user-attachments/assets/d2bc197c-8a70-4237-9877-18e33f33568c" />
