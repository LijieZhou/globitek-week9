# Project 10 - Honeypot

Time spent: 10 hours spent in total

> Objective: Setup a honeypot and provide a working demonstration of its features.

### Required: Overview & Setup

- [x] A basic writeup (250-500 words) on the `README.md` desribing the overall approach, resources/tools used, findings

There are two purposes for this honeypot project: 1) to minitor any brute-force attack on my home network. 2) to distract attacker from my production server. 

Tool: Kippo https://github.com/desaster/kippo
OS: Ubuntu 16.04

Findings:
After successfully setup Kippo, use another machine to ssh into the kippo server, the intrusion is successfully recorded in kippo.log

- [x] A specific, reproducible honeypot setup, ideally automated. There are several possibilities for this: 

	- Prerequisites: 

		- Kippo is a Python based application. Thus,we need to first install python libraries
		> sudo apt-get install python-twisted 
		- Note: The most updated kippo only works with twisted version 15.1.0 (most updated: 16.0.0.1). So please do install the right version. Otherwise, it is not compatible with Kippo. 

	- Setup:

		- Change ssh port from 22 to a random number. In my case, I use 4633 instead. There is a big debate about whether or not moving SSH port to a non-priviledged port. The reason that I decided to change it is that I want a more real disguise. 
		> vi /etc/ssh/sshd_config 
		Change Port from 22 to 4633
		> sudo service ssh restart

		- Next, Kippo needs to run as a non-priviledged user. Thus, create a user account and run Kippo under this account. 
		> sudo adduser kippo

		- Installation
		> git clone https://github.com/desaster/kippo.git
		
		- Configuration
			- Kippo comes pre-configured with port 2222. Thus, we need to change it to 4633. 
			> $ sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 4633
			- some optional configurations to make the honeypot look more attractive
				- Alternative rootpasswords: Kippo comes with pre-configured password "123456". You can change it to some more tricky ones to make attackers believe it is real. 
				> $ utils/passdb.py data/pass.db add root
				- Hostnames: edit the configuration file kippo.cfg to change the hostname more attractive like: 
				> hostname = accounting
		- Start Kippo Honeypot
			> ./start.sh


### Required: Demonstration

- [x] A basic writeup of the attack (what offensive tools were used, what specifically was detected by the honeypot)
	- In another terminal, the attacker tried to login. Password has been setup, so he/she tried to brute-force some common passwords. It didn't take him/her long to hack in with the default 123456. 

	- ![Screenshot1]https://drive.google.com/file/d/0BzsBD-G31f93SDVYb2NzZy03enc/view?usp=sharing
	- See in the red area, when the attacker tried to ssh into the honeypot (192.168.86.10:4633), it gets a warning (RSA fingerprint key will be generated). He/she continued and tried two passwords. He/she gained root priviledge afterwards(See area green)
- [x] An example of the data captured by the honeypot (example: IDS logs including IP, request paths, alerts triggered)
	- See kippo.log
- [x] A screen-cap of the attack being conducted
	- Take a look at the recording of the attacker's behavior in the log. 
	![Screenshot2]https://drive.google.com/file/d/0BzsBD-G31f93ODBsdkR2RkpVdG8/view?usp=sharing
    