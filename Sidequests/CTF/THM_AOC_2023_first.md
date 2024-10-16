#CTF 
## Info

	Router: IP: 10.0.0.1 MAC: 22:c7:12:c7:e2:35
	IP: MAC: 2a:84:49:ac:f9:d8
	: IP: 10.0.0.3 MAC: d2:67:d1:3f:36:ec
## WIFI

The SSID was easy to find, it was in plaintext of the pcap file on pretty much all the packets. "FreeWifiBFC".
Now to try and crack the password.
### Cracking
Using [[Pen testing tools#Aircrack-ng|aircrack-ng]] for this. First though I cleaned the file using [[Pen testing tools#Wpaclean|wpaclean]]. 
I got the SSID mac address also from wpaclean, it also provided the SSID. Tried some wordlists but rockyou is the one that worked.
These are the commands:

`wpaclean <cleaned_pcap_file> <pcap_file>`
`aircrack-ng -s -a 2 -w <rockyou.txt> -b 22:c7:12:c7:e2:35 <cleaned_pcap_file>`

And the password was "Christmas".

### TCP stream 1005

The last tcp stream is interesting. Someone connected to the system as admin and downloaded [[Pen testing tools#Mimikatz|mimikatz]], and then exported certificates. He printed the certificate as a base64 string so I have it.

#### The certificate

The following powershell commands was used to turn the certificate string into a .pfx file like it was originally.

`$Base64String = "<the base64 string>"`
`$Bytes = [Convert]::FromBase64String($Base64String)`
`[IO.File]::WriteAllBytes("<filepath.pfx>", $Bytes)`

<section mimikatzPassword>And since mimikatz was used, the pasword should be 'mimikatz'.
Now we can decrypt all this TLS traffic. </section>

## RDP

A lot of RDP packets on this layer.


Now we will use [[Pen testing tools#PyRDP Convert|pyrdp-convert]] to replay the RDP pdu's. 
After exporting the pcap file by `File > Export PDUs` and selecting `OSI Layer 7`. We run the command:
`pyrdp-convert -f replay '<filepath.pcap>'`


#### C2 or C&C