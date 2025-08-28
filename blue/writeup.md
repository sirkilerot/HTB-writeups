Blue – HackTheBox Writeup

This is my first write-up on this page.
As this is the first machine I managed to crack without following any write-up.

The Blue is, by ratings, the easiest machine on HTB.
The machine runs a Windows 7 version that has an easily exploitable vulnerability via SMB shares.

Enumeration

At first I ran an nmap scan as usual.
There were 3 TCP ports open along with some noise not worth attention.
After running the deeper scan with script and version flags I found out the SMB username and operating system.

SMB Enumeration

Then I enumerated SMB shares with the obtained username, yet nothing of interest was found there. At least it validated the user.

Exploitation

Because the machine was retired there was a hint by HTB that I should search for MS17-010.
So I did.

The vulnerability was EternalBlue, which exploited SMBv1 on Windows and allowed RCE.
This was linked to a great ransomware attack called WannaCry back in 2017 where this attack infected a whole lot of computers around the world.

I looked for an exploit and found out that it is in Metasploit.
So I searched it up.

At first I used the scanner for detection of this vulnerability and it confirmed that the target is indeed vulnerable.

Then I used an exploit called EternalRomance for RCE on Windows because the description seemed suitable. However this was a mistake — I tried inserting a remote shell to be executed and to capture it with netcat, but this was probably my misunderstanding of the exploit.

Then I used the first one suggested — EternalBlue.
But after running it I didn’t get the session, so I was confused. Then I set up the SMB username — since it wasn’t mandatory I thought it would work without it.
I ran it and once again a session was not created.

I looked closely at the OPTIONS and there I found the LHOST was set to my IP address. However I was connected to the target via VPN, so I edited LHOST to my address on that network and voilà! A Meterpreter session was created.

Then I grabbed the user flag and, since I already had admin access, the root flag.

Conclusion

This machine was really straightforward indeed, and it used a real-world vulnerability for exploitation, which is very exciting.
p scan as usual.
There were 3 TCP ports open along with some noise not worth attention.
After running the deeper scan with script and version flags I found out the SMB username and operating system.

SMB Enumeration

Then I enumerated SMB shares with the obtained username, yet nothing of interest was found there. At least it validated the user.

Exploitation

Because the machine was retired there was a hint by HTB that I should search for MS17-010.
So I did.                                                          

The vulnerability was EternalBlue, which exploited SMBv1 on Windows and allowed RCE.
This was linked to a great ransomware attack called WannaCry back in 2017 where this attack infected a whole lot of computers around the world.

I looked for an exploit and found out that it is in Metasploit.
So I searched it up.

At first I used the scanner for detection of this vulnerability and it confirmed that the target is indeed vulnerable.

Then I used an exploit called EternalRomance for RCE on Windows because the description seemed suitable. However this was a mistake — I tried inserting a re>

Then I used the first one suggested — EternalBlue.
But after running it I didn’t get the session, so I was confused. Then I set up the SMB username — since it wasn’t mandatory I thought it would work without>
I ran it and once again a session was not created.

I looked closely at the OPTIONS and there I found the LHOST was set to my IP address. However I was connected to the target via VPN, so I edited LHOST to my>

Then I grabbed the user flag and, since I already had admin access, the root flag.

Conclusion

This machine was really straightforward indeed, and it used a real-world vulnerability for exploitation, which is very exciting.


