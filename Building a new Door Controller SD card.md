# Steps to take to build a new SD card for the door controller

## Goals: 
- Create an SD card that will plug into the current door controller Pi (a *Raspberry Pi Model B+ V1.2* aka Pi 1B+)
- It will boot and run the door controller system, with the currently-working features
- It runs under the `pi` user, from the directory `~/AccessController/`.
	- Note that this is a change from previously, when the directory was called `~/HHSAccessControlV4/`
	- This brings the directory name into alignment with the repository name, in accordance with current standard practice
- It runs the required services:
	- The `tagreader.service`
	- The `pigpiod.service`
- It communicates with  *Home Assistant* within the Space
- It runs the ancillary temperature monitoring & reporting app
- It allows login via `ssh`
	- Other modes of access, such as *Tailscale* or *Raspberry Pi Connect* are not addressed

# Grab a Pi to use as setup machine
- A Pi Zero or any early generation Pi is suitable
	- If you're using a PiZero, it's easier if you have a USB->Ethernet dongle - it'll work with WiFI, but you'll need to disable WiFi later when you have it installed, to avoid possible network issues from the live machine, as it doesn't have a WiFi port at all.
# Acquire the card
- It needs to be a "High Endurance" or similar micro-SD card
- Size isn't important - this isn't a big app or DB.

# Write a basic Raspberry Pi OS image
- Modern releases of *Raspberry Pi OS* are best imaged with *Raspberry Pi Imager*. 
	- This allows configuration in advance and works with all recent releases. 
	- (These notes were written with `trixie` as current release.)
- We're running on an old Pi 1 model B+, so choose the 32-bit version.
- Choose the "minimal" (no desktop) version.
- Set hostname to `door-system`;
	- If you're doing this within the Space you might want to set this to `door-system-new` at first and change it later, so that you can connect to both new & old at the same time
- Set password for user-id `pi` to `spacehackers@#` or whatever is currently in the vault;
- Enable `ssh`;
- Optional:
	- Add an `ssh` key for login from your computer;
	- Set up wifi SSID & passcode for access to the HHS IoT network.

# Boot into the image and update it
- Install the card in your chosen Pi and boot it up
- Connect with `ssh` and run an update:
	- `sudo apt update; sudo apt upgrade -y`
	- This'll probably take a while...
- Reboot, just in case

# Install `git` & `uv`
- `sudo apt install git`
- There's no `apt` repository for `uv` yet, so:
	- `curl -LsSf https://astral.sh/uv/install.sh | sh`
- Reboot again, to ensure paths are set up repeatably

# Pull in the door system code
- Note that you have to have read access to the *GitHub* repository for this.
- Log in to the Pi
- Change to the relevant superior directory (normally `~/`)
- `git clone https://github.com/hobart-hackerspace/AccessController.git`
	- You'll need your *GitHub* user ID and Personal Access Token.
- Change directory to the freshly-cloned directory (`~/AccessController/`)
- Ensure you're in the correct branch of the repository
	- `git checkout python3`

# Create the Python virtual environment (`.venv`)
- Change directory to the freshly-cloned directory (`~/AccessController/`)
- Resolve any Python version issue
	- Check which Python version is native to the distribution you're using:
		- `python --version`
	- Compare that to the one in the repo branch
		- `cat .python-version`
	- If the two are different, check whether `uv` has access to a version that matches the repo
		- `uv python install <repo-python-version>`
	- If that fails, change the version specified in the repo to match the installed one
		- `nano .python-version`
			- (or whatever your favourite CLI editor is)
- Finally, create & populate the virtual environment 
	- `uv sync`

# Test the installed code
- Activate the virtual environment
	- `source .venv/bin/activate`
	- This should show that you're in the virtual environment by changing the shell prompt.
- run the python script
	- `python tag_reader.py`
- That will fail because we've not yet set up the `pigpiod` daemon, but will show that code levels match and required libraries are in place.
- Exit the venv when you're done
	- `deactivate`

# Install `pigpio`
- Raspberry Pi OS `trixie` & later don't include binaries of `pigpio`, so we have to install them ourselves.
	- See the following if you want details:
		https://forums.raspberrypi.com/viewtopic.php?t=392438
		https://github.com/joan2937/pigpio/issues/632#issuecomment-3379034242
- We follow the instructions at:
	https://abyz.me.uk/rpi/pigpio/download.html
	- You'll have to install the Python setup tools first:
		- `sudo apt install python-setuptools python3-setuptools`
	- Ensure you're at the home directory: `cd ~/`
	- Then do the full build-from-source bit:
	``` bash
		wget https://github.com/joan2937/pigpio/archive/master.zip  
		unzip master.zip  
		cd pigpio-master  
		make  
		sudo make install
	```
	- That takes a while, especially on a Pi 1 or Pi Zero  
- It's worth running the library verification tests that are also listed on the `pigpio` download page, especially if you're using a Pi other than the current Pi 1 B+ or a Zero 1
- Start the `pigpio` daemon: `sudo pigpiod`
- Change back into the `~/AccessController/` directory, activate the venv and try again.
- This time it should run to the stage where it's waiting on input from somewhere, and you should get console output like:
```
	PiGPIO enabled.
	Locking Door..
	GPIO setup complete.
	2026-03-10 18:44:32,160 GPIO setup complete.
	2026-03-10 18:44:32,164 System startup: Startup completed
```

# Install services
The system needs to run automatically on boot or power up, so we set up both  `pigpiod` and the python script to run as services under `systemd`.

## The `pigpiod` service
- Building and installing `pigpio` doesn't install the daemon as a service, so we do that manually.
- Before we start, ensure that the daemon is no longer running after any tests:
	- `cat /var/run/pigpio.pid` will reveal a process ID if it's running
	- Kill it: `sudo kill -hup <the process ID no>`
- Copy (using `sudo`) the following contents into a file called `/lib/systemd/system/pigpiod.service`:
``` ini
[Unit]
Description=Daemon required to control GPIO pins via pigpio
[Service]
ExecStart=/usr/local/bin/pigpiod -l
ExecStop=/bin/systemctl kill pigpiod
Type=forking
[Install]
WantedBy=multi-user.target
```
- Enable it as a service, start it and then verify: 
``` bash
sudo systemctl enable pigpiod
sudo systemctl start pigpiod
sudo systemctl status pigpiod
```


## The python script as a service
Python scripts that are set up to run in virtual environments don't inherit the venv when run as a service, so we have to add some detail. See: https://docs.astral.sh/uv/guides/scripts/#running-a-script-with-dependencies for full background.

In the repository there are two files which do the work for us: `script_as_service.sh` and `tagreader.service`.

- `script_as_service.sh` provides the additional arguments required by `uv` to connect to the dependency libraries that have been installed in the venv.
- `tagreader.service` is the file that `systemd` uses to construct our service.

To install the service, we need to copy the `tagreader.service` file to the relevant `systemd` directory and activate it.
```bash
sudo cp tagreader.service /usr/lib/systemd/system/
sudo systemctl enable tagreader
sudo systemctl start tagreader
sudo systemctl status tagreader
```

# Temperature reporter
The system runs a temperature sensor that reports to Home Assistant. 
It uses:
- The `temperature.sh` script file, installed in the home directory and made executable:
	``` bash
	#!/bin/bash
	MQTT='192.168.2.42'
	# or MQTT='homeassistant.local'
	TOPIC='access/system/temperature'
	PAYLOAD=$(vcgencmd measure_temp | egrep -o '[0-9]*\.[0-9]*')
	/usr/bin/mosquitto_pub -r -t "$TOPIC" -m "$PAYLOAD" -h "$MQTT" -u "mqtt" -P 'mqtt'
	```
- an MQTT client. 
	- The script uses `mosquitto_pub`.
	- Install this with: `sudo apt install mosquitto-clients`
- Trigger it with a user-level `cron` job:
	```cron
	# m h  dom mon dow   command
	*/5 * * * * /home/pi/temperature.sh
	```

# Git  user details
We sometimes need to commit changes to `git` that we've made on the working system.
For this, `git` requires details of the user to add to the commit log. The `pi` user is shared, so we use shared client details:
``` bash
git config user.email "committee@hobarthackerspace.org.au"
git config user.name "Hackerspace Coder"
```

# Adding the working files

The above should be sufficient to build from scratch a working door controller SD card, which is almost ready to be installed into the in-service Pi in place of the running one. But it's missing a few major items which are excluded from the *GitHub* repository:
- Members database
	- `~/AccessController/members.db`
	- Vital for RFID authentication
- Database archive directory
	- `~/AccessController/DBarchive/`
	- Useful as a backup after we add or change members records on the live database.
	- Create an empty one
- Historic log records
	- `~/AccessController/access_log.log`
	- Bringing in the old one(s) provides continuity across a change inSD card or release version.

These can be copied from the working system (if it's still available) or from the saved copies on our Microsoft OneDrive cloud storage. 

- Database copies are in:
	- `Committee - Documents` → `Systems` → `Door controller `→ `db archive`
- Log copies are in:
	- `Committee - Documents` → `Systems` → `Door controller` → `access log archive`

# Disabling WiFi
When the SD card is initially imaged with *Raspberry Pi Imager* it is set up with WiFi enabled. The existing Pi 1 B+ has no WiFI hardware and it is cleaner to disable it on the OS side as well.

Disable WiFI by changing `/boot/firmware/config.txt` by adding after:
	
``` inf
# Additional overlays and parameters are documented
# /boot/firmware/overlays/README
```

- the lines:
``` inf
# Disable WiFi & Bluetooth (BWM 2026-03-08)
dtoverlay=disable-wifi
dtoverlay=disable-bt
```

# Reserve the IP address on the Router
The system on the Pi 1 B+ has an IPv4 address reserved on the Router of  `192.168.2.125`.
This is reserved for a MAC address that is not actually that of the board, due to some historic complications that were resolved in software on the Pi because our old router was primitive.

We now have a new, smarter router, so, in rebuilding the SD card we should fix this, letting the Pi keep its hardware MAC and assigning a normal reserved IP address in the router.