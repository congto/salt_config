1. Config APT sources list and install Salt:
The following line is needed in either /etc/apt/sources.list or a file in /etc/apt/sources.list.d
<pre>
<code>	
	deb http://http.debian.net/debian wheezy main
	deb-src http://http.debian.net/debian wheezy main

	deb http://http.debian.net/debian wheezy-updates main
	deb-src http://http.debian.net/debian wheezy-updates main
</code>
</pre>

Import the repository key:

	wget -q -O- "http://debian.saltstack.com/debian-salt-team-joehealy.gpg.key" | apt-key add -

Update the package database:
	apt-get update

Install SaltStack:
	apt-get install salt-master
	apt-get install salt-minion
	apt-get install salt-syndic 

2.  Configure Salt Master:

- Open file /etc/salt/master then modify paramenter below:
	interface: IP LAN of server.Eg: 192.168.1.10
	worker_threads: 100
	file_roots:
  		base:
    		- /srv/salt



- Create file top.sls :
In this example, i will user salt to install and config nginx with templates file.
File structure :

	/srv/salt:
		top.sls
		nginx/site-enabled/vhost.conf
		nginx/nginx.conf

 	

 	- top.sls 


 		nginx:
 	    	pkg:
 	      	   - installed
 	    /etc/nginx/nginx.conf:
  		   file.managed:
    	      - source: salt://nginx/nginx.conf
        /etc/nginx/site-enabled/vhost.conf
    	   file.managed:
    	      - source: salt://nginx/site-enabled/vhost.conf

 
3. Configure Salt minion on client:

Open file /etc/salt/minion then modify paramenter below:
  	- master : ip of salt master server
  	- id : hostname or ip of client, eg: node-01
Restart salt minion : /etc/init.d/salt-minion restart

4. Add client on salt master:
List client key on salt master, type command:
	salt-key -L
Add client key on salt master, type command:
	salt-key -A

5. Install package:
in this example, i set client id : node-01.

	salt "node-01" state.hightstate

After finish task, nginx package was installed in client with template config file


Finish example




