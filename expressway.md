Expressway - HTB Writeup
![Express Top](images/expressway/express-top.png)

This is an easy difficulty box that is exploited via ike and root was gained with basic CVE.
As firs i started with an nmap scan however it only revealed ssh so that is unsusual.
Since there were no other ports open that could serve as attack surface i tried to scan for `UDP` ports.
![Express Nmap](images/expressway/express-nmap.png)

![Express Bash](images/expressway/express-bash.png)
![Express IKE1](images/expressway/express-ike1.png)
![Express IKE2](images/expressway/express-ike2.png)
![Express IKE3](images/expressway/express-ike3.png)
![Express Root](images/expressway/express-root.png)
![Express User](images/expressway/express-user.png)
