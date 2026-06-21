# RIFFHACK: Black Market Break-In

## 1. The Crawler's Courtesy

`RIFFHACK`  `FLAG` `Easy`

### Description

The marketplace tried to politely hide one operator scrap from search engines. Courtesy files are not access control, and crawlers are not the only ones who can read them.

### Solution

- Go to robots.txt :  `http://159.89.230.27/robots.txt`

- Then go to the /operator-cache-drop page which ahs been bolcked by all the crawlers . 

- Find the flag

Flag : `bitflag{r0b0ts_4r3_n0t_4_s3cr3t_v4ult}`


---


## 2. The Glitchy Contact System

`RIFFHACK`  `FLAG`  `Easy`

### Description
Something in the marketplace isn't working quite right. Those who dig deeper find more than they bargained for.

### Solution

- Go to contact page at bottom :  `http://157.245.135.174/contact`

- Then go to console at instructed

Flag  :` bitflag{d3bug_m0d3_1s_d4ng3r0us}`


---


## 3. The Exposed Orders

`RIFFHACK`  `FLAG`  `Easy`

### Description

Order history should be private, but the marketplace leaves a few loose threads. Can you follow one to something that is not yours?

### Solution

- Go to aby tools and take a look at the Operator Reviews . Form this you can collect the usid information.

- User id Found : `k7m3n`  `abc12`  `xyz78`

- Now we are going to directly try to connect the api for the order history details

- Then go to each of the users for finding the flag : `http://159.89.237.133/api/orders?userId={user_id}`

Flag  :  `bitflag{1d0r_1s_4_d4ng3r0us_g4m3}`


---


## 4.Black Mirror Memory Fragments

`Forensics`  `FLAG`  `Medium`

### Description

A corrupted mobile backup is all that remains of a wiped conversation. Sort the real evidence from the noise, reconstruct what actually happened, and recover the hidden key.

### Solution  

- unzip the file `tar -xvf 1781949803_black-mirror-memory-fragments-backup.tar` . Two directoryies will we extracted .

- The `quarantine_note.txt` in the `decoy/` directory is dummy and doesn't any information about the flag .

- The conversation in `messages.db` at `app_export/` directory is analyzed and found that two bas64 encoded part .

```bash
$ echo "Yml0Y3Rme3tzbTF0aDNyMzNuX3RocjM0ZF9y" | base64 -d
bitctf{{sm1th3r33n_thr34d_
```

- From the converstion we understood `att_river_final`  `att_river_photo` contain important informaiton.

- we found the files with attachment id  `att_river_final`  `att_river_photo` in attachment_index.json at the `metadata/` subdirectory of the `app_export/` directory.

- First the files in the `cache/` subdirectory are checked and then the content are viewed as per the attachment id.

```bash
file ch_5a.dat ch_2c.dat ch_9f.dat
cat ch_5a.dat ch_2c.dat ch_9f.dat
```

- This gives as that the UserComment needs to be found  and base64-decoded for the rest of the flag .

- Then the second set of files are checked and then the contents are viewed .

```bash
file ch_x7.dat ch_y4.dat
cat ch_x7.dat ch_y4.dat
```

- Here a usercomment has been found and it has been base64 decoed for the rest of the flag.

```bash
echo "M2Fzc2VtYmxlZF80Y3Jvc3NfZnJhZ21lbnRzfX0=" | base64 -d
3assembled_4cross_fragments}}
```

Flag : `bitctf{{sm1th3r33n_thr34d_3assembled_4cross_fragments}}`

