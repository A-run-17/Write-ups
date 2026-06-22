# RIFFHACK: Black Market Break-In

# 1. The Crawler's Courtesy


`RIFFHACK`  `FLAG`  `Easy`
## Description

> The marketplace tried to politely hide one operator scrap from search engines. Courtesy files are not access control, and crawlers are not the only ones who can read them.

## Objective

Locate the hidden flag by examining publicly accessible files intended for search engine crawlers.
## Solution

The challenge description suggests that sensitive information may be referenced in the `robots.txt` file. This file is commonly used to instruct web crawlers which paths should not be indexed, but it **does not prevent users from accessing those paths directly**.

### Step 1: Check `robots.txt`

Visit:
```
http://159.89.230.27/robots.txt
```
The file contains the following entry:
``` 
Disallow: /operator-cache-drop
```

This indicates that the `/operator-cache-drop` directory is excluded from search engine indexing.

### Step 2: Access the Disallowed Directory

Since `robots.txt` is only a crawler directive and **not an access control mechanism**, the directory can still be accessed directly.

Navigate to:

```
http://159.89.230.27/operator-cache-drop
```
The page reveals the flag.
## Flag

```
bitflag{r0b0ts_4r3_n0t_4_s3cr3t_v4ult}
```


---

# 2. The Glitchy Contact System

`RIFFHACK`  `FLAG`  `Easy`

## Description

> Something in the marketplace isn't working quite right. Those who dig deeper find more than they bargained for.
## Objective

Identify information unintentionally exposed through the application's client-side debugging features.

## Solution

The challenge hints that the contact page contains additional information that is not immediately visible.

### Step 1: Navigate to the Contact Page

Visit the following URL:

```
http://157.245.135.174/contact
```

### Step 2: Open the Browser Developer Tools

Open the browser's **Developer Tools** (`F12` or `Ctrl + Shift + I`) and switch to the **Console** tab.

Upon inspecting the console output, a debug message reveals the challenge flag.

## Flag

```
bitflag{d3bug_m0d3_1s_d4ng3r0us}
```

---


# 3. The Exposed Orders

`RIFFHACK`  `FLAG`  `Easy`

## Description

> Order history should be private, but the marketplace leaves a few loose threads. Can you follow one to something that is not yours?

## Objective

Identify and exploit an **Insecure Direct Object Reference (IDOR)** vulnerability to access another user's order history and retrieve the flag.

## Solution

The challenge hints that order history belonging to other users may be accessible by manipulating user identifiers.

### Step 1: Enumerate User IDs

Navigate to the **Operator Reviews** section available under **Tools**.
The review data exposes several user identifiers:
```
k7m3n
abc12
xyz78
```
### Step 2: Inspect the API Endpoint

The application exposes an API endpoint for retrieving order history:

```
http://159.89.237.133/api/orders?userId={user_id}
```
Replace `{user_id}` with each of the discovered user IDs.

Example:
```
http://159.89.237.133/api/orders?userId=xyz78
```
Repeat the request for each user ID until the response containing the flag is identified.

This demonstrates an **Insecure Direct Object Reference (IDOR)** vulnerability, where the application fails to verify whether the authenticated user is authorized to access the requested resource.
## Flag
```
bitflag{1d0r_1s_4_d4ng3r0us_g4m3}
```

---

# 4. Black Mirror Memory Fragments


`Forensics`  `FLAG`  `Medium`

## Description

> A corrupted mobile backup is all that remains of a wiped conversation. Sort the real evidence from the noise, reconstruct what actually happened, and recover the hidden key.

## Objective

Analyze the mobile backup, reconstruct the deleted conversation, and recover the hidden flag from the available artifacts.

## Solution

### Step 1: Extract the Backup Archive

Extract the provided archive:
```
tar -xvf 1781949803_black-mirror-memory-fragments-backup.tar
```
The archive extracts two directories:  `app_export/`  ,  `decoy/`


### Step 2: Identify the Decoy

Inspecting the contents of the `decoy/` directory reveals a file named:
```
quarantine_note.txt
```
This file is a decoy and does not contain any useful information related to the challenge.

### Step 3: Analyze the Message Database

The primary evidence is stored in:
```
app_export/messages.db
```
Examining the conversation reveals 2  Base64-encoded strings separately. Decoding it will give 
```
$ echo "Yml0Y3Rme3tzbTF0aDNyMzNuX3RocjM0ZF9y" | base64 -d
bitctf{{sm1th3r33n_thr34d_
```
This provides the first half of the flag.

### Step 4: Identify Relevant Attachments

The conversation from `messages.db` references two attachment IDs:  `att_river_final`  `att_river_photo` which contains sensitive information.
Searching the attachment metadata file:

```
app_export/metadata/attachment_index.json
```
reveals the corresponding cached files associated with these attachment IDs.

### Step 5: Inspect Cached Files

First, inspect the cached files:

```
file ch_5a.dat ch_2c.dat ch_9f.dat
cat ch_5a.dat ch_2c.dat ch_9f.dat
```
The output indicates that the required information is stored in the **UserComment** field and must be Base64 decoded.

Next, inspect the remaining cache files:
```
file ch_x7.dat ch_y4.dat
cat ch_x7.dat ch_y4.dat
```
A Base64-encoded **UserComment** value is recovered:
```
M2Fzc2VtYmxlZF80Y3Jvc3NfZnJhZ21lbnRzfX0=
```
Decode it using:
```
$ echo "M2Fzc2VtYmxlZF80Y3Jvc3NfZnJhZ21lbnRzfX0=" | base64 -d
3assembled_4cross_fragments}}
```
This provides the remaining portion of the flag.
### Step 6: Reconstruct the Flag
Combining both decoded fragments results in:
## Flag
```
bitctf{{sm1th3r33n_thr34d_3assembled_4cross_fragments}}
```

