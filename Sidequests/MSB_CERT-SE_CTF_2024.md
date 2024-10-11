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

IP they found suspicious. Involved in C2 and exfiltration activities. 195.200.72.82

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

### FLAGS:

CTF\[E65D46AD10F92508F500944B53168930]
PASS CTF\[AES128]