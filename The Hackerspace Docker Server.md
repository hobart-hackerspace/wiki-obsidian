# Basics
## Server setup

- This is small Lenovo desktop box  that sits in the "server" rack, labelled **HHS-Docker-Svr** and accessible on the LAN as `hhs-docker-svr.local`.
- The OS is Ubuntu version 24.04 LTS.
- As its hostname implies, the machine is intended to be a host for several docker instances as required.
	- It also provides an `nginx` instance acting as a reverse proxy to various internal web-based services.
- Its IP address is allocated by the router, using DHCP.
	- It has a reserved IP address of `192.168.2.2
- Its hostname is `hhs-docker-svr`
	- It responds as `hhs-docker-svr.lan` (if  `avahi-daemon` is running).
- Initially, just one user was set up: `hhs-admin`, with [its password in our vault](Password%20vault.md). 
	This user has `sudo` access.
- SSH service is enabled to provide admin access from other local machines. Authorized_keys can be added as required for convenience.
- `avahi-daemon` and `avahi-utils` are installed (using `apt`). 
	This enables an internal HHS network address of `hhs-docker-svr.local`, and sets us up to do a fiddle to allow addressing as `wiki.local`. See [URL Adjustments](#URL%20Adjustments) below.

## Docker software

### Base Docker

- Installed `docker` and `portainer`. 
- Docker is installed with `apt`, after adding the relevant `apt` config stuff:
```
	curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor \
		-o /usr/share/keyrings/docker-archive-keyring.gpg
	echo "deb [arch=$(dpkg --print-architecture) \
		signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
		$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
	sudo apt update
	sudo apt-get install docker-ce docker-ce-cli containerd.io
	sudo systemctl status docker
	sudo docker run hello-world
```
- Noting that the last bits ("run hello-world" & "status docker") are checks that it worked ok

- Then we installed `portainer`:
```
	sudo docker volume create portainer_data
	sudo docker volume inspect portainer_data  # ( a check step )
	sudo docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always \
		-v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data \
		portainer/portainer-ce:latest
	sudo docker ps # should show it running
```
- Connecting to `https://hhs-docker-svr.local:9443` with a browser should show the portainer web GUI
	- On first connection from a given browser, the browser may warn about insecure connections - this is because the certificate is locally-generated. Over-ride the warning.
	- On first connection after (re)installation, Portainer will ask for an admin user to be created and a password supplied. The password to use is in the [vault](Password%20vault.md). 

- Added the linux user to the `docker` group to avoid the need for `sudo` all the time:
```
	sudo usermod -aG docker hhs-admin
```

## WikMD user
- The docker server was initially set up to host *WikMD*, the first iteration of our Wiki.
- For this, there is a user-id on the docker server as owner of the wiki. This is to avoid permissions issues and to make it easy to work at the filesystem level for backup etc:
	- User is "wiki-user"; password in [the vault](Password%20vault.md).
```
	sudo adduser wiki-user
	# and add it to the "sudo" group
	sudo usermod -aG sudo wiki-user
	sudo usermod -aG docker wiki-user
```

The WikMD software is still in place, running as an archival reference on port `1841`.
<a id="URL_Adjustments"></a>

# URL Adjustments using `mDNS` and `ngnix` reverse proxy

## Aim: Simpler URLs

The above was sufficient to get us up and running, but for convenience sake some networking "adjustments" weere made to provide for simpler URLs.

The basic networking setup gives us page URLs of the form `http://hhs-docker-host:1841/` which is not very memorable. A couple of additions gave us much easier-to-remember addresses:

1. An `mDNS` alias to allow a hostname of the form `wiki.local`
1. A reverse proxy to pass requests on port 80 to whichever actual port the wiki uses, so our address ia simply `http://wiki.local/`

## mDNS alias
We're already running `mDNS`, so it's a simple enhancement to call `avahi-publish-address` to provide an alternate address. The command `avahi-publish-address` has to be running for the address to be active, so we run it as a service. `avahi-publish` was installed as part of `avahi-utils`. The guide for doing this came from a ['Painfully Obvious' post](https://andrewdupont.net/2022/01/27/using-mdns-aliases-within-your-home-network/).

The service file is `/etc/systemd/system/wiki-mdns-cname.service`. Its contents are:

~~~ ini
[Unit]
Description=Avahi/mDNS CNAME publisher
After=network.target avahi-daemon.service
PartOf=avahi-daemon.service

[Service]
User=wiki-user
Group=wiki-user
Type=simple
WorkingDirectory=/tmp/
ExecStart=/usr/bin/avahi-publish-address  -R wiki.local 192.168.2.2
Restart=no
PrivateTmp=true
PrivateDevices=true

[Install]
WantedBy=multi-user.target
~~~


The service is activated by the commands:

~~~ bash
sudo systemctl enable wiki-mdns-cname
sudo systemctl start wiki-mdns-cname.service && sudo journalctl -u wiki-mdns-cname -f
~~~

## Reverse proxy
`Nginx` is used to provide a reverse proxy service. A longer-term aspiration is to use it to provide authenticated access from outside our local network.

`Nginx` was installed with: `sudo apt install nginx` 
and  appropriate config files have been created:
1. `/etc/nginx/conf.d/wiki.conf`, for our *Obsidian*-based Wiki
2. `/etc/nginx/conf.d/wiki-old.conf`, for our original *WikMD*-based Wiki

These config file are `included` by the `nginx` `/etc/nginx/nginx.conf` file. 
Changes can be checked with the command:

~~~ bash
sudo systemctl restart nginx && sudo journalctl -u nginx -f
~~~

### The `nginx` config files
1. `/etc/nginx/conf.d/wiki.conf`:
	``` nginx
	server {
		listen 80;      # for IPv4
		listen [::]:80; # for IPv6
		listen 7008;    # the forwarded port
		listen [::]:7008;
		http2 on;
		listen 443 ssl;
		listen [::]:443 ssl;
		
		ssl_certificate  /etc/letsencrypt/live/wiki.hobarthackerspace.org.au/fullchain.pem;
		ssl_certificate_key  /etc/letsencrypt/live/wiki.hobarthackerspace.org.au/privkey.pem;
		
		server_name wiki.local wiki.hobarthackerspace.org.au;
		access_log /var/log/nginx/wiki.log;
		
		location / {
			proxy_pass https://publish.obsidian.md/hhs-wiki/$request_uri;
			proxy_ssl_server_name on;
			proxy_set_header Host publish.obsidian.md;
			# Launtel DNS server addresses:
			resolver 203.12.12.12 [2404:e80::1337:af];
		}
	}
	```
	
2. `/etc/nginx/conf.d/wiki-old.conf`:
	``` nginx
	server {
		listen 80;      # for IPv4
		listen [::]:80; # for IPv6
		#  http2 on;
		#  listen 443 ssl;
		#  listen [::]:443 ssl;
		server_name wiki-old.local wiki-old.hobarthackerspace.org.au;
		access_log /var/log/nginx/wiki-old.log;
		
		location / {
			proxy_pass http://hhs-docker-svr.local:1841;
		
			proxy_set_header Host            $host;
			proxy_set_header X-Real-IP       $remote_addr;
			proxy_set_header X-Forwarded-For $remote_addr;
			proxy_http_version 1.1;
		
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
			proxy_cache_bypass 1;
			proxy_no_cache 1;
		
			port_in_redirect on;
		}
	}
	```
