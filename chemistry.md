# Chemistry - HTB Writeup
Chemistry is an easy difficulty machine. The solving took many steps however the journey was natural so it didn't pose much trouble.
Tasks Included:
-identifying vulnerable services
-exploiting vulnerable services
-lateral movement
-ssh port tunneling
## Initial exploitation
As always i started with nmap scan.
The application used ssh which is ordinary and port 5000 was open.
The script and port scan revealed that the service can be accessed via http. 
The important is the python version `Python 3.9.5` that  will be utilized later.
To acces it all you  need to do is visit the ip on browser with thee port specified : `http://10.129.231.170:5000`
On the page we are presented with login and register form.
Since we don't have any login credential we can register with new account and access the application.
Now the page is used for storing and viewing `.cif` files and examplpe is also provided.
The .cif is used to store crystallography data that describe crystalic structure. 
So this application serves some chemistry workers to analyze their data.
However the python version used that we enumerated earlier is vulnerable.
Uploading malicious .cif file can trigger RCE because of broken eval() function.
For more detail check out this https://www.vicarius.io/vsociety/posts/critical-security-flaw-in-pymatgen-library-cve-2024-23346
