(This is a work in progress. Brian is using it initially to capture notes -- it will get filled out over time.)

# URLs for access
Access is only available using `https:` addresses. There are several alternates:

- [homeassistant.hobarthacker.space](https://homeassistant.hobarthacker.space/)
- [homeassistant.hobarthackerspace.org.au](https://homeassistant.hobarthackerspace.org.au/)
- [homeassistant.hobarthackerspace.org.au:8123](https://homeassistant.hobarthackerspace.org.au:8123/)

The `:8123` port works but isn’t normally necessary.
Redirection to the relevant port is done via an `nginx` reverse proxy.

We use a *`Let's Encrypt`* SSL certificate. [See below](#Let's%20Encrypt%20SSL%20certificate)

# Video cameras
## Hikvision NVR
Most of the cameras are (currently ) connected to a Hikvision NVR. IP address is [192.168.2.6](http://192.168.2.6). The password is in [the password vault](Password vault).

Home Assistant doesn't seem to have a plugin; the recommendation is to use [iVMS-4200](https://www.hikvision.com/au-en/support/download/software/ivms4200-series/) instead. This works well, even on a Mac with the ARM processor:-) From outside, configure it to use IP address office.hobarthackerspace.org.au and port 8385.

## Container inner camera
This is an Axis 214 PTZ. It's connected to the ethernet in the container. Video is captured using `xxx` software, which is running in a contaner on the `docker host` machine.

# Automatic Alarm Setting -- AutoAlarm
The "Alarm panel" system is set to enable the alarm after a period of no movement detected in the Space.
Currently that periold is two hours.
Timing is done by monitoring the PIRs within the building:

- Back corridor
- Kitchen
- Ada Lovelace room
- Tesla room
- Margaret Hamilton room
- Bertrand Russell room

If none of them is activated for the defined period, the alarm panel plug-in is told to arm the building.
This is preceeded by a five-minute "pre-warning" period of progressively more frequent warning beeps and chatter from the alarm system.

When the alarm is set, an MQTT message is posted (to `security/alarm/message`) to report this. The door Raspberry Pi logs it and passes it on to the relevant message systems. 

## Configuration of the AutoAlarm
This is done with a Home Assistant helper ([`Movement sensed timer`](https://office.hobarthackerspace.org.au:8123/logbook?entity_id=timer.movement_sensed_timer)) and two Home Assistant automations ([`Movement timer: set`](https://office.hobarthackerspace.org.au:8123/logbook?entity_id=automation.set_movement_timer) and [`Movement timer: timeout`](https://office.hobarthackerspace.org.au:8123/logbook?entity_id=automation.no_movement_timeout)). 
A second Home Assistant helper ([`Alarm pre-warning`](https://office.hobarthackerspace.org.au:8123/logbook?entity_id=input_boolean.alarm_pre_warning)) is set when the "pre-warning" period starts. This is reset whenever movemnet is sensed, to short-circuit the setting of the alarm.

- To alter the timeout duration, change the value that `Movement sensed timer` is set to in `Movement timer: set`.
- To prevent the AutoAlarm from operating for some reason (eg sleepover), simply turn off the "switch" that enables the `Movement timer: timeout` automation. Don't forget to re-enable it!
- If the warnings start to happen, the system should reset whenever movement is sensde by the above set of movement detectors.

<a name="letsencrypt"></a>

# Let's Encrypt SSL certificate
An SSL Certificate has been created for three domains:
- `homeassistant.hobarthackerspace.org.au` ,
- `homeassistant.hobarthacker.space` and
- `office.homeassistant.org.au`
	- The last one is a legacy hold-over from earlier configurations. At some point it will be removed.

We have the `Let's Encrypt` add-on installed to update the certificate. It requires port 80 to be open on some machine in this domain (it doesn't matter which, so long as it listens on port 80 and responds to `HTTP` requests). At present (March 2006) the gateway router routes port 80 to the Homeassistant machine (`192.168.2.42`).

If the certificate doesn't get updated for whatever reason, we can be locked out of the web GUI.
The backdoor way to resolve this is to `ssh` into the box from within our LAN, as `root`: `ssh root@192.168.2.42` and run:
``` bash
ha addons start core_letsencrypt # update the certificates
reboot # certs don't get used until reboot
```

# `Nginx` reverse proxy setup

- The reverse proxy runs on the server `hhs-docker-svr.local`, as does the one for the Wiki.
- Sub-domain names are registered for `homeassistant` on the two base domain names `hobarthacker.space` and `hobarthackerspace.org.au`.
- The *Letsencrypt* certificate for the `wiki` names includes four domain names - the two `wiki` ones and the two `homeassistant` ones.
	- This certificate is generated using `certbot` and uses the *DNS-01* challenge, to avoid needing to have port 80 open on this machine — it’s already in use for the `Letsencrypt` plugin on Home Assistant.
	- At the moment (Mar 2026) this requires manual updating every 90 days. It’s expected that the *DNS-PERSIST-01* challenge will soon be available and this can be automated.
	- The relevant command to create/update the certificate is: 
	``` bash
	sudo certbot -d wiki.hobarthackerspace.org.au \
	    -d wiki.hobarthacker.space -d homeassistant.hobarthackerspace.org.au \
	    -d homeassistant.hobarthacker.space --manual --preferred-challenges dns \
	    certonly; date
	# the date command is there in case you're too fast for domain propogation and have to wait
	```
- There is a `homeassistant.conf` file in the `/etc/nginx/conf.d/` directory which provides a `server-name` directive responding to the two `homeassistant` domain names and linking to the above certificates. This file provides the relevant proxy commands:
``` 
server {
  http2 on;
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name homeassistant.hobarthackerspace.org.au homeassistant.hobarthacker.space;
  access_log /var/log/nginx/home-assistant.log;

  ssl_certificate  /etc/letsencrypt/live/wiki.hobarthackerspace.org.au/fullchain.pem;
  ssl_certificate_key  /etc/letsencrypt/live/wiki.hobarthackerspace.org.au/privkey.pem;

  location / {
    proxy_pass https://homeassistant.hobarthackerspace.org.au:8123;

    proxy_ssl_server_name on;
    proxy_set_header Host homeassistant.hobarthackerspace.org.au;
    proxy_http_version 1.1;

    # Launtel DNS server addresses:
    resolver 203.12.12.12 [2404:e80::1337:af];

    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_cache_bypass 1;
    proxy_no_cache 1;

    port_in_redirect on;
  }
}
```
