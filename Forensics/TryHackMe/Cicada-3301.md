# Cicada-3301 Vol:1 - TryHackMe
A basic steganography and cryptography challenge room based on the Cicada 3301 challenges

---

# Task 1  -  Download

This section introduces the challenge and gives context about Cicada 3301 . This level dosent require any type of solving  . You just have to download the zip file given in your local computer and unzip it .  Linux operating system be better an option to do operations with this file rather than windows

---

# Task 2  - Analyze the audio

We need to inspect the audio file “ 3001.wav ”  that we have just extracted from the zip file. It tells us to use the Sonic Viewer . 

If we apply necessary filters, we can find the thing we want. 

After many trail and error ,we apply Add Spectogram from the Layer tab . We got a QR code as a output . 

Then change the color scheme into White on Black .

we will got a QR code which while scanning will lead to a webpage . That will lead us in the future tasks 

##  Found
```
Link : https://pastebin.com/wphPq0Aa
```

---

# Task 3  - Decode the passphrase 


From the link we found from the last task , we found the information : 

             Passphrase  :  SG01Ul80X1A0NTVtaHA0NTMh 
             Key         :  Q2ljYWRh

It is encoded in base64 encoding . so using online tools like cyberchef to decode this passphrase and key 

             Decoded passpahrase   :  Hm5R_4_P455mhp453!
             Decoded key           :  Cicada

Using this passphrase and key as plain text and key respectively in Vigenere ciper encoding method we get

             Text              :  Hm5R_4_P455mhp453!
             key               :  Cicada 
             Final passphrase  :  Ju5T_4_P455phr453!

## Found 
we found the decoded passphrasea and key and we found the final passphrase

             Decoded passpahrase   :  Hm5R_4_P455mhp453!
             Decoded key           :  Cicada
             Final passphrase      :  Ju5T_4_P455phr453!

---

# Task 4  - Gather the metadata 

Now we are going to analyze the image we got when we unzipped the file “Welcome .jpg”  . For analyzing the image we are going to use a steganography tool called steghide .
Use the previous decoded passphrase for this .

     Command      :  steghide extract -sf welcome.jpg 
     
     Passphrase   :  Hm5R_4_P455mhp453!

After analyzing the image we will get a txt file as the output . when we cat that file we will get a link which will lead us in the next task


## Found 
    Link     :    https://imgur.com/a/c0ZSZga

---

# Task 5  - Find hidden files 

By using the above link , we will get an image . Download that image to your local computer for further analysing . For this we are going to use a steganography tool called outguess 

       Tool used    :   outguess
       Command      :   outguess  -r  img.png  output.txt

After running this command we will get a text file as an output.txt which will lead us in the next task .

## Found
      command used : outguess

---

# Task 6 - Book ciper 

When we check the output of the previous step we got a bunch of things 

          A book code as a  hash
          Some integers with the instructions
          And a PGP signature
          
Though it is mentioned as SHA1 hash , After brute forcing method we found out that it is a 
SHA512 hash . So upon using an online hash cracking application we got the web address
 
       Link  :  https://pastebin.com/6FNiVLh5

In that web address we can see some proverbs . After various trails we found the clue which is nothing 
but the integers we got while analyzing the second image . (Ex : I 13:5 , I 23 : -1 .. etc) .  For example

      I 13:5   means 13th line 5 character (Excluding spacebars )
      I 23:-1  means 23rd line -1 character (3)

Like this if we did for the entire integers we will again get a web address 
 
      Link : https://bit.ly/39pw2NH

## Found  

     hash          :  SHA512
     cracked hash  :  https://pastebin.com/6FNiVLh5
     decodec  Link : https://bit.ly/39pw2NH

---

# Task 7  - Final Song (Fianl Task)


On visiting the above web address ,  we can see a song which is ready to be played . Play and listen to 
that song  . 
 
    Name of the song  :  Cicada 3301 - The Instar Emergence

With this the Cicada 3301 Vol : 1 comes to an end

## Found
    Name of the song  :  Cicada 3301 - The Instar Emergence
