## File Formats
File Extensions are not the sole way to identify the type of a file, files have certain leading bytes called file signatures which allow programs to parse the data in a consistent manner. Files can also contain additional "hidden" data called metadata which can be useful in finding out information about the context of a file's data.

## File Signatures
File signatures (also known as File Magic Numbers) are bytes within a file used to identify the format of the file. Generally they’re 2-4 bytes long, found at the beginning of a file.

## What is it used for?
Files can sometimes come without an extension, or with incorrect ones. We use file signature analysis to identify the format (file type) of the file. Programs need to know the file type in order to open it properly. It's useful to analyze the file type before any forensics software.

Signatures of some common files :

```
25 50 44 46                 -   PDF, FDF
                            Trailers:
                            0A 25 25 45 4F 46 (.%%EOF)
                            0A 25 25 45 4F 46 0A (.%%EOF.)
                            0D 0A 25 25 45 4F 46 0D 0A (..%%EOF..)
                            0D 25 25 45 4F 46 0D (.%%EOF.)


89 50 4E 47 0D 0A 1A 0A     -   PNG (Portable Network Graphics file)
                            Trailer: 49 45 4E 44 AE 42 60 82 (IEND®B`‚...)



FF D8 FF E0 xx xx 4A 46    -  JPGE / JFIF ( JPEG File Interchange Format )
49 46 00                   Trailer: FF D9



FF D8 FF E1 xx xx 45 78    - JPGE / EXIF (Exchangeable Image File Format)
69 66 00	 	               Trailer: FF D9


FF D8 FF E8 xx xx 53 50    -  JPG	/ SPIFF ( Still Picture Interchange File Format ) 
49 46 46 00                Trailer: FF D9


EB 3C 90 2A                -  IMG


FF Ex	/  FF Fx 	       -   MPEG, MPG, MP3	 MPEG audio file frame synch pattern 


66 74 79 70 4D 53 4E 56	   -  MP4	MPEG-4 video file

```
Note  : Here tailers denote the end of file  wheres as the signature will be at the beginning of the file . Some may have trailer and may not have 

### How do you find the file signature?
You need to be able to look at the binary data that constitutes the file you’re examining. To do this, you’ll use a hexadecimal editor.

<img width="996" height="619" alt="image" src="https://github.com/user-attachments/assets/6185cd2c-381e-4530-816c-7d1b1cf8aaaa" />


Here we found the SOI (start of image ) and EOI ( End of image ) of a jpge file  


But using simple linux commnd , file format can be identified even without the extension of the file

<img width="1910" height="101" alt="image" src="https://github.com/user-attachments/assets/cdf05784-3ae7-46f9-b61b-a381a6aa1ff3" />
