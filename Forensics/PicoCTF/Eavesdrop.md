# Eavesdrop - CyLab Security Academy

Category   : Forensics

Difficulty : Medium


## Method udes for resolving

Download the given packet file from the task. First of all we will use strings command in this .pcap file and see what is inside this file.

Command:
```
strings capture.flag.pcap
```

On using the strings command we can see that there is a conversation in the file.

Conversation :
```
Hey, how do you decrypt this file again?
You're serious?
Yeah, I'm serious
*sigh* openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
Ok, great, thanks.
Let's use Discord next time, it's more secure.
C'mon, no one knows we use this program like this!
Whatever.
Hey.
Yeah?
Could you transfer the file to me again?
Oh great. Ok, over 9002?
Yeah, listening.
Sent it
Got it.
You're unbelievable
```
From this command:
```
openssl des3 -d -salt -in file.des3 -out file.txt -k supersecretpassword123
```
- -d - indiactes decrypt
- -salt indicates the encrypted file was created using a salt
- -in indicates the input file
- -out indicates the output file
- -k indicates the key

From this we can come to a conclusion that the flag has been encrypted using openssl (des3) andthere is an file exchange involved in the conversation. 
Our goal for now is to identify the file involded in it. Now let's open this .pcap file in the **WireShark** application.

Since we know that the encrypted file was created using a salt, we can find the packet which has salt.

<img width="732" height="511" alt="image" src="https://github.com/user-attachments/assets/d3dba12a-d0e3-4b90-8221-f8a1593bb256" />


Here we can see that the data start with 5361 6c74 6564 5f5f. which means
| Hex | ASCII |
|-----|-----|
|53	|S|
|61	|a|
|6c	|l|
|74	|t|
|65	|e|
|64	|d|
|5f	|_|
|5f	|_|

So finally it becomes : Salted__

```
Data: 564768706379427063794230614755676332566a636d56304f69427761574e76513152476531497a4e45524a546b64665445394c5a46383d
```

Taking this entire data and converting it back to binary and then store it in the file named **file.dec3** and run the above command or 
Simply do it all together in one single command

```bash
echo "53616c7465645f5fd30c863ca650da1fff4fbe72a086457e63626beda615c692cdd27026ae7deea6d1e918b3d40a7f46" | xxd -r -p | openssl des3 -d -salt -k supersecretpassword123
```
You will get the flag directly a output of the above command which is found to be
```
picoCTF{nc_73115_411_5786acc3}
```
