# Torrent Analyze - CyLab Security Academy

Category   - Forensics

Difficulty - Medium

## Method used for resolving

First of all download the packet captue file avaliable in the task.

For this task we have to understand some basic concepts in torrent.


What are seeds, peers, and leechers in torrent?

```
Seed     - is a person who has a torrent file open in their client (let’s say the same file you are trying to download)
           and the only difference between you and them is that they have the complete file downloaded already and are
           now “seeding” – sharing the file with peers but not downloading any parts of the file from others.

Leechers -  are those who are downloading and uploading at the same time. If a user starts sharing a
            file that he already has, and downloads what other users have already uploaded or are in
            the process of uploading a torrent file, he becomes a leecher.

Peer     -  is someone who is both downloading and uploading the file in the swarm. Files are downloaded in pieces.
            When a user downloads some pieces, he then automatically starts uploading it.
```

Now we can open this file in  **WireShark** application. As there is large number of packets in the file, we can apply some filters for easy analysis

Apply this display filter:
```
bt-dht
```
This shows packets that are related to only BitTorrent Distributed Hash Table (DHT) protocol. This narrows down to only 175 packets roughly 0.3% of the original packets in the given file

<img width="725" height="509" alt="image" src="https://github.com/user-attachments/assets/905ca274-ac6b-4151-b46c-cb51c0a458a0" />

As we can see that one hash repeatedly appears in the file 
```
Value: e2467cbf021192c241367b892230dc1e05c0580e
```

Search this hash on a public torrent indexer / search engine (e.g. via Google or a site like Torrentz/Linuxtracker). The hash resolves to a well-known Ubuntu ISO torrent:

```
ubuntu-19.10-desktop-amd64.iso
```

From the information given from the task we know that the flag will be in the format of **picoCTF{filename}**

So the flag is 
```
picoCTF{ubuntu-19.10-desktop-amd64.iso}
```


