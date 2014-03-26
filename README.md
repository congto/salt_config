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
<pre>
	<code>
	apt-get update
</code>
</pre>
Install SaltStack:
<pre>
	<code>
	apt-get install salt-master
	apt-get install salt-minion
	apt-get install salt-syndic 
</code>
</pre>
2.  Configure Salt Master:

- Open file /etc/salt/master then modify paramenter below:
<pre>
	<code>
	interface: IP LAN of server.Eg: 192.168.1.10
	worker_threads: 100
	file_roots:
  		base:
    		- /srv/salt

</code>
</pre>

- Create file top.sls :
In this example, i will user salt to install and config nginx with templates file.
File structure :
<pre>
	<code>

	/srv/salt:
		top.sls
		nginx/site-enabled/vhost.conf
		nginx/nginx.conf
</code>
</pre>
 	

 	- top.sls 

<pre>
	<code>
 		nginx:
 	    	pkg:
 	      	   - installed
 	    /etc/nginx/nginx.conf:
  		   file.managed:
    	      - source: salt://nginx/nginx.conf
        /etc/nginx/site-enabled/vhost.conf
    	   file.managed:
    	      - source: salt://nginx/site-enabled/vhost.conf

 </code>
</pre>

3. Configure Salt minion on client:

Open file /etc/salt/minion then modify paramenter below:
<pre>
	<code>
  	- master : ip of salt master server
  	- id : hostname or ip of client, eg: node-01
  	</code>
  </pre>

Restart salt minion : 

/etc/init.d/salt-minion restart
<p>
4. Add client on salt master:
List client key on salt master, type command:
<pre>
	<code>
	salt-key -L
	</code>
</pre>
Add client key on salt master, type command:
	pre>
	<code>
	salt-key -A
</code>
</pre>
5. Install package:
in this example, i set client id : node-01.
<pre>
	<code>
	salt "node-01" state.hightstate
</code>
</pre>
After finish task, nginx package was installed in client with template config file


Finish example




