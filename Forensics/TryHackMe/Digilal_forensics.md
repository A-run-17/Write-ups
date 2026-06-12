# Intro to Digital Forensics - TryHackMe

Learn about digital forensics and related processes and experiment with a practical example.

## Task 1 - Introduction to digital Forensics 

Forensics is the application of science to investigate crimes and establish facts. With the use and spread of digital systems, such as computers and smartphones, a new branch of forensics was born to investigate related crimes: computer forensics, which later evolved into, digital forensics.

Digital forensics is the application of computer science to investigate digital evidence for a legal purpose. Digital forensics is used in two types of investigations:

1. Public-sector investigations refer to the investigations carried out by government and law enforcement agencies. They would be part of a crime or civil investigation.


2. Private-sector investigations refer to the investigations carried out by corporate bodies by assigning a private investigator, whether in-house or outsourced. They are triggered by corporate policy violations.


Question :
```
Consider the desk in the photo above. In addition to the smartphone, camera, and SD cards,
what would be interesting for digital forensics?
```

Answer :
```
laptop
```

---

## Task 2 - Digital Forensics Process

As a digital forensics investigator, you arrive at a scene similar to the one shown in the image above. What should you do as a digital forensics investigator? After getting the proper legal authorization, the basic plan goes as follows:

- Acquire the evidence

- Establish a chain of custody

- Place the evidence in a secure container:
  
- Transport the evidence to your digital forensics lab.

Generally, according to the former director of the Defense Computer Forensics Laboratory, Ken Zatyko, digital forensics includes :

- Proper legal authority before starting

- Chain of custody maintained throughout

- Hash validation to confirm files are unmodified

- Use of validated tools only

- Repeatability of findings
  
- Final written report

Question :
```
It is essential to keep track of who is handling it at any point in time to ensure that evidence is admissible in the court of law.
What is the name of the documentation that would help establish that?
```

Answer :
```
Chain of Custody
```

---

## Task 3 -  Practical Example of Digital Forensics

For this You can download the attached file to your local machine for inspection; however, for your convenience we have added the files to the AttackBox. To follow along, open the terminal on the AttackBox, then go to the directory /root/Rooms/introdigitalforensics

## Document Metadata

We can try to read the metadata using the program pdfinfo. Pdfinfo displays various metadata related to a PDF file, such as title, subject, author, creator, and creation date. 



<img width="750" height="438" alt="image" src="https://github.com/user-attachments/assets/6f9cff31-8ff2-4033-bd58-5faf85418302" />


From this we can see the metadata of the pdf file .

Question : 
```
Using pdfinfo, find out the author of the attached PDF file, ransom-letter.pdf.
```

Answer:
```
Ann Gree Shepherd
```

## Photo EXIF Data

EXIF stands for Exchangeable Image File Format; it is a standard for saving metadata to image files. Whenever you take a photo with your smartphone or with your digital camera, plenty of information gets embedded in the image. The following are examples of metadata that can be found in the original digital images:

- Camera model / Smartphone model

- Date and time of image capture

- Photo settings such as focal length, aperture, shutter speed, and ISO settings

There are many online and offline tools to read the EXIF data from images. One command-line tool is exiftool. ExifTool is used to read and write metadata in various file types, such as JPEG images


<img width="720" height="450" alt="image" src="https://github.com/user-attachments/assets/7e85b1e2-5bda-42ed-9cf7-5785930b5a36" />

Using this exiftool we can find the metadata of an image file .


Question :
```
Using exiftool or any similar tool, try to find where the kidnappers took the image they attached to their document.
What is the name of the street?
```

Answer :
```
Milk Street
```



Question :
```
What is the model name of the camera used to take this photo?
```

Answer:
```
Canon EOS R6
```
